---
title: Cross-Site Request Forgery (CSRF) Walkthrough
---

For this walkthrough we'll be using the **RestSpace** server. The same one we used to demonstrate REST interfaces in a previous walkthrough. In addition to the regular login, **RestSpace** has alternative logins that are either limited or vulnerable in various ways.

## A Naive REST Server

* Browse to [https://restspace.dicax.org/c/callapi](https://restspace.dicax.org/c/callapi).

This is a handy page that lets us compose and call REST APIs sort of like a lightwight version of [Postman](https://www.postman.com/).

* Click on the `Login` template.
* Change the URL to `https://restspace.dicax.org/api/login-n`. This gives you the **naive** login method.
* Set the username to some name and the password to that same name plus `-9455`.

> **RestSpace** lets you create new accounts on the fly so long as the password follows that pattern. Using such a pattern would not be secure on a real website. But **RestSpace** makes accounts expire with the session so there's not a real risk here.

* Click `Send` and you will successfully log in using a cookie. You can see that cookie in the "SetCookie" response header.
* Click the `Create` template and create a car to add to the database. Edit the body of the request (to customize the car) and click `Send`. If you want, create another item for the database. It doesn't have to be a car, any valid JSON object will work.
* Click `Read` to read all of your items from the database.

So, everything works. What make this a naive server? Well, let's try calling this from a different host (a.k.a. origin)?

* Browse to [https://byu-itc-210.github.io/callapi-RestSpace](https://byu-itc-210.github.io/callapi-RestSpace). This is the same page content as the one we were using, but it's hosted on a different server.
* Like before, click on the `Login` template but change the URL to `login-n` to use the naive login mode.
* Enter valid credentials (password is the same as the username but with a `-9455` suffix).
* Click `Send` and ... you get an error.
* Open the console to see what the error is.

You see an error something like this:
```
Access to fetch at 'https://restspace.dicax.org/api/login-n' from origin 'https://byu-itc-210.github.io' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.

Failed to load resource: net::ERR_FAILED
```

This happens way too often. You create a server, it works, then you try it cross-origin and you suddenly get CORS errors. "What is CORS? And why should I care?"

CORS stands for Cross-Origin Request Security. It's a web standard intended to prevent unauthorized access. Since it's enforced by the browser, other applications such as **Postman** or **curl** don't generate CORS errors so it can be a surprise, late in the development process when a CORS error arises.

An "Origin" to the browser is the first part of the URL, including the protocol, the host name, and the port (if any). In this case we are dealing with two orgins. The origin of the server is `https://restspace.dicax.org`. The origin of the client is `https://byu-itc-210.github.io`. Since the origins aren't the same, the browser determines that this is a *cross-origin* request.

* Click on the `Network` tab in the developer tools.
* Click `Send` again to see what the browser is actually doing.

You will see that the browser sent an OPTIONS request to the server. This is known as a "CORS Preflight" request. When calling *cross-origin* the browser will first send a preflight request to find out if the server accepts such requests. In this naive case, the programmer didn't know about CORS and returned a 404 error. Without a valid response, the browser blocked JavaScript from completing the `fetch()`.

## A Permissive REST Server

> Important! For this segment of the walkthrough to work, you must enable third-party cookies. As of 2024, most browser still permit third-party cookies by default but an increasing number are disabling them and nearly all browsers allow the user to disable third-party cookies. If want to selectively enable third-party cookies, this demo requires them to be enabled on `restspace.dicax.org`, `byu-itc-210.github.io` and `byujekylldemo.github.io`.

* Browse to [https://byu-itc-210.github.io/callapi-RestSpace](https://byu-itc-210.github.io/callapi-RestSpace).
* Click on the `Login` template. This time change the URL to "login-p" for "permissive."
* Enter a username and password. (Remember the `-9455` password suffix.)
* Click on `Credentials: Include` to cause the `fetch()` to include cookies and other credentials.
* Click `Send`

The login was successful, But notice that there are limited headers that are displayed. That's because the browser limits the headers that are visible in a cross-origin request.

* Open the `Network` tab in the developer tools and click on `Send` again so that we can see the requests and responses.

Now there are two calls to `login-p`. The preflight request with the `OPTIONS` verb and the actual request that performed the login. The preflight request returned `204 No Content` meaning that the request was successful but all returned information is in the headers. There is no body.

The requests headers include the origin for which access is being requested. The response includes the `Access-Control-Allow-Origin` header indicating an authorized origin. You can also see the `Access-Control-Allow-Credentials`, `Access-Control-Allow-Methods` and `Access-Control-Max-Age` headers. The age of 86400 means that this information is good for 86400 seconds which is 24 hours.

* Experiment with the API, add at least one item and see that everything works as expected.
* In doing so, look at the network tab to see preflight requests on each URL.

So, everything is OK, right? Wrong!

* Browse to another copy of `callapi`. This one is at [https://byujekylldemo.github.io/callapi](https://byujekylldemo.github.io/callapi).
* For the URL, enter `https://restspace.dicax.org/api/items`.
* Click `Credentials: Include`
* Click `Send`

Whoah! An entirely different domain successfully called the API and got results back! How did that happen?

* Try something else. Use the `Create` or `Update` templates to make changes to the database. Again, it succeeds!

It seems that one site's connection to `restspace` got hijacked by the other.

When the browser logged into `https://restspace.dicax.org` from the `https://byu-itc-210.github.io` origin, `restspace` dropped a cookie on the browser as a credential for the session. But, that cookie is tied to the server that created it, `restspace`. It's not connected in any way to the `https://byu-itc-210.github.io` origin. So, when another origin, `https://byujekylldemo.github.io` makes a request to that API, the browser goes ahead and sends the cookie right along.

This attack is known as **Cross-Origin Request Forgery (CSRF)** and it's serious! Imagine if you log into a bank. And then you browse to some other website unaware that the site is malicious. That other site could make API calls to your bank and retrieve sensitive information or transfer money.

CORS is intended to protect against CSRF. But a naive implementation of CORS on the server can completely erase that protection.

## A Secure Cross-Site REST Server

* Browse to [https://byu-itc-210.github.io/callapi-RestSpace](https://byu-itc-210.github.io/callapi-RestSpace).
* Click on the `Login` template. This time change the URL to "login-c" for "cookie".
* Enter a username and password. (Remember the `-9455` password suffix.)
* Click on `Credentials: Include` (it's required for cross-origin requests)
* Click `Send`

Look at the cookie that was returned in the headers. At the beginning it includes "o=" and the name of an *origin*. The server has included the origin in the cookie and it will enforce that only requests from the same origin will be accepted.

You can also examine cookies in the Application tab of the developer tools.

* Create and read objects to show that the API works as expected.

Now let's switch to the alternate domain, [https://byujekylldemo.github.io/callapi](https://byujekylldemo.github.io/callapi). Try reading content and you get `401 Unauthorized`.

You can use the `network` tab in the developer tools to examine the requests and responses. In doing so you'll find that the browser did present the cookie. But, since the origin didn't match the browser rejected the cookie.

To really push things, you could use the `application` tab to edit the cookie and change the origin in the cookie to `https://byujekylldemo.github.io`. If you do so, you'll find that it still fails because now the digital signature that's on the cookie doesn't match.

This is a huge improvement. And it finally achieves the security level we were seeking. Indeed, this was state of the art as of roughly 2019. But it still has a problem. It relies on third-party cookies. More and more browsers are blocking third-party cookies for privacy reasons.

## Authentication Tokens

* Browse to [https://byu-itc-210.github.io/callapi-RestSpace](https://byu-itc-210.github.io/callapi-RestSpace).
* Click on the `Login` template. This time leave the URL alone because the default login uses secure tokens.
* Enter a username and password. (Remember the `-9455` password suffix.)
* You can leave `Credentials: Include` off because header tokens don't require them.
* Click `Send`

This time the server doesn't send a cookie at all. The authentication token is returned in the JSON and also in a header labeled `Authentication-Info` with a value of `Bearer-Update` followed by the token. The `Authentication-Info` header is standardized as is the syntax of its contents. But my use of the `Bearer-Update` instruction is an enhancement, consistent with existing standards.

You'll see that the JavaScript of `callapi` is smart enough to pick up the token from the `Authentication-Info` header and put it in the `Authorization: Bearer` field in the upper part of the form.

* Create, Read, and Update items in the database to make sure everything works.

Notice that as you perform operations, the token gets updated through the `Authentication-Info: Bearer-Update` header. Each time it updates the expiration date embedded in the token to extend the session timeout.

Now lets see if it is protected against cross-origin use.

* Copy the token out of the `Authorization: Bearer` field.
* Browse to [https://byujekylldemo.github.io/callapi](https://byujekylldemo.github.io/callapi)
* Paste the token into the other site's `Authorization: Bearer` field.
* Try to read or update.

The token is rejected because the origin doesn't match.

The Authentication Token method has several advantages. Most important is that it enables secure cross-origin requests even when third-party cookies are disabled. Another advantage is that it doesn't use cookies. So, you don't need a cookie authorization banner even if you're developing for an international company.

On the other hand, it requires that you manage the tokens yourself, in JavaScript while the browser will manage cookies for you. And if you want to maintain a session token across multiple browser tabs you have to manage that carefully. [This article](https://blog.guya.net/2015/06/12/sharing-sessionstorage-between-tabs-for-secure-multi-tab-authentication/) discusses that in detail.

In our labs, we are keeping both the REST API and the application on the same server. In that context, CORS headers aren't required and the existing of CORS on the browser will protect against Cross-Site Request Forgery. But for anything that involves cross-origin requests, Secure Browser Tokens are the best option.
