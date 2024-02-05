---
title: PHP Walkthrough
---

PHP is a popular language for developing web applications. As of 2023, there are [more websites built](https://w3techs.com/technologies/overview/programming_language) using PHP than all other frameworks combined. It forms the basis of popular content management systems like MediaWiki, WordPress, Moodle, and Drupal and it's a core piece of the Linux-Apache-MySQL-PHP (LAMP) stack. Newer frameworks may scale better or be more efficient to program but PHP will remain important for a long time.

This walkthrough will get you started with the basic features of the PHP language. For more details you can consult [the official PHP Manual](https://www.php.net/manual/en/).

## Setup

You will need a server with a PHP interpreter. For this walkthrough you will use a prebuilt Linux Docker container running Apache and PHP. You should already have Docker Desktop installed on your computer. Follow the instructions for your platform. For Windows computers it's best to use Docker Desktop on WSL2.

Create an empty folder for your workspace. Download [PHP-Walkthrough.zip](PHP-Walkthrough.zip) and unpack it into that folder.

Open a command line, change directories to the folder where you unpacked the .zip and enter the following command:

```
docker compose up
```

The web server will be on port 4000 and the page you'll be working with is hello.php. So, browse to [http://localhost:4000/hello.php](http://localhost:4000/hello.php){:target="_blank"}.  You will see a simple starting page.

Later, when it's time to shut the container down. Return to that command line, press Ctrl-C to stop the server, and enter `docker compose down` to stop the server.

## The Basics

Three basic principles:
* PHP code is embedded in HTML and interpreted on the server side. The beginning of a PHP code segment is delimited by `<?php` and the end is `?>`.
* PHP Variables start with `$` and, like most languages, values are assigned to variables using the equals sign. Variables are loosely typed. That is, a variable can take a value of any type. You do not need to declare variables before you use them.
* The `echo` command will write an expression into the HTML being sent to the browser.
* PHP statements end with a semicolon.

Let's put this to work. Open `hello.php` from your working directory. It should look like this:

```php
<!DOCTYPE html>
<html>
<head>
    <title>PHP Walkthrough</title>
</head>
<body>
    <h1>Hi!</h1>
    <p> my friend!</p>
</body>
</html>
```

We will set a variable to the user's name and use it in the HTML. Add PHP code to `hello.php` like this:

```php
<?php $my_name = "Susan"; ?>
<!DOCTYPE html>
<html>
    <title>PHP Walkthrough</title>
<head>
</head>
<body>
    <h1>Hi!</h1>
    <p><?php echo $my_name; ?>, my friend!</p>
</body>
</html>
```

Refresh the page in your browser and you will see that "Susan" has been greeted as a friend.

Right-click on the page and select "View page source". Unlike JavaScript, PHP code executes on the server so you will just see the rendered HTML. You will not see any of the PHP code in what was sent to the browser.

## Includes

The include directive imports another file which is treated as if that text was in that location of the .php file. This can be used to share code and content. For example, an include file could be used to put a common header on every page. In our case we will use it to load data into variables.

At the beginning of your file, remove `<?php $my_name = "Susan"; ?>` and replace it with `<?php include 'variables.php'; ?>`.

Create a file called `variables.php` and put the following in it:

```php
<?php

$my_name = 'Luna';

?>
```

In your browser, refresh the page and see that Luna is now being greeted.

## Quotes

* A string in single quotes (') is a simple string of text.
* A string in double quotes (") will replace any embedded variables with the contents of the variable.
* The period or dot operator (.) will concatenate strings.

Add this code to your `hello.php` file somewhere in the `<body>`.

```php
    <p>
    <? echo 'Single quoted: $my_name'; ?><br/>
    <? echo "Double quoted: $my_name"; ?><br/>
    <? echo 'Concatenated: ' . $my_name; ?><br/>
    </p>
```

Refresh your page and you'll see that the first string gives the name of the variable while the other two give its value. [Read more about PHP strings here.](https://www.php.net/manual/en/language.types.string.php)

If you send multiple arguments, separated by commas, to the [echo](https://www.php.net/manual/en/function.echo.php) command it will write them out in sequence. This will have the same result as concatenation but it's slightly more efficient.

```php
    <? echo 'Multiple: ', $my_name, '<br/>'; ?>
```

Notice that we've reduced `<?php` to just `<?`. The syntax was designed to support other scripting languages but it defaults to PHP so we can just leave that part out.

## Loops and Conditionals

Any HTML that appears within a [loop](https://www.php.net/manual/en/control-structures.for.php) will be repeated in the output for each iteration of the loop. Any HTML that appears within a [conditional](https://www.php.net/manual/en/control-structures.if.php) will only appear if the condition is true.

Add this code to your `hello.php` file somewhere within the `<body>`.

```php
    <p>
    <? for ($x = 0; $x<20; $x++) { ?>
        <? echo $x; ?>
        <? if ($x % 2 == 0) { ?>
            <span>is even.</span>
        <? } ?>
        <br/>
    <? } ?>
    </p>
```

Note: As with C and C-related languages, `%` is the [modulo operator](https://www.php.net/manual/en/language.operators.arithmetic.php) so `$x % 2` will result in zero if the value is even.

Refresh the page and it will list the numbers from 0 to 19 indicating which ones are even.

In this example, each PHP section composes one line. However, PHP can span multiple lines and you can use `echo` to write HTML directly. the above example could be rewritten like this:

```php
    <p><?
    for ($x = 0; $x<20; $x++) {
        echo $x;
        if ($x % 2 == 0) {
            echo ' <span>is even.</span>';
        }
        echo '<br/>';
    }
    ?></p>
```

## Numeric Arrays

A numeric array contains values that are accessed by position. They are declared using the `array()` function.

To your `variables.php` file, add this line. Make sure it appears between the `<?php` and `?>`:

```php
$a = array("John", "Paul", "George", "Ringo");
```

Now let's greet them all. Somewhere in the `<body>` of your `hello.php` file add the following:

```php
    <p><?
    for ($x=0; $x<count($a); $x++) {
        echo "$a[$x]!<br/>";
    }
    ?></p>
```

Refresh your page to "[Meet the Beatles!](https://en.wikipedia.org/wiki/Meet_the_Beatles!)".

You can use [foreach](https://www.php.net/manual/en/control-structures.foreach.php) instead of a counter to enumerate the contents of a numeric array. Replace the previous code with this:

```php
    <p>
    <? foreach ($a as $value) {
        echo "$value!<br/>";
    } ?>
    </p>
```


## Associative Arrays

An [associative array](https://www.php.net/manual/en/language.types.array.php) associates names with values. To your `variables.php` file add this line:

```php
$car = array('make'=>'MINI', 'model'=>'Cooper', 'year'=>2004);
```

Now, add the following somewhere in the `<body>` of your `hello.php` and refresh.

```php
    <p>
        Make: <? echo $car['make']; ?><br/>
        Model: <? echo $car['model']; ?><br/>
        Year: <? echo $car['year']; ?><br/>
    </p>
```

Keys to arrays can be of any native type, not just strings. Indeed, a numeric array is simply an array where the keys are all integers and when you add an element to an array without specifying the key, it takes the maximum existing integer key and adds one.For details, see the official [PHP Array Reference](https://www.php.net/manual/en/language.types.array.php).


You can use `foreach` to enumerate an associative array and get both keys and values:

```php
    <p><?
    foreach ($car as $key => $value) {
        echo "$key: $value <br/>";
    }
    foreach ($a as $key => $value) {
        echo "$key: $value <br/>";
    }
    ?></p>
```

## Superglobal Arrays

PHP pre-defines nine [Superglobal Arrays](https://www.php.net/manual/en/language.variables.superglobals.php) which are associative arrays with special handling or contents:

* **[$GLOBALS](https://www.php.net/manual/en/reserved.variables.globals.php)** Contains all global variables.
* **[$_SERVER](https://www.php.net/manual/en/reserved.variables.server.php)** Data about the currently running serer.
* **[$_GET](https://www.php.net/manual/en/reserved.variables.get.php)** Data sent in the query string of an HTTP request (whether or not the verb is actually GET).
* **[$_POST](https://www.php.net/manual/en/reserved.variables.post.php)** Data sent in the payload of an HTTP POST request.
* **[$_FILES](https://www.php.net/manual/en/reserved.variables.files.php)** All files uploaded by an HTTP POST request.
* **[$_COOKIE](https://www.php.net/manual/en/reserved.variables.cookies.php)** All cookies included in the HTTP request.
* **[$_SESSION](https://www.php.net/manual/en/reserved.variables.session.php)** Session variables (see more info about sessions below).
* **[$_REQUEST](https://www.php.net/manual/en/reserved.variables.request.php)** Combined data from $_GET, $_POST, and $_COOKIE
* **[$_ENV](https://www.php.net/manual/en/reserved.variables.environment.php)** All shell environment variables

The built-in function `var_dump()` is a quick and easy way to output the contents of an array or any other variable. We'll use that to display the contents of the `$_SERVER` array.

Add the following somewhere in the `<body>`:

```php
    <pre>
        <code><? var_dump($_SERVER); ?></code>
    </pre>
```

## Protecting Against XSS Attacks

The [echo](https://www.php.net/manual/en/function.echo.php) command will send the literal text it's provided out to the browser. That means that any HTML tags, including `<script>` in the text will also be output for interpretation by the browser. Thus, if a variable contains something provided by the user, it could become a Cross-Site Scripting (XSS) vulnerability.

In your `variables.php` file, change the value of `my_name` as follows:

```php
$my_name = 'Luna<script>alert("XSS vulnerable!")</script>';
```

Refresh your page and see the alert pop up. It will appear multiple times - once each time the variable is used in your page.

The [htmlspecialchars()](https://www.php.net/manual/en/function.htmlspecialchars.php) function is built into PHP specifically for purposes like this. It encodes special HTML characters - `<` becomes `&lt;`, `>` becomes `&gt;` and `&` becomes `&amp;`. Sanitize your inputs by surrounding each use of `$my_name` with `htmlspecialchars()`. Here's an example:

```php
    <p><?php echo htmlspecialchars($my_name); ?>, my friend!</p>
```

For the double-quoted example, you cannot embed `htmlspecialchars()` within the string. Instead, you can either encode the whole string or encode the variable contents before putting the variable in the string. For example:
```php
    <? $encoded = htmlspecialchars($my_name);
    echo "Double quoted: $encoded"; ?><br/>
```

Another option is to use [strip_tags()](https://www.php.net/manual/en/function.strip-tags.php) which removes HTML tags rather than encoding them. Give that a try by changing one instance of `htmlspecialchars()` to `strip_tags()`.

## Sessions

Values placed in the `$_SESSION` superglobal array will be preserved between pages on the same web application. The session is managed using storage on the server linked to a cookie sent to the browser. We'll discuss the benefits and problems of sessions, in a future class. For sessions to work, you must call `session_start()` at the beginning of the page. This must be among the first operations - before any content is generated.

At the beginning of your `hello.php` page, even before the `include` statement insert the following:

```php
<?
begin_session();
if (!isset($_SESSION['counter']))
{
    $_SESSION['counter'] = 1;
}
else
{
    ++$_SESSION['counter'];
}
?>
```

Anywhere in the body of the page add the following:

```php
    <p><?
        echo 'You have visited this page ' . $_SESSION['counter'] . ' times in this session.';
    ?></p>
```

Refresh to see your new page view counter.

## Other Useful Stuff

PHP is object oriented with objects, classes, methods, and so forth. In this class we won't be creating our own classes but we will use existing classes and objects to connect to databases.

To call a method on an object *instance* use the `->` operator like this:

```php
myobject->mymethod();
```

To call a static method on an object class *class* use the `::` operator like this:

```php
myclass::mystaticmethod();
```

