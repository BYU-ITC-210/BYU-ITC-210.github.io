---
title: PHP-SQL Walkthrough Including SQL Injection and XSS Attacks
---

By now you should know the [basics of PHP](PHP-SQL). In this walkthrough we'll use PHP to access a SQL database. We'll see some common security pitfalls and learn to remedy them.

## TL;DR

If you just want to examine the source code we used in class, it's all in the `src` directory of the [PHP-SQL-Walkthrough.zip](PHP-SQL-Walkthrough.zip) package. `index.php` contains the original, insecure code and `secured.php` contains the version with protection against SQL injection ad cross-site scripting (XSS).

## Setup

You will need a web server with a PHP interpreter. The PHP interpreter must have the [mysquli](https://www.php.net/manual/en/book.mysqli.php) package installed so that it can access a SQL database. You will also need a SQL server. As usual, we will use [Docker Desktop](https://www.docker.com/products/docker-desktop/) to host prebuilt containers with everything needed thereby simplifying setup and operation.

Create an empty folder for your workspace. Download [PHP-SQL-Walkthrough.zip](PHP-SQL-Walkthrough.zip) and unpack it into that folder.

### Examining the setup

Open the folder in an editor like [VS Code](https://code.visualstudio.com/). Let's examine the contents to see how the setup works.

**docker-compose.yml**
* Defines two container-services, apache-php on port 80 and mariadb on port 3306
* **apache-php** is built from `Apache-PHP-Mysqli.dockerfile` in the current directory.
    * Port 80 on the container is mapped to port 80 on the local machine. This is the default HTTP port.
    * The `/var/www/html` path where Apache expects to find the web site files is mapped to `./src` which means the `src` subdirectory of the current folder.
* **mariadb** uses the standard `mariadb` container distribution from the [Docker Hub](https://hub.docker.com) library.
* A volume called **mariadb** is created and mapped to the appropriate folder so that the database server can create a database and preserve its contents when the container is stopped and started.

**Apache-PHP-Mysqli.dockerfile**
* Defines a customized Docker container.
* Uses the [php:7.2-apache](https://hub.docker.com/_/php/) container from the Docker Hub library.
* Adds the [mysqli](https://www.php.net/manual/en/book.mysqli.php) extensions for database access.

**.env**
* Defines environment variables, including passwords, that are used by the database server container to secure the database and by the web server to access that database. You will see those environment variables referenced in `docker-compose.yml` and in the PHP source files.

**.src**
* Contains three .php web pages and one SQL script file which we'll use throughout this walkthrough.

### Starting the Servers

Open a command line, change directories to the folder where you unpacked the .zip and enter the following command:

```
docker compose up
```

## Loading Data into the Database

In the `.src` folder examine `create-and-fill-table.sql`. This is a SQL script with three commands:

* `USE` specifies the database to open. In this context, it's unnecessary as the database connection will already default to `cars`. But if you run the script directly using [phpmyadmin](https://www.phpmyadmin.net) it can be helpful.
* `CREATE TABLE` creates the table `cars` with six fields and one primary key index. Note that the `cars` table exists in the `cars` database. They both have the same name.
* `INSERT INTO` loads the `cars` table with 20 records.

Now take a look at `prep.php`. This PHP page simply sends the `create-and-fill-table.sql` script to the SQL server and reports any success or failure messages.

Open a browser and browse to [http://localhost/prep.php](http://localhost/prep.php) to run the database preparation. When you run it the first time it should report that 20 records were created. Running it again should give you an error that the table `cars` already exists.

## The "Cars!" Application Page

The `Cars!` application is contained in `index.php`. It simply lets you query for cars and add new ones.

Browse to [http://localhost](http://localhost). Since `index.php` is the default page name it will run and you'll see a form where you can search for or add cars.

Enter "Ford" in the `Make` field and click `Find`. It should find three cars. Try some other queries. You can consult `create-and-fill-table.sql` to see what values are likely to find results. Try filling in more than one field. For example `Make="Ford"` and `Model="Maverick"`. You'll find that it only uses the first value you enter. That's a limitation of how the PHP page was built.

As you experiment with the searches, you'll see that the page prints the SQL query that it uses right above the table of results.

## Examining the "Find" Code - How it Works

Take a look at the `index.php` page. At the top of the page there's code that collects the environment variables and opens a connection to the SQL server. Next, there's a form that collects the five fields: make, model, year, plate, and owner. It has two `submit` buttons. One for "Find" and one for "Add".

The next part handles the "Find" action. First is this code:

```php
// Choose a field on which to search. Set $fieldName and $value to the value
$properties = array("make", "model", "year", "plate", "owner");
foreach ($properties as $p) {
    if ($_POST[$p]) {
        $fieldName = $p;
        $value = $_POST[$p];
        break;
    }
} 
```

That code iterates through the properties posted from the form. It takes the first field that isn't empty and sets $fieldname to the name of the field and $value to the field's contents. Notice that even though the variables are created and assigned within the `if` statement that their scope is the entire page.

The variable name and value are composed into a sql statement and it's printed to the page.

```php
$sql = "SELECT make, model, year, plate, owner FROM cars WHERE $fieldName = '$value'";
echo '<p>' . htmlspecialchars($sql) . '</p>';
```

Next is the part that connects to SQL and performs the search:

```php
// Connect to the SQL server and perform the search
$conn = new mysqli($mysql_servername, $mysql_user, $mysql_password, $mysql_database);
$stmt = $conn->prepare($sql);
$stmt->execute();
$stmt->bind_result($rmake, $rmodel, $ryear, $rplate, $rowner);
```

The first line creates a connection object. The values for servername, user, password, and database are all retrieved from the environment at the very beginning of the page. `$conn->prepare($sql)` creates a prepared statement. The statement is executed (querying the database) and the results are bound to a set of variables.

Following that, each result is retrieved using `$stmt->fetch()`. The fetch loads the bound variables with the next row and those are filled into the table.

## SQL Injection Vulnerability

Imagine this cars database is being used for parking enforcement. A truck is driving through the parking lot with license plate reading cameras and it comes across this:
![Car with SQL injection license plate](/images/InjectionCar.png){:style="width: 30em;"}

Our application has a SQL injection vulnerability. In the "License plate" field enter `' OR 0='0`. Make sure you get the quotes just right and click `Find`. It returns every car in the database. Potentially the law enforcement computer would be issuing tickets to everyone!

The clue comes in the SQL that was output. The WHERE clause reads `WHERE plate = '' OR 0='0'`.

To prevent this, we need to make sure user input is always interpreted as data and never as code. One way to do that is to encode the user data. In `index.php` change this:

```php
// Compose the query and echo it to the page
$sql = "SELECT make, model, year, plate, owner FROM cars WHERE $fieldName = '$value'";
echo '<p>' . htmlspecialchars($sql) . '</p>';
```

to this:

```php
// Compose the query and echo it to the page
$sql = "SELECT make, model, year, plate, owner FROM cars WHERE $fieldName = '" . $conn->real_escape_string($value) . "'";
echo '<p>' . htmlspecialchars($sql) . '</p>';
```

Remember that dot (period) is the concatenate operator in PHP. This encodes $value so that all quotes and other special characters are escaped. Now test the form with the special license plate value and see what happens.

While `real_escape_string()` works and prevents the exploit, it is not the preferred way to pass arguments. It's better to bind a variable to a parameter. Change the code to this:

```php
// Compose the query and echo it to the page
$sql = "SELECT make, model, year, plate, owner FROM cars WHERE $fieldName = ?'";
echo '<p>' . htmlspecialchars($sql) . '</p>';
```

The question mark indicates a parameter to be bound to the query. Now, add one line following `$conn->prepare($sql)` as follows:

```php
// Perform the search
$stmt = $conn->prepare($sql);
$stmt->bind_param('s', $value);
$stmt->execute();
$stmt->bind_result($rmake, $rmodel, $ryear, $rplate, $rowner);
```

The `$stmt->bind_param('s', $value)` statement binds a string parameter (indicated by 's') to the `$value` variable and it will replace `?` in the query. This way there's no possibility of the user-provided data being treated like code. It is also slightly faster for the database server.

Try the SQL injection exploit again and verify that it's prevented.

## Adding More Cars

The page has another function for adding cars to the database. Fill in all fields with a favorite car and click `Add`. Search for the car using any of the fields and see that it works.

### Examining the code

The functional part is the following code:

```php
// Open a connection to the server and insert a new record
$sql = "INSERT INTO cars (make, model, year, plate, owner) VALUES (?, ?, ?, ?, ?)";
$stmt = $conn->prepare($sql);
$stmt->bind_param('ssiss', $_POST['make'], $_POST['model'], $_POST['year'], $_POST['plate'], $_POST['owner']);
$stmt->execute();
```

Hopefully this is looking somewhat familiar to you by now. First we create a SQL statement with `?` for the five values. We prepare the statement and the bind the five parameters to the values that came in by way of the HTTP post. Finally we execute the query which adds the data to the database. Just like `SELECT`, and `INSERT` statement is vulnerable to SQL injection. The use of a prepared statement and `bind_param()` prevents that from happening. The `'ssiss'` argument to `bind_param)()` indicates that the arguments are, respectively, string, string, integer, string, string.

## Cross-Site Scripting (XSS) Exploit

PHP is notorious for making it easy to generate vulnerable code. There's one vulnerability left here. Add a new car to the database but, for the model, enter this: `<script>alert('Gotcha!')</script>` . Now, search for your new car (by any parameter) and see what happens.

This is another case of "Never trust user-submitted data!" In this case data returned from the database includes JavaScript code which is executed by the browser. For even more fun, try this value `<script>window.location='https://www.byu.edu'</script>` . Now you see why it's called a cross-site scripting attack. You can send users to entirely different websites.

### Fixing the Exploit

The problem is in the code where the result of the query is rendered to the page. Characters that have special meaning in HTML like `<` and `>` need to be encoded into `&lt;` and `&gt;`. In PHP you do this using `htmlspecialchars()`.

Find the code below the comment `// Write the results to a table` and change this code:

```html
<tr><td><? echo $rmake; ?></td>
<td><? echo $rmodel; ?></td>
<td><? echo $ryear; ?></td>
<td><? echo $rplate; ?></td>
<td><? echo $rowner; ?></td></tr>
```

to this:

```html
<tr><td><? echo htmlspecialchars($rmake); ?></td>
<td><? echo htmlspecialchars($rmodel); ?></td>
<td><? echo htmlspecialchars($ryear); ?></td>
<td><? echo htmlspecialchars($rplate); ?></td>
<td><? echo htmlspecialchars($rowner); ?></td></tr>
```

Search for your exploitative cars and see what happens.

## On Your Own

If you search for a car without entering anything in the fields, you get an error. Find a way to return all cars in that situation.
