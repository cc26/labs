SQL
====

Structured Query Language is a language to speak to databases.

A database is a collection of tables. Each table has a number of columns (or fields)
and rows. Rows usually represent a thing, and columns are one aspect of that thing.
For example, we might have a table of "people" with columns for first name, last name,
and age. Columns usually have types like "text" or "integer" to indicate what sort
of data they are.

Databases are complicated and you won't have to implement one -- but you will have
to leverage one. There are many SQL databases such as MySQL, PostgreSQL, and what
you'll be using today: SQLite3. SQLite is a little bit different from most other
databases because it's simply stored in a file. (Yours will be named
zipcodes.db and should appear in the same directory as everything else when you run
your loader.) The library you'll be using to talk to the database, AnyDB, abstracts
many of these differences, however.

Once you're able to talk to a database you can manipulate its contents through
saying SQL statements, which the server will respond to. The process of sending
a statement and recieving an answer is called querying, and you'll often hear
SQL statements referred to as "queries." *We'll get into the specifics of sending
and receiving later.*

The three types of statements you'll need to know for this lab (as well as project 3)
are CREATE TABLE, INSERT INTO, and SELECT.

CREATE TABLE does exactly what it sounds like. It creates a new table in the database
given a specification of the columns the table should contain. A newly created table
has 0 rows. A `CREATE TABLE` statement for the people database might look like this:

```
CREATE TABLE people (firstname TEXT, lastname TEXT, age INTEGER);
```

The database doesn't say much in response to a CREATE TABLE unless there's an error,
so you can mostly ignore it. Note though that issuing a CREATE TABLE when a table
with that name exists already is an error! The most straightforward way to deal with
this is to delete the database (which isn't destructive because you're filling it back
up again every time when you run your loader.)

INSERT INTO is also pretty straightforward: it inserts (adds) a row into a table. You'll give
it the name of the table as well as the values for each column and it creates a new row.
We could add a new person to people like this:

```
INSERT INTO people VALUES ('Sam', 'Birch', 20)
```

Again, the server doesn't say much in response to an INSERT INTO.

SELECT is a little trickier, but a good metaphor might be "find me rows." It takes
three components: a what (which columns, or `*` for all of them), a table, and a corollary
clause called WHERE. It's clearer when it's written out:

```
SELECT lastname FROM people WHERE age=20
```

The database would return the last names of all the people in the table who have
an age of 20. We could fetch the whole row instead, since we probably want to
know their first name too:

```
SELECT * FROM people WHERE age=20
```

Unlike CREATE TABLE and INSERT INTO, the database's response to a SELECT is important!
Again, we'll get into the specifics of reading responses later, but for now understand
that the database will be sending you a list of rows which fulfil your WHERE clause.

(More about indexes and why you should have a key?)

# Your task

In this lab you will be implementing the Zipcoder, a website which tells you where
a zipcode is. Zipcodes for the unfamiliar are 5 digit numbers which specify a region
in the United States (a town, a part of a city, etc.). For example:

02912: Brown University (Providence, RI)

10001: [A sliver of Manhattan](http://goo.gl/maps/7H8Nh)

20500: The White House

02906: Region around Brown in Providence

## Setup

Download the stencil folder for this lab [here](UPLOADTOSERVER) and extract
it. Open a terminal and `cd` into that directory.

Run `npm install` at the command line to install required libraries for this lab.

This shouldn't raise any errors but flag a TA if it does!

## The loader

## The server

The second portion of your task for this project is to write a server using Node and
Express to allow users to look up the locations of Zipcodes. You can implement this
any way that works, but one simple (and uninteresting!) way might be to display the
location of a Zipcode in the URL:

```
localhost:8080/02912

-> PROVIDENCE, RI
```

You can parametrize the path with Express simply:

```
app.get('/:zipcode', function(request, response){
	//request.params.zipcode contains the Zipcode from the path!
	//Use that to write a query for your database and return the result.
});
```