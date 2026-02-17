---
title: Redirect Walkthrough
---

Redirects are a way of telling the browser, "Not here, over there." In response to an HTTP request you can send the browser to a different URL. There are many uses for redirects. The most common is alternate domain names. For example, if you browse to [http://google.com](http://google.com) you end up at [https://www.google.com](https://www.google.com). Another common use, which you apply in Lab 3, is to accept POST data through one URL and then send the browser to another URL to display information. Single-sign-on systems like the BYU Central Authentication Service ([https://cas.byu.edu](https://cas.byu.edu)) also rely on redirects.

In this walkthrough you will use three forms of redirect:

* HTTP 3xx
* HTML \<meta \> refresh
* JavaScript redirect

## Setup

In order to do HTTP redirects, you need a server-side language. For this walkthrough we will use PHP but, Python, C#, or server-side JavaScript would work equally well. Here, you will use a prebuilt Linux Docker container running Apache and PHP -- the same one we used for the PHP walkthrough. You should already have Docker Desktop installed on your computer. If not, the Docker setup instructions are [here](/InstallWsl2AndDocker).

Create an empty folder for your workspace. Download [Redirect-Walkthrough.zip](Redirect-Walkthrough.zip){: download="Redirect-Walkthrough.zip"} and unpack it into that folder.

Open a command line, change directories to the folder where you unpacked the .zip and enter the following command:

```sh
docker compose up
```

The web server will be on port 4001 so URLs will start with [http://localhost:4001/](http://localhost:4001/)

Later, when it's time to shut the container down. Return to that command line, press Ctrl-C to stop the server, and enter `docker compose down` to stop the server.

## HTTP 3xx Redirects

An HTTP server can redirect the browser using a `3xx` response code and a `Location:` header. The response code tells the browser that it is being redirected and the `Location:` header gives the URL of where the browser should look for the page. There are five redirect response codes. `301` and `302` are legacy values which still work but you should avoid in new code. The new values, `303`, `307`, and `308` each has a specific purpose. Here is the full set:

|-------------------------------------------------|
|Code|Text              |Method        |Permanence|
|-------------------------------------------------|
|301 |Moved Permanently |Change to GET*|Permanent |
|302 |Found             |Change to GET*|Temporary |
|303 |See Other         |Change to GET |Temporary |
|307 |Temporary Redirect|Preserve      |Temporary |
|308 |Permanent Redirect|Preserve      |Permanent |
|-------------------------------------------------|

An early question was whether to preserve the HTTP method on a redirect or switch to `GET`. A common scenario is an HTTP `<form>` that sends data to the server with a `POST`. The server stores the data and then redirects to another page for the user to view. Should the data be POSTed to the new page or should the browser just do a GET. The [original HTTP 1.1 spec](https://www.rfc-editor.org/rfc/rfc2616) was ambiguous on this issue. In practice, most browsers changed the method to `GET` regardless of whether the code was `301` or `302`. But what if your intention is that a browser should submit the data to a different URL?

The issue was resolved in an [update to HTTP 1.1](https://www.rfc-editor.org/rfc/rfc7231). The new spec specifically indicated that browser MAY change to `GET` on `301` and `302` and then introduced `303` which says browsers MUST change to `GET`, and `307` and `308` both of which MUST preserve the method.

*Let's do it:*

Open `start.php` in your code editor. Edit it to contain the following:

```php
<?php
header('Location: target.html', true, 307);
exit();
?>
```

The [header](https://www.php.net/manual/en/function.header.php) function sets an HTTP header to be sent to the browser. It must appear before any contents because all headers must be sent to the browser before any page content. In this example, there is no page content anyway but it's important to know that constraint.

The `true` parameter indicates that this should replace any previously set `Location` header.
The `307` parameter indicates that the status code returned to the browser should be `307 Temporary Redirect`.

Now browse to [http://localhost:4001/start.php](http://localhost:4001/start.php){: target="_blank"}. Your browser will be redirected. You will see `target.html` in the address bar and the contents of `target.html` appear in the body of the browser.

*What difference does permanent redirect make?*

In `start.php` change `307` to `308` and try it again. You won't notice any difference immediately. The difference has to do with caching. A `308` permanent redirect can be cached by your browser. If you bring up the network tab in **developer tools** and *turn caching on* you will find that a repeat visit to `start.php` will redirect to `target.html` without sending any information to the server.

*Why is there not a permanent redirect that changes the request to `GET`?*

Practically, a `301` might do that but the specification is ambiguous and `301` is deprecated. Suppose that a redirect was cached and the method changed from `POST` to `GET`. Then the data to be sent by the `POST` would not be sent anywhere. That is not useful so they did not include it in the updated specification.

## Collect Data and Redirect

A common use of redirects is to collect data from a form and then redirect to a new page that is sensitive to that data. For example, in Lab 3 you collect data when the user fills in a form to create a new task. Once the data has been added to the database then you redirect to the home page that shows all tasks including the newly added one.

Open `form.php` in your code editor. It already has a form for collecting a "statement". The `method` on the form is `POST`. Since there is no `action` element, the form data will be posted to the very same url/page that contains the form. We will add some code to process that submission.

At the beginning of the page add the following:

```php
<?php
session_start();
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $_SESSION['statement'] = $_POST['statement'];
    header('Location: home.php', true, 303);
    exit();
}
?>
```

The first line initiates session management as we discussed in the [PHP Walkthrough](PHP). Then, if the method was `POST` the code takes the statement that was posted and inserts it into the session data. Finally the `header` line redirects to `home.php`.

Now, edit `home.php` to do something with the data.

At the beginning of the page, add the following:

```php
<?php
session_start();
$statement = $_SESSION['statement'] ?? "";
?>
```

This code initiates session handling and copies the statement from the session data into the `$statement` variable. The special `?? ""` syntax says that if `$_SESSION['statement']` is `null` (has not been set) then an empty string will be substituted.

Now edit the `<p>` element to read as follows:

```php
<p>Statement: <? echo htmlspecialchars($statement); ?></p>
```

Note the use of `htmlspecialchars()` which prevents an [XSS vulnerability](https://owasp.org/www-community/attacks/xss/).

With this in place, browse to [http://localhost:4001/form.php](http://localhost:4001/form.php){: target="_blank"}, enter something in the form, and click `Submit`.

## HTML Redirects

There are many cases where you don't have the opportunity to run server-side code but you still need a redirect. A common case is when you reorganize a website and people have bookmarked the old pages. If it's hosted on a static server like [GitHub Pages](https://docs.github.com/en/pages) you won't be able to use HTTP redirects. That's where HTML redirects fit in.

In your code editor, open `togoogle.html`. In the `<head>` element add the following:

```html
<meta http-equiv="refresh" content="0; url=https://www.google.com"/>
```

The first number in `content` indicates the number of seconds to wait before redirecting to the specified URL. Zero means redirect immediately. Browse to [http://localhost:4001/togoogle.html](http://localhost:4001/togoogle.html) and see that you end up at Google.

You might notice that the page flashes up momentarily before the redirect. To prevent that, HTML redirects typically use a blank page unless the wait time is greater than zero. Try setting the wait number to `5` and run it again.

## JavaScript Redirects

Sometimes you need to customize the redirect URL. In this case, redirecting from JavaScript is the most effective approach. Like HTML redirects, JavaScript redirects work without any server-side code. In your code editor, open `tobing.html`. You'll see that it has a form in which the user can enter a query. The attribute, `onsubmit="dosearch(event);"` should call a `dosearch()` JavaScript function.

To the `<script>` element, add the following function:

```html
<script>
function dosearch(event) {
    event.preventDefault();
    let url = "https://www.bing.com/search?q="
        + encodeURIComponent(document.getElementById('query').value);
    window.location.assign(url);
}
</script>
```

The method, `window.location.assign()` causes the browser to open the specified URL. You can also use `window.location.replace()`. The difference is that `assign()` includes the previous page in the history, so if the user clicks the **back** button they will return to it whereas `replace()` leaves the prior page out of the history.

Try it out by browsing to [http://localhost:4001/tobing.html](http://localhost:4001/tobing.html){: target="_blank"}.

## HTTP Redirects through server configuration

You can use server configuration files to perform redirects without any web page at all. To do this, we will have to add another configuration file to the Apache server. This walkthrough is too short for a full tutorial on Apache configuration files. For more information, consult [the official documentation](https://httpd.apache.org/docs/2.4/configuring.html).

First, create a new configuration file called `redirects.conf' with the following contents:

```
Redirect 307 /server /target.html
```

Now, you need to change the docker-compose.yml file to incorporate the new configuration file. The line you will add under `volumes:` is this:

```
    - ./redirects.conf:/etc/apache2/conf-enabled/redirects.conf
```

Here is how the whole docker-compose.yml should look. Make sure you get the indentation right because [YAML](https://en.wikipedia.org/wiki/YAML) is sensitive to that.

```yml
services:
    web:
        image: php:apache
        container_name: apache_php
        ports:
            - 4001:80
        volumes:
            - ./:/var/www/html
            - ./redirects.conf:/etc/apache2/conf-enabled/redirects.conf
```

You have to restart the container for Apache to recognize the new config file. So, from the command-line do the following:
 ```cmd
 Ctrl-C
 docker compose down
 docker compose up
 ```

Now browse to [http://localhost:4001/server](http://localhost:4001/server) and you will be redirected to `http://localhost:4001/target.html`.

In a real server, you would not put configuration files in the same directory as content files. In this case we just simplified things. A more secure solution would be to have separate folders for content and configuration. So the docker-compose file would look like this:

```yml
services:
    web:
        image: php:apache
        container_name: apache_php
        ports:
            - 4001:80
        volumes:
            - ./src:/var/www/html
            - ./apache/redirects.conf:/etc/apache2/conf-enabled/redirects.conf
```

Then you would move all of your source files into a `src` subdirectory and all of your configuration files into an `apache` directory.

For more details, see the [Docker Compose Documentation](https://docs.docker.com/compose/) and the [Apache Configuration Documentation](https://httpd.apache.org/docs/2.4/configuring.html).
