---
title: SQL Walkthrough
---

Structured Query Language (SQL) is a standardized language for creating, updating, and querying relational databases. Variations on SQL are used to query other kinds of databases. SQL has been [standardized](https://blog.ansi.org/2018/10/sql-standard-iso-iec-9075-2016-ansi-x3-135/). Nevertheless, different relational databases have their own dialects. In this walkthrough we will use the [MySQL dialect](https://mariadb.com/kb/en/sql-language-structure/) which is common between the [MySQL](https://dev.mysql.com/) and [MariaDB](https://mariadb.org/) database servers.

## A Fragment of History

In this walkthrough, and in the related labs, we will use [MariaDB](https://mariadb.org/), an popular open source relational database server created by [Michael Widenius](https://en.wikipedia.org/wiki/Michael_Widenius) and supported by a robust community of developers. It based on [MySQL](https://en.wikipedia.org/wiki/MySQL). When Oracle acquired MySQL in 2010, Widenius created the MariaDB fork and it has been enhanced considerably since then. MySQL is named after Widenius' daugher, My, whereas, MariaDB is named after Widenius' younger daughter, Maria.

## Setup

A typical database server does not have a user interface. Rather, it is accessed through its API. Thus, a separate admin tool is required to manage the database including creating databases, loading data, performing queries, optimizing indexes, and so forth. For this walkthrough, we will use two popular [Docker](https://www.docker.com/) containers, one for MariaDB and one for [phpMyAdmin](https://www.phpmyadmin.net) - a web-based administration tool for MySQL and MariaDB written, unsurprisingly, in [PHP](https://www.php.net/).

Create an empty folder for your workspace. Download [SQL-Walkthrough.zip](SQL-Walkthrough.zip) and unpack it into that folder.

Open a command line terminal, change directories to the folder where you unpacked the .zip and enter the following command:

```
docker compose up
```

The web server will be on port 4000 and the page you'll be working with is hello.php. So, browse to [http://localhost:4000/hello.php](http://localhost:4000/hello.php){:target="_blank"}.  You will see a simple starting page.

Later, when it's time to shut the containers down. Return to that command line, press Ctrl-C to stop the server, and enter `docker compose down` to stop the server. If you want to erase the contents of the database and return the server to its original status, enter `docker compose down -v`.

Docker will bring up two servers. **MariaDB** will be listening on it's default port, 3306. But you won't connect directly to that. **phpMyAdmin** is a web server that will be listening on port 8080. In you web browser, open [http://localhost:8080](http://localhost:8080).

The default username and password are set in the **docker-compose.yml** file that's included in the .zip file. The username is "developer" and the password is "password". You may consider changing them in **docker-compose.yml** before launching the containers. Once you enter the username and password you will see the home screen of **phpMyAdmin**.

## Loading a Database Via Script

From **phpMyAdmin** you can run a SQL script, which is a series of SQL commands. For this walkthrough, we have provided a script that will load the **my_movies** database with three tables of data.

In **phpMyAdmin**

1. Click on the **Import** tab.
2. Under **File to import:** click **Choose File**.
3. Browse to the folder where you unpacked "SQL-Walkthrough.zip" and select "MoviesDb_create_and_fill.sql"
4. Scroll to the bottom and click **Import**.

You should see a message, "Import has been successfully finished, 476 queries executed. (MoviesDb_create_and_fill.sql)".

## Browsing the Database Schema

In the left column, you will find a tree view of the database server. Expand the **my_movies** database (if it isn't expanded already), click **actor** and it will display a the contents of the *actor* table. You can do the same to explore the **movie** and **role** tables.

## Simple Queries

Before you can enter a query, you must select a database. By browsing the tables, you have done so and "Database: my_movies" should appear at the top of the page. If not, click **my_movies** in the left column to select the database.

Now click the **SQL** tab.

Let's query the **actor** table. In the query box at the top of the page, enter the following:

```
SELECT * FROM actor
```

Click the **Go** button to run the query.

SQL, is case-insensitive. But, by convention, SQL keywords are given in ALL CAPS. The `SELECT` clause indicates which fields (also known as columns) you wish to retrieve. The `*` asterisk indicates all fields.

The `FROM` clause indicates which table should be searched for the data.

When you perform a query, it hides the query box to maximize the space you have for the results. Click **Show query box** to get the box back on the screen.

Edit your query to retrieve all contents of the **movie** table.

## Limiting Fields

Let's retrieve just the fields we are interested in with the following query:

```
SELECT name, networth FROM actor
```

## Sorting: The ORDER BY Clause

Suppose you want to list actors in order of their net worth. Use the following query:

```
SELECT name, networth FROM actor ORDER BY networth
```

That's interesting, but it sorted by lowest net worth first, and all we see on the screen are those with a "NULL" value, for whom we don't know the net worth. Let's change the sort to descending order.

```
SELECT name, networth FROM actor ORDER BY networth DESC
```

## Aggregate Functions

Suppose you want to know how many actors there are in the database. The `COUNT` function will get you that value.

```
SELECT COUNT(actorid) FROM actor
```

What about counting the number of cities in which actors were born. You can try this:

```
SELECT COUNT(birth_city) FROM actor
```

But that number seems too high. Indeed, it simply returns the count of records that have a non-NULL value for **birth_city**. To get the count you're lookign for, use the `DISTINCT` keyword:

```
SELECT COUNT(DISTINCT birth_city) FROM actor
```

What about a list of cities in alphabetical order.

```
SELECT DISTINCT birth_city FROM actor ORDER BY birth_city
```

Other aggregate functions include `SUM()`, `AVG()`, `MIN()`, and `MAX()`.

## Filtering: The WHERE Clause

The `WHERE` clause lets us filter the results. Suppose we want cities in Germany where actors were born.

```
SELECT DISTINCT birth_city
FROM actor
WHERE birth_country = 'Germany'
ORDER BY birth_city
```

As queries get more complex, it's helpful, but not required, to put each clause on its own line.

## Aggregate Groups: The GROUP BY Clause

So far, we've only used the aggregate function, `COUNT`, across the whole set of results. For example, this will list the number of actors who were born in Germany.

```
SELECT COUNT(actorid) FROM actor WHERE birth_country = 'Germany'
```

But what if we want to know now many actors were born in each city. For this we use the `GROUP BY` clause.

```
SELECT birth_city, COUNT(actorid) AS Count
FROM actor
GROUP BY birth_city
ORDER BY Count DESC
```

Notice the `AS` keyword in this query. It lets us name the aggregate column so that we can use it later in the `ORDER BY` clause. 

And we can still filter by country:

```
SELECT birth_city, COUNT(actorid) AS Count
FROM actor
WHERE birth_country = 'Germany'
GROUP BY birth_city
ORDER BY Count DESC
```

### Bridging Tables: The JOIN operator

All of our queries so far have involved just one table. In our movies database there is a many-to-many relationship between **actor** and **movie**. That connection is made by the **role** table which indicates which role an actor played in a movie.

We'll start by listing all roles played by an actor.

```
SELECT name, character_name
FROM actor
LEFT JOIN role ON actor.actorid = role.actorid
```

When joining two tables, we have to indicate the fields to be matched across the tables. A convention when designing databases is to name the fields to be matched the same.

There are four types of joins in SQL. `LEFT JOIN` will include all entries in the table on the left of the `JOIN` operator but only those in the table on the right that have a match on the left. `RIGHT JOIN` does the opposite. It includes all entries in the table on the right of the operation but only those on the left that have a match to the table on the right. `INNER JOIN` only includes entries where there's a match across tables. `OUTER JOIN` includes all entries in both tables regardless of whether there's a match. However, MariaDB does not directly support outer joins.

Joins can be used with aggregate functions. Suppose we want to know how many roles each actor has played.

```
SELECT name, COUNT(movieid) as Roles
FROM actor
LEFT JOIN role ON actor.actorid = role.actorid
GROUP BY name
ORDER BY Roles DESC, name ASC
```

There are two fiels listed in the `ORDER BY`. The second one, `name` will be used when there's a tie on the first field, `Roles`.

You can join across all three tables to list the movies that each actor has been in.

```
SELECT name, title
FROM actor
LEFT JOIN role ON actor.actorid = role.actorid
LEFT JOIN movie ON role.movieid = movie.movieid
ORDER BY name ASC
```

And, of course, you can filter such a query. For example, just the actors that were born in Germany.

```
SELECT name, title, birth_city
FROM actor
LEFT JOIN role ON actor.actorid = role.actorid
LEFT JOIN movie ON role.movieid = movie.movieid
WHERE birth_country = "Germany"
ORDER BY name ASC

```

## Updating the database: The UPDATE clause

Suppose by some weird international treaty, Berlin was given to Austria. First, let's see which actors would be affected.

```
SELECT name, birth_country FROM actor WHERE birth_city = 'Berlin'
```

Now, let's update the database.

```
UPDATE actor
SET birth_country = "Austria"
WHERE birth_city = "Berlin" AND birth_country = "Germany"
```

Now look again to see the result.

```
SELECT name, birth_country FROM actor WHERE birth_city = 'Berlin'
```

## Addign data: The INSERT INTO clause

Now, suppose that former British Prime Minister, Boris Johnson, acts in a movie. We'll need to add him to the database.

```
INSERT INTO actor(actorid,name,date_of_birth,birth_city,birth_country,gender,ethnicity)
VALUES (9000,'Boris Johnson','1964-06-19','New York City','USA','Male','White')
```

Let's look up the data:

```
SELECT * FROM actor WHERE name LIKE 'Boris%'
```

### Some other queries

Movies sorted by the average height of their actors.
