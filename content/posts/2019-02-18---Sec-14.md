---
title: SQL Injection
date: '2019-02-18'
time: '☕️☕️'
template: "post"
draft: false
slug: "SQL-vulnerability"
category: "Cybersecurity"
tags:
  - "Cybersecurity"
description: "A structured query language (SQL) injection, or SQLi, occurs when a vulnerability on a
database-backed site allows an attacker to query or otherwise attack the site’s database."
---

This article was done using my notes from a book titled WEB Hacking 101 written by Peter Yaworksi in 2018.

<sub>Yaworksi P. (2018). Web Hacking 101, How To Make Money Hacking Ethically</sub>

## What is an SQL Injection ?

As said in the summary, SQL injection or SQLi occurs when a vulnerability on a
database-backed site allows an attacker to query or otherwise attack the site’s database. They can enable
an attacker to manipulate or extract information or even create an administrator log in
for themselves in the database.

### SQL databases 101

Databases store information in records and fields contained in a collection of tables.
Tables contain one or more columns and a row in a table represents a record in the
database.

Users rely on a programming language called SQL (structured query language) to create,
read, update, and delete records in the database. The user sends SQL commands
(also called statements or queries) to the database and, assuming the commands are
accepted, the database interprets the statements and performs some action.

SQL statements are made up of keywords and functions.For example, the following
statement tells the database to select information from the name column in the users
table, for records where the ID column is equal to 1.

```
SELECT name FROM users WHERE id = 1;
```

Many websites rely on databases to store information and to use that information to
dynamically generate content.

## SQLi

Let’s look at a theoretical example of a server’s PHP code to generate a MySQL command
after a user visits the URL https://www.google.com?search=hello

```
$search = $_GET['search'];
$q = "SELECT * FROM users WHERE search = '$search' ";
mysql_query($query);
```

The code uses $_ GET to access the search value from the URL parameters specified
between its brackets and stores the value in the $search variable. The parameter is then
passed to the $q variable without any sanitization. The $q variable represents the query
to execute and fetches all data from the users table where the search column matches
the value in the search URL parameter. The query is executed by passing the \$q variable
to the PHP function mysql_query.

The site is expecting name to contain regular text, but if a user enters the malicious
input test' OR 1='1 into the URL parameter as in :

```
https://www.google.com?search=test' OR 1='1,

the executed query is :

$query = "SELECT * FROM users WHERE name = 'test' OR 1='1' ";
```

If the injected query didn’t include an opening single quote, the hanging quote would have caused SQL syntax errors, which
would prevent the query from executing.

SQL uses conditional operators like AND and OR. In this case, the SQLi modifies the
WHERE clause to search for records where the name column matches test or the
equation 1='1' returns true. MySQL helpfully converts treats '1' as an integer and since 1
always equals 1, the condition is true and the query returns all records in the users table.

## How to protect yourself against SQLi ?

First and foremost, Web frameworks like Ruby on Rails, Django, Symphony, and so on also offer built in
protections to help prevent SQLi. However, they aren’t perfect and can’t prevent the
vulnerability everywhere.

To effectively protect yourself from SQLi, you want to properly sanitized your input. See the following code written by OWASP developers (Rapid7, 2019).
This code is available in Dvwa once Metasploit installed.

See the following code :

```
<?php
if(isset($_GET['Submit'])){

    // Retrieve data
    $id = $_GET['id'];

    $id = stripslashes($id);

    // real_escape will look for special characters (',",/,#,...)
    $id = mysql_real_escape_string($id);

    if (is_numeric($id)) {

        // '$id' is surrounded by quotes, anything we would inject would be consider as part of the id
        $getid = "SELECT first_name, last_name FROM users WHERE user_id = '$id'";

        $result = mysql_query($getid); // Removed 'or die' to suppress mysql errors

        $num = @mysql_numrows($result); // The '@' character suppresses errors making the injection 'blind'

        $i=0;

        while ($i < $num) {

            $first = mysql_result($result,$i,"first_name");

            $last = mysql_result($result,$i,"last_name");

            echo '<pre>';

            echo 'ID: ' . $id . '<br>First name: ' . $first . '<br>Surname: ' . $last;

            echo '</pre>';

            $i++;
        }
    }
}
?>
```

The important take away is the following piece of code :

```
    $getid = "SELECT first_name, last_name FROM users WHERE user_id = '$id'";
```

\$id is surrounded by quotes which neutralizes whatever SQL queries you may want to inject. Quotes will treat your injection as a string. As such, your injection won't just work.

<sub>
Rapid7 (2019). Metasploitable 2 Exploitability Guide, url : https://metasploit.help.rapid7.com/docs/metasploitable-2-exploitability-guide
</sub>
