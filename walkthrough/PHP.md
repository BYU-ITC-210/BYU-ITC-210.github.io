---
title: PHP Walkthrough
---

PHP is a popular language for developing web applications. As of 2023, there are more websites built using PHP than all other frameworks combined (see [w3techs.com](https://w3techs.com/technologies/overview/programming_language)). It forms the basis of popular content management systems like WordPress and Drupal and it's a core piece of the Linux-Apache-MySQL-PHP (LAMP) stack. Newer frameworks may scale better or be more efficient to program but PHP will remain important for a long time.

This walkthrough will get you started with the basic features of the PHP language. For more details you can consult [the official PHP Manual](https://www.php.net/manual/en/).

## Setup

You will need a server with a PHP interpreter. For this walkthrough you will use a prebuilt Linux Docker container running Apache and PHP. You should already have Docker Desktop installed on your computer. Follow the instructions for your platform. For Windows computers it's best to use Docker Desktop on WSL2.

Create an empty folder for your workspace. Download [PHPWalkthrough.zip](PHPWalkthrough.zip) and unpack it into that folder.

Open a command line, change directories to the folder where you unpacked the .zip and enter the following command:

```
docker compose up
```

The web server will be on port 4000 and the page you'll be working with is hello.php. So, browse to [http://localhost:4000](http://localhost:4000).  You will see a simple starting page.

## The Basics

Three basic principles:
* PHP code is embedded HTML. The beginning of a PHP code segment is delimited by `<?php` and the end is `?>`.
* PHP Variables start with `$` and, like most languages, values are assigned to variables using the equals sign. Variables are weakly typed. That is, a variable can take a value of any type. You do not need to declare variables before you use them.
* The `echo` command will write an expression to the output, which is the HTML being sent to the browser.
* PHP statements end with a semicolon.

Let's put this to work. Open `hello.php` from your working directory. It should look like this:

```
<html>
<body>
    <h1>Hi!</h1>
    <p> my friend!</h1>
</body>
</html>
```

We will set a variable to the user's name and use it in the HTML. Add PHP code to `hello.php` like this:

```
<?php $my_name = "Susan"; ?>
<html>
<body>
    <h1>Hi!</h1>
    <p><?php echo $my_name ?> my friend!</h1>
</body>
</html>
```

Refresh the page in your browser and you will see that "Susan" has been greeted as a friend.

Right-click on the page and select "View Source". Unlike JavaScript, PHP code executes on the server so you will just see the rendered HTML. You will not see any of the PHP code in what was sent to the browser.

## Includes

The include directive imports another file which is treated as if that text was in the .php file. This can be used to share code and content. For example, an include file could be used to put a common header on every page.

At the beginning of your file, remove `<?php $my_name = "Susan"; ?>` and replace it with `<?php include 'variables.php'; ?>`.

Create a file called `variables.php` and put the following in it:

```
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

```
    <p>
    <? echo 'Single quoted: $my_name' ?><br/>
    <? echo "Double quoted: $my_name" ?><br/>
    <? echo 'Concatenated: ' . $my_name ?><br/>
    </p>
```

Refresh your page and you'll see that the first string gives the name of the variable while the other two give its value. [Read more about PHP strings here.](https://www.php.net/manual/en/language.types.string.php)

Notice that we've reduced `<?php` to just `<?`. The syntax was designed to support other scripting languages but it defaults to PHP so we can just leave that part out.

## Loops and Conditionals

Any HTML that appears within a [loop](https://www.php.net/manual/en/control-structures.for.php) will be repeated in the output for each iteration of the loop. Any HTML that appears within a [conditional](https://www.php.net/manual/en/control-structures.if.php) will only appear if the condition is true.

Add this code to your `hello.php` file somewhere within the `<body>`.

```
    <p>
    <? for ($x = 0; $x<20; $x++) { ?>
        <? echo $x ?>
        <? if ($x % 2 == 0) { ?>
            <span>is even.</span>
        <? } ?>
        <br/>
    <? } ?>
    </p>
```

Note: As with C and C-related languages, `%` is the [modulo operator](https://www.php.net/manual/en/language.operators.arithmetic.php) so `$x % 2` will result in zero if the value is even.

Refresh the page and it will list the numbers from 0 to 19 indicating which ones are even.

In this example, all PHP sections are in one line. However, PHP can span multiple lines and you can use `echo` to write HTML directly. the above example could be rewritten like this:

```
    <p><?
    for ($x = 0; $x<20; $x++) {
        echo $x
        if ($x % 2 == 0) {
            echo ' <span>is even.</span>';
        }
        echo '<br/>'
    }
    </p>
```

## Numeric Arrays

A numeric array contains values that are accessed by position. They are declared using the `array()` function.

To your `variables.php` file, add this line:

```
$a = array("John", "Paul", "George", "Ringo");
```

Now let's greet them all. Somewhere in the `<body>` of your `hello.php` file add the following:

```
    <p>
    <? for ($x=0; $x<count($a); $x++) {
        echo "$a[$x]!<br/>"
    <? } ?>
    </p>
```

Refresh to "[Meet the Beatles!](https://en.wikipedia.org/wiki/Meet_the_Beatles!)".

You can use [foreach](https://www.php.net/manual/en/control-structures.foreach.php) instead of a counter to enumerate the contents of a numeric array. Replace the previous code with this:

```
    <p>
    <? foreach ($a as $value) {
        echo "$value!<br/>";
    <? } ?>
    </p>
```


## Associative Arrays

An [associative array](https://www.php.net/manual/en/language.types.array.php) associates names with values. To your `variables.php` file add this line:

```
$car = array('make'=>'MINI', 'model'=>'Cooper', 'year'=>2004);
```

Now, add the following somewhere in the `<body>` of your `hello.php`.

```
    <p>
        Make: <? echo $car['make'] ?><br/>
        Model: <? echo $car['model'] ?><br/>
        Year: <? echo $car['year'] ?><br/>
    </p>
```

Technically, PHP doesn't distinguish between numeric and associative arrays. You can access all arrays numerically as the members of an array always are ordered. Its just that numeric arrays usually leave off the key. Some examples are below. For details, see the official [PHP Array Reference](https://www.php.net/manual/en/language.types.array.php).

To demonstrate numerical access, use the following code to access the members of `$car` numerically:

```
    <p>
    <? for ($x=0; $x<count($a); $x++) {
        echo "$a[$x]<br/>";
    <? } ?>
    </p>
```

You can use `foreach` to enumerate an associative array and get both keys and values:

```
    <p>
    <? foreach ($car as $key => $value) {
    echo "$key: $value <br/>
    } ?>
    </p>
```

## Superglobal Arrays

PHP sre-defines nine [Superglobal Arrays](https://www.php.net/manual/en/language.variables.superglobals.php) which are associative arrays with special handling or contents:

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

```
    <p>
        <? var_dump($_SERVER); ?>
    </p>
```

## Sessions

Values placed in the `$_SESSION` superglobal array will be preserved between pages on the same web application. The session is managed using storage on the server linked to a cookie sent to the browser. We'll discuss the benefits and problems of sessions, in a future class. For sessions to work, you must call `session_start()` at the beginning of the page. This must be among the first operations - before any content is generated.

At the beginning of your `hello.php` page, before even the `include` statement insert the following:

```
begin_session();
```

Anywhere in the body of the page add the following:

```
    <p><?
    $counter = $_SESSION['counter'];
    $counter = $counter + 1;
    echo "You have visited this page $counter times in this session."
    ?></p>
```

## Other Useful Stuff

PHP is object oriented with objects, classes, methods, and so forth. In this class we won't be creating our own classes but we will use objects to connect to databases.

To call a method on an object *instance* use the `->` operator like this:

```
myobject->mymethod();
```

To call a static method on an object class *class* use the `::` operator like this:

```
myclass::mystaticmethod();
```

