# Lab #8: From SQLite into Python

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>
This tutorial is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

## Lab Goals

## Acknowledgements

Python for Data Analysis Wes McKinney, Chapter 6 Data Loading Storage and File Formats, 6.4 Interacting With Databases (191-193)

David Muller, "[How To Use the `sqlite3` Module in Python 3](https://www.digitalocean.com/community/tutorials/how-to-use-the-sqlite3-module-in-python-3)" *Digital Ocean* (2 June 2020)

Data Carpentries "[Data Analysis and Visualization in Python for Ecologists](https://datacarpentry.org/python-ecology-lesson/09-working-with-sql/index.html)"

Charles Severance, Python for Everybody: Exploring Data in Python 3, Chapter 15 "Using Databases and SQL" (185-208)

Nik Piepenbreier, "[Python SQLite Tutorial - The Ultimate Guide](https://towardsdatascience.com/python-sqlite-tutorial-the-ultimate-guide-fdcb8d7a4f30)", *Towards Data Science* (1 April 2020).

# Table of Contents

# From SQLite into Python

In SQLite lab, we created a relational database with information about U.S. professional baseball players and teams.

We also explored how SQL syntax can let us interact with a relational database.

But imagine we want to work with a relational database from within Python.

We could work in SQL to create a new table that combines specific fields we want to work with, then export that table as a `.csv` file and load it into Python.

But we also have the option to interact with a SQL-based relational database from within Python, using a couple of key packages.

The `sqlite3` module "provides a SQL interface" from within Python ([`sqlite3` documentation](https://docs.python.org/3/library/sqlite3.html)].

`sqlite3` lets us establish a connection with a SQL database from within Python and then we can interact with the database using modified SQL commands.

The basic steps for `sqlite3`:
- Install and import `sqlite3` module
- Establish a `connection` with the database
- Create a `cursor` object 
- Use the `execute()` method in combination with the `cursor` object to execute SQL queries
- Close the connection using `close()` when done working with the database

To express this programmatically in Python:
```Python
# import sqlite3 module
import sqlite3

# establish connection to database
connection = sqlite3.connect("database_name.db")

# creates the cursor object
cursor = connection.cursor()

# closes the connection
cursor.close()
```

Now we can use our `cursor` object to interact with the database using modified SQL syntax.

The basic template for `sqlite3` syntax:
```Python
cursor.execute("SQL QUERY/COMMANDS GO HERE")
```

`cursor.execute()` followed by the SQL command (in quotation marks) lets us use SQL statements on our database from within Python.

Check out the [`sqlite3` documentation](https://docs.python.org/3/library/sqlite3.html) to learn more about the module's functionality and syntax.

For example, let's say we wanted to select the unique player ids from the `Player_Birthplaces` table in our database.

Remember the original SQL syntax for this query:
```SQL
SELECT DISTINCT player_ids
FROM player_birthplaces;
```

The modified syntax for this query in Python using `sqlite3`:
```Python
# import sqlite3 module
import sqlite3

# establish connection to database
connection = sqlite3.connect("database_name.db")

# creates the cursor object
cursor = connection.cursor()

# creates a new variable player ids 
player_ids = cursor.execute("SELECT DISTINCT player_ids FROM player_birthplaces")

# print the list of unique player ids now contained in the player_ids variable
print(player_ids)

# closes the connection
cursor.close()
```

The `cursor.execute()` function runs the `SELECT DISTINCT` statement.

The new `player_ids` variable contains the `player_id` column from the `player_birthplaces` table as a list in Python.

Another example using the `WHERE` statement for filtering.

The original SQL query:
```SQL
SELECT *
FROM Player_Birthplaces
WHERE country='DO';
```

This query returns all columns from the `Player_Birthplaces` table where data in the `country` field is equal to `DO`.

The modified syntax for this query in Python using `sqlite3`:
```Python
# import sqlite3 module
import sqlite3

# establish connection to database
connection = sqlite3.connect("database_name.db")

# creates the cursor object
cursor = connection.cursor()

# creates a new variable for query results
do_players = cursor.execute("SELECT * FROM Player_Birthplaces WHERE country='do'")

# print the new do_players variable
print(do_players)

# closes the connection
cursor.close()
```

Let's say we wanted to just see a query result and not load it to a new varible.

A modified version of the previous example:
```Python
# import sqlite3 module
import sqlite3

# establish connection to database
connection = sqlite3.connect("database_name.db")

# creates the cursor object
cursor = connection.cursor()

# creates a new variable for query results
cursor.execute("SELECT * FROM Player_Birthplaces WHERE country='do'")

# set a row count variable
count = 0

# for loop that prints row contents and reassigns count value
for row in cursor:
  print(row)
  count = count + 1

# print statement that takes final count value (i.e. the number of rows) and prints that total
print(count, 'rows')

# closes the connection
cursor.close()
```

In this example, the `for` loop iterates over the rows in the database and uses the `count` variable to track how many rows in the database meet the `WHERE` condition.

<blockquote>QX: How is Python storing the output for the last query? Should be seeing each row as sub-list within list. What could we do with this output?</blockquote>

<blockquote>QX: Explore modifying other SQL queries in Python. What did you try, how did it go, etc.</blockquote>

Why would we want to work with a relational database from within Python?

The short answer is program performance and memory load.

Storing your data in an SQLite database and loading query results into Python requires significantly less memory than storing all the database data in Python.

Less stored data = lower memory needs = improved program performance.

Interacting with a database from Python can also be a workflow consideration.

Imagine your company has data stored in a relational database system (Oracle, AWS, Microsft Access, etc.), and there are specific aggregations or calculations that need to be performed on that data regularly (say, for quarterly or annual reports).

Provided the underlying data structure remains largely consistent (which in most proprietary database systems it will), you could write a program in Python to automatically generate those aggregations and calculations.

# Practice Problems


# Lab Notebook Questions
