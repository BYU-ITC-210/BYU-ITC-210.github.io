---
title: Cross-Site Request Forgery (CSRF) Walkthrough
---

For this walkthrough we'll be using the **RestSpace** server. The same one we used to demonstrate REST interfaces in a previous walkthrough. In addition to the regular, mostly secure form, **RestSpace** has three alternative domain names that are limited or vulnerable in various ways.

## A Naive REST Server

* Browse to [https://n.restspace.dicax.org/callapi](https://n.restspace.dicax.org/callapi){: target="_blank"}.

The `callapi` page has a handy interface that lets us compose and call REST APIs sort of like a lightweight version of [Postman](https://www.postman.com/) or [BurpSuite](https://portswigger.net/burp). Behind the scenes, `callapi` uses the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) and so `callapi` shows us exactly how browser code will behave wheras `BurpSuite` deliberately lets you bypass some of the limitations in `fetch`.

* Click on the `Login` template.
* In the request body, set the `username` to some name (make one up) and the password to that same name plus `-9455`. For example, username: `jorg`, password: `jorg-9455`.

> **RestSpace** lets you create new accounts on the fly so long as the password is the username plus `-9455`. Using such a pattern would not be secure on a real website. But **RestSpace** makes accounts expire with the session so there's not a real risk here. Also, each session gets a new account. So, even if you login using the same username, you'll get a different account.

* Click `Send` and you will successfully log in using a cookie. You can see that cookie in the "Set-Cookie" response header.
* Click the `Create` template and create a car to add to the database. Edit the body of the request (to customize the car) and click `Send`. If you want, create another item for the database. It doesn't have to be a car, any valid JSON object will work.
* Click the `Read` template and then click `Send` to read all of your items from the database.

So, everything works. What makes this a "naive" server? Well, let's try calling this from a different host (a.k.a. origin)?

* In another tab, browse to [https://byu-itc-210.github.io/callapi-RestSpace](https://byu-itc-210.github.io/callapi-RestSpace){: target="_blank"}. This is the same `callapi` page as the one we were using, but it's hosted on a different *domain*. That's important because we are experimenting with cross-origin requests.
* Like before, click on the `Login` template but change the URL to `https://n.restspace.dicax.org/login` to use the same naive instance.
* Enter valid credentials, with a different name from the one you used before. (Remember, the password is the username plus a `-9455` suffix).
* Click `Send` and ... you get an error.
* Open the browser console to see what the error is.

You see an error report something like this:
```
Access to fetch at 'https://n.restspace.dicax.org/api/login'
from origin 'https://byu-itc-210.github.io' has been blocked
by CORS policy: Response to preflight request doesn't pass
access control check: No 'Access-Control-Allow-Origin' header
is present on the requested resource. If an opaque response
serves your needs, set the request's mode to 'no-cors' to
fetch the resource with CORS disabled.

Failed to load resource: net::ERR_FAILED
```

If you want, try changing the `mode` to "no-cors" and try again. This time you get a 401 Unauthorized error. (Remember to change the mode back for future calls.)

This happens way too often. You create a server, it works, then you try it cross-origin and you suddenly get CORS errors. "What is CORS? And why should you care?"

CORS stands for [Cross-Origin Request Security](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS). It is a web standard intended to enable authorized access while preventing unauthorized access. Since it is enforced by the browser, other applications such as **BurpSuite**, **Postman** and **curl** don't generate CORS errors. Neither does Python `http.request()`, `fetch()` in Node.js doesn't have CORS security. Only browsers. Therefore, you may be surprised by such an error when CORS problems arise late in the development process.

An **Origin** to the browser is the first part of the URL; including the scheme, the host name, and the port number (if any). In this case we are dealing with two origins. The origin of the server is `https://n.restspace.dicax.org`. The origin of the client is `https://byu-itc-210.github.io`. Since the origins aren't the same, the browser determines that this is a *cross-origin* request.

* Click on the `Network` tab in the developer tools.
* Click `Send` again to see what the browser is actually doing.

You can try the Send with different `mode` settings. Flip between the `Console` and the `Network` tabs to get all of the information. Use the &#x2298; button to clear results between tests so you can better understand what's happening. With the naive server,of the `mode` settings work but the error patterns are different. 

In `(default)` and `cors` modes you will see that the browser sent an OPTIONS request to the server. This is known as a "CORS Preflight" request. When calling *cross-origin* the browser will first send a preflight request to find out if the server accepts such requests. In this naive case, the programmer didn't know about CORS and the server returned a 404 error. Without a valid response, the browser blocked JavaScript from completing the `fetch()`.

## A Permissive REST Server

Suppose you *want* to be able to access your [REST API](https://www.geeksforgeeks.org/rest-api-introduction/) from other servers. The naive server blocks that. To allow cross-origin requests you have to implement the [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) mechanism on your website. Generally, it's best to use an existing library for that. Here you will find out how it works.

> Important! For this segment of the walkthrough to work, you must enable third-party cookies. As of 2024, most browsers still permit third-party cookies by default but an increasing number are disabling them (e.g. [Safari](https://www.apple.com/safari/) and [Brave](https://brave.com/)) and nearly all browsers have an option to disable third-party cookies. If you (like me) disable third-party cookies either enable them for this part of the walkthrough or enable third-party cookies on the following domains: `n-restspace.dicax.org`, `p-restspace.dicax.org`, and `c-restspace.dicax.org`. This is only necessary for the demo. Later in this walkthrough, I will explain how to make cross-site requests work even with third-party cookies disabled.

* Browse to [https://byu-itc-210.github.io/callapi-RestSpace](https://byu-itc-210.github.io/callapi-RestSpace){: target="_blank"}.
* Click on the `Login` template. This time change the URL to `https://p.restspace.dicax.org/login`. This is the "permissive" instance of the RestSpace server.
* Enter a username and password. (Remember the `-9455` password suffix.)
* Make sure "credentials: include" is checked.
* Click `Send`

The login was successful, You may notice that it displays fewer headers than the naive server. And, even though it set a cookie it doesn't show that in the response. The browser limits the headers that are visible in a cross-origin request and, while it stored the cookie, it blocks JavaScript from other origins from accessing it.

> Even the `Application` tab in the developer tools doesn't show the cookie. You would have to open another browser tab at [https://p.restspace.dicax.org/](https://p.restspace.dicax.org/){: target="_blank"} and open the `Application` developer tool to view the cookie.

* Open the `Network` tab in the developer tools and click on `Send` again so that you can see the requests and responses.

Because this is a cross-origin call there are two calls to `login`. The preflight request with the `OPTIONS` verb and the actual request that performed the login. The preflight request returned `204 No Content` meaning that the request was successful but all returned information is in the headers. There is no body.

> Even though the preflight request always goes first, the Network tab often shows the requests in reverse order. This is just a strange quirk of the developer tool.

Look at the preflight request (OPTIONS verb and 204 response). The request headers include the `Origin` which indicates what origin is making the request. The response headers include the `Access-Control-Allow-Origin` header indicating an authorized origin. You can also see the `Access-Control-Allow-Credentials`, `Access-Control-Allow-Methods` and `Access-Control-Max-Age` headers. The age of 3600 seconds means that this information is good for one hour.

* Experiment with the API. Make sure you add at least one item (using the `Create` template) and read all items (using the `Read` template) -- see that everything works as expected.
* In doing so, look at the network tab to see preflight requests on each URL. Assuming you have `Disable cache` checked on the Network tab then you will see a preflight with each `POST`. `GET` requests are considered [simple requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#simple_requests) and don't require a preflight.

So, with cross-origin working everything is OK, right? Wrong!

* Browse to another copy of `callapi`. This one is at [https://misc.dicax.org/callapi-RestSpace](https://misc.dicax.org/callapi-RestSpace).
* For the URL, enter `https://p.restspace.dicax.org/api/items`.
* Click `Send`

Whoah! An entirely different domain successfully called the API, *without logging in* and got results back! How did that happen?

* Try something else. Use the `Create` or `Update` templates to make changes to the database. Again, it succeeds!

It seems that one site's connection to `RestSpace` got hijacked by the other. This is called [Cross-Site Request Forgery (CSRF)](https://owasp.org/www-community/attacks/csrf) and it is a serious vulnerability. In fact, CORS was invented to protect against CSRF but the inadequate implementation of CORS on the *permissive* site did the opposite.

Imagine if you log into a bank. And then you browse to some other website unaware that the site is malicious. Since your browser retains cookies, that other site could make API calls to your bank and retrieve sensitive information or transfer money.

In this case, when the browser logged into `https://p.restspace.dicax.org` from the `https://byu-itc-210.github.io` origin, `restspace` dropped a cookie on the browser as a credential for the session. That cookie is tied to the *origin* that created it, `http://p.restspace.dicax.org`. It's not connected in any way to the `https://byu-itc-210.github.io` origin. So, when another origin, `https://misc.dicax.org` makes a request to that API, the browser goes ahead and sends the cookie right along.

You can try accessing `https://p.restspace.dicax.org` from other copies of `callapi` such as the one from the naive server at [https://n.restspace.dicax.org/callapi](https://n.restspace.dicax.org/callapi) or on the permissive server itself at [https://p.restspace.dicax.org/callapi](https://p.restspace.dicax.org/callapi). In each case, if you set the URL to `https://p.restspace.dicax.org` you get access to the same data.

CORS is intended to protect against CSRF. But a permissive implementation of CORS on the server can completely erase that protection.

## A Secure Cross-Site REST Server

* Browse to [https://byu-itc-210.github.io/callapi-RestSpace](https://byu-itc-210.github.io/callapi-RestSpace){: target="_blank"} (or just go back to that tab).
* Click on the `Login` template. This time change the URL to `https://c.restspace.dicax.org/api/login`. This is the "secure-cookie" instance of the RestSpace server.
* Enter a username and password. (Remember the `-9455` password suffix.)
* Make sure "credentials: include" is checked.
* Click `Send`

Look at the cookie that was returned in the headers. At the beginning it includes "o=" and the name of an *origin*. The server has included the origin in the cookie and it will require that requests come from the same origin as the one recorded in the cookie.

* Create and read objects to show that the API works as expected.

Now switch to the alternate domain in a different tab: [https://misc.dicax.org/callapi-RestSpace](https://misc.dicax.org/callapi-RestSpace){: target="_blank"}. Set the URL to `https://c.restspace.dicax.org` Try reading content (by using the *Read* template) and you get a "401 Unauthorized" error.

You can use the `network` tab in the developer tools to examine the requests and responses. In doing so you'll find that the browser did present the cookie. But, since the origin didn't match, the server rejected the cookie.

To really push things, you could go to [https://c.restspace.dicax.org](https://c.restspace.dicax.org) in another browser tab, use the developer tools `application` tab to edit the cookie and change the origin in the cookie to `https://misc.dicax.org`. If you do so, you'll find that it still fails because now the digital signature that's on the cookie doesn't match.

This is a huge improvement. And it finally achieves the security level we were seeking. Indeed, this was state of the art as of roughly 2019. But it still has two problems.

First, there's only one cookie for `https://c.restspace.dicax.org` regardless of how many applications use the API. Go back to your tab that's on [https://misc.dicax.org/callapi-RestSpace](https://misc.dicax.org/callapi-RestSpace){: target="_blank"}. Use the `Login` template and log in. Add an item or two using the `Create` template and read them out using the `Read` template. All is well. Now go back to your original tab at [https://byu-itc-210.github.io/callapi-RestSpace](https://byu-itc-210.github.io/callapi-RestSpace){: target="_blank"}. A `Read` fails. When you logged in at the new origin, the new origin's cookie replaced the old one and the previous origin was logged out. This could be solved with some clever coding. For example, you could add the origin to the name of the cookie. But there's another problem.

Second, is that this solution relies on [third-party cookies](https://developer.mozilla.org/en-US/docs/Web/Privacy/Third-party_cookies). Many browsers are beginning to block third-party cookies for privacy reasons. 

## Authentication Tokens

* Browse to [https://byu-itc-210.github.io/callapi-RestSpace](https://byu-itc-210.github.io/callapi-RestSpace){: target="_blank"} one more time.
* Click on the `Login` template. This time leave the URL as `https://restspace.dicax.org/api/login` because the default login uses secure tokens.
* Enter a username and password. (Remember the `-9455` password suffix.)
* Click `Send`

This time the server doesn't send a cookie at all. The authentication token is returned in the JSON and also in a header labeled `Authentication-Info` with a value of `Bearer-Update` followed by the token. The `Authentication-Info` header is standardized as is the syntax of its contents. But my use of the `Bearer-Update` instruction is an enhancement. It's consistent with existing standards but not necessarily adopted on other sites.

You'll see that the JavaScript of `callapi` is smart enough to pick up the token from the `Authentication-Info` header and put it in the `Authorization: Bearer` field in the upper part of the form. Other applications may use the value from the JSON body of the response.

* Create, Read, and Update items in the database to make sure everything works.

Notice that as you perform operations, the token gets updated through the `Authentication-Info: Bearer-Update` header. Each update extends the expiration date embedded in the token.

Now lets see if it is protected against cross-origin use.

* Copy the `bearer` token out of the `Authorization:` field.
* Browse to [https://misc.dicax.org/callapi-RestSpace](https://misc.dicax.org/callapi-RestSpace){: target="_blank"} (or just go to that tab).
* Set the URL on that tab to `https://restspace.dicax.org/api/items`
* Paste the token into the other site's `Authorization: Bearer` field.
* Try to read or update.

The token is rejected because the origin doesn't match.

The Authentication Token method has several advantages. Most important is that it enables secure cross-origin requests even when third-party cookies are disabled. Another advantage is that it doesn't use cookies. So, you don't need a cookie authorization banner even if you're developing for an international company.

If you log in from different tabs, whether the from the same or different domains, each has its own Authorization token and so they each get their own session. If you want to share the session across tabs you can store the token in [localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage). That's how many of my applications including the DNS Registrar work.

On the other hand, this requires that you manage the tokens in the front end using JavaScript. And if you want to share a session token across multiple browser tabs you have to manage that carefully. [This article](https://blog.guya.net/2015/06/12/sharing-sessionstorage-between-tabs-for-secure-multi-tab-authentication/) discusses that in detail.

## Other Ways to Prevent CRSF

If you read up on Cross-Site Request Forgery you will find that the most common recommendation is to use [Token-Based Mitigation](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html#token-based-mitigation). In this method, your frontend retrieves one or more single-use tokens from the server. A token must be sent with each call that makes changes and tokens may not be reused. This is a proven system and, prior to browser implementation of CORS, this and the [Double-Submit](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html#alternative-using-a-double-submit-cookie-pattern) pattern were the only options.

The trouble with Token-Based Mitigation and Double-Submit is that the server must keep track of issued tokens in a database of some sort thereby recognizing issued tokens and prohibiting reuse. It's very complicated and consumes extra server resources. If you choose one of these methods I strongly recommend that you adopt a tested and reliable library.

In contrast, the methods used in the secure-cookie and authentication methods in this demo both rely on storing the origin of the login in the authentication token. Thus, calls from other origins are rejected. It is much simpler and equally secure as other methods. However, this method relies on browsers providing the [Origin](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Origin) header which [only became broadly available in 2015](https://caniuse.com/mdn-http_headers_origin). As of 2024 it remains unsupported on 3% of browsers in use. Due to a fail-safe implementation, this application will simply not work on older browsers without Origin headers.

## Additional Resources
* [Common no-cors misconceptions](https://evertpot.com/no-cors/)
* [OWASP Cross-Site Request Forgery Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)

# More Information

The following is not but it's important background if you plan to build a secure back end.

## Security Considerations

While creating a server that manifests CSRF vulnerabilities I have taken care to ensure that it cannot be exploited in dangerous ways. The main protection is that each session gets its own new database. Even if you log in with the same credentials that you or someone else used previously, you will get a new session with a new database. Thus, the CSRF vulnerability is limited to accessing data you entered - not data entered by anyone else.

A second precaution is that data are only retained for 24 hours. This prevents the server from being exploited as a data storage service.

## Token Format
According to the [Authentication: Bearer](https://datatracker.ietf.org/doc/html/rfc6750) specification, the format of a bearer token is opaque to the client. The server can generate any kind of string as it will be the only service to interpret it. Very important, however, is that the token must be secure against tampering or forgery.

The authentication tokens issued by this application are based on [x-www-form-urlencoded](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST) (urlencoded) encoding. More common, and more standardized, are [JWT Tokens](https://jwt.io/). I started using x-www-form-urlencoded tokens when I built the Central Authentication Service for Ancestry.com in 1999. The first official JSON spec was in 2006 and it wasn't standardized until 2013. So, yeah, JWT wasn't an option when I first started generating secure tokens.

**Similarities:** Both tokens use [HMAC](https://en.wikipedia.org/wiki/HMAC) authentication codes to prevent forgery or tampering. Both let you embed arbitrary data such as user IDs, and session information. Both usually have a creation date/time or an expiration date/time. Both are encoded to prevent use of characters that are disallowed in cookies and http headers. Both can be easily parsed and decoded in JavaScript. JWT using [Uint8Array](https://developer.mozilla.org/en-US/docs/Glossary/Base64) and [JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON) classes; urlencoded using [URISearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams).

**Differences:** urlencoded tokens use [URL query string encoding](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams#percent_encoding) whereas JWTs use [Base64](https://en.wikipedia.org/wiki/Base64) encoding. That makes urlencoded tokens easier for humans to read without the assistance of a decoder but they are no less secure. Urlencoded tokens are more compact. JWTs can use signature algorithms other than HMAC.

## Things I discovered along the way

The CORS standards [as documented in MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) are detailed and specific. And yet, there are nuanced and surprising pieces. For example, the [Sec-Fetch-Site](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-Fetch-Site) header is among the newer features, only generally implemented since 2020. ([See current browser support](https://caniuse.com/?search=Sec-Fetch-Site)) Nevertheless, it's quite handy. In RestSpace I originally used it to determine whether an updated cookie (with a new expiration time) should have [Samesite](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie#samesitesamesite-value) set to `none` or `strict`.

The documentation for `Sec-Fetch-Site` give four possible values:
* cross-site
* same-origin
* same-site
* none

The weird value is `same-site`. What constitutes the same site? I assumed that `restspace.dicax.org` and `misc.dicax.org` would be different sites and several less-reliable sources implied this. But [digging deep here](https://developer.mozilla.org/en-US/docs/Glossary/Site) I found that the distinguishing feature between sites is the "registrable domain" which is just "dicax.org."

Despite that definition of *site* my experiments found that setting a cookie to `SameSite=strict` prevents that cookie from being shared between `restspace.dicax.org` and `misc.dicax.org`. In that case, it seems that both addresses must have the *same-origin* which means that the scheme, host name, and port must all match. Thus, I changed my test to check whether origins are the same.