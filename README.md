# From SQLite into Python

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>This tutorial was written by Katherine Walden and is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

## Lab Goals

This lab covers how to connect to an SQLite database from Python using `sqlite3`. It also covers how to modify SQL query syntax when working in the Python programming environment.

By the end of this lab, students will be able to:
- Understand the core components of `sqlite3`
- Connecting to a SQLite database from Python
- Modify SQL queries for the Python programming environment
- Understand the basics of data loading, storage, memory, and program performance

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?pid=a9cdb4d3-f14d-4923-9359-ae350111e68e">Lecture/live coding playlist</a></td>
  </tr>
  </table>

## Acknowledgements

The author consulted the following resources when writing this tutorial.
- Chapter 6, "Data Loading, Storate, and File Formats" section 6.4 "Interacting With Databases" (pp. 191-193) from Wes McKinney's [*Python for Data Analysis: Data Wrangling With pandas, Numpy, and IPython*](https://www.oreilly.com/library/view/python-for-data/9781491957653/) (O'Reilly, 2017).
- David Muller, "[How To Use the `sqlite3` Module in Python 3](https://www.digitalocean.com/community/tutorials/how-to-use-the-sqlite3-module-in-python-3)" *Digital Ocean* (2 June 2020)
- Data Carpentries tutorial, "[Data Analysis and Visualization in Python for Ecologists](https://datacarpentry.org/python-ecology-lesson/09-working-with-sql/index.html)"
- Chapter 15 "Using Databases and SQL" (pp. 185-208) from Charles Severance's [*Python for Everybody: Exploring Data in Python 3*](https://www.py4e.com/) (2016).
- Nik Piepenbreier, "[Python SQLite Tutorial - The Ultimate Guide](https://towardsdatascience.com/python-sqlite-tutorial-the-ultimate-guide-fdcb8d7a4f30)", *Towards Data Science* (1 April 2020).

Peer review and editing was provided by Spring 2021 graduate teaching assistant [Subhadyuti Sahoo](https://github.com/SDSAHOO703).

# Table of Contents

- [Lecture & Live Coding](#lecture--live-coding)
- [Lab notebook template](#lab-notebook-template)
- [Data](#data)
- [Overview](#overview)
- [Establishing a Connection](#establishing-a-connection)
- [Modifying SQL Query Syntax](#modifying-sql-query-syntax)
- [Creating a Pandas `DataFrame`](#creating-a-pandas-dataframe)
- [Additional Considerations](#additional-considerations)
- [Additional Lab Notebook Questions](#additional-lab-notebook-questions)
- [Lab Notebook Questions](#lab-notebook-questions)

[Link to lab procedure as a Jupyter Notebook](https://drive.google.com/file/d/19TiuTHWtsn_0aDpn6g2VKK0IGqQNDj6y/view?usp=sharing)

# Lecture & Live Coding

Throughout this lab, you will see a Panopto icon at the start of select sections.

This icon indicates there is lecture/live coding asynchronous content that accompanies this section of the lab. 

You can click the link in the figure caption to access these materials (ND users only).

Example:

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?pid=a9cdb4d3-f14d-4923-9359-ae350111e68e">Lecture/live coding playlist</a></td>
  </tr>
  </table>

# Lab Notebook Template

[Link to lab notebook template (Jupyter Notebook)](https://colab.research.google.com/drive/1LFkXDBg0IUQ-Rpa_-ZVvAx1agKZ-2Hft?usp=sharing)

# Data

In a previous lab, we created a relational database from three `.csv` files.
- [`Player_Birthplaces.csv`](https://drive.google.com/file/d/1qthm3HSN5FaEFusC1_Zk-0qL0A6LIEB-/view?usp=sharing)
- [`Team_Locations.csv`](https://drive.google.com/file/d/1fttQhzoAwYOJC-mB17RATaqD4mfUTIV_/view?usp=sharing)
- [`Combined_Transactions.csv`](https://drive.google.com/file/d/17Z9C7snAMjARTQYoRXufwaO2j9ssl0g8/view?usp=sharing)

Consult [that lab's procedure/documentation](https://github.com/kwaldenphd/data-models#getting-started-with-db-browser-for-sqlite) if you need to re-create this database.

You will need this relational database file for this lab.

If needed, you can download the relational database [from Google Drive](https://drive.google.com/file/d/1uQHOIeMdCZOyXsIdDgfimUE-63I33pIm/view?usp=sharing).

# Overview

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=11b2b9a8-5a01-42f7-86b8-ae3500fcb717">Overview</a></td>
  </tr>
  </table>

In previous labs, we created a relational database with information about U.S. professional baseball players and teams. We also explored how SQL syntax can let us interact with a relational database.

But imagine we want to work with a relational database from within Python.

We could work in SQL using a combination of queries and joins to create a new table with specific fields we want to work with, then export that table as a `.csv` file and load it into Python. But we also have the option to interact with a SQL-based relational database from within Python, using a couple of key packages.

# Establishing a Connection

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=45557e7d-ae1c-487e-9474-ae3500fd9ee0">Establishing a Connection</a></td>
  </tr>
  </table>

The `sqlite3` module "provides a SQL interface" from within Python ([`sqlite3` documentation](https://docs.python.org/3/library/sqlite3.html)), letting us establish a connection with a SQL database from within Python and then we can interact with the database using modified SQL commands.

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
connection = sqlite3.connect("data.db")

# creates the cursor object
cursor = connection.cursor()

# closes the connection
cursor.close()
```

<blockquote>Q1: Describe the basic steps of how to establish a connection with a SQLite database from within Python.</blockquote>

# Modifying SQL Query Syntax

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=f66a46f8-d475-4d44-b8b0-ae35010d1dca">Modifying SQL Query Syntax</a></td>
  </tr>
  </table>

Now we can use our `cursor` object to interact with the database using modified SQL syntax. The basic template for `sqlite3` syntax:

```Python
cursor.execute("SQL QUERY/COMMANDS GO HERE")
```

ALTERNATE WORKFLOW:

```Python
sql_command = """SQL QUERY/COMMANDS GO HERE;"""

cursor.execute(sql_command)
```

`cursor.execute()` followed by the SQL command (in quotation marks) lets us use SQL statements on our database from within Python.
  * Alternatively, saving the SQL query as a string (with three double quotation marks and a semi colon) and running `cursor.execute()` on the string variable.

<blockquote>Check out the <a href = "https://docs.python.org/3/library/sqlite3.html"><code>sqlite3</code> documentation</a> to learn more about the module's functionality and syntax.</blockquote>

For example, let's say we wanted to select the unique player ids from the `Player_Birthplaces` table in our database. Remember the original SQL syntax for this query:

```SQL
-- select unique values from table field
SELECT DISTINCT player_ids
FROM player_birthplaces;
```

The modified syntax for this query in Python using `sqlite3`:

```Python
# import sqlite3 module
import sqlite3

# establish connection to database
connection = sqlite3.connect("data.db")

# creates the cursor object
cursor = connection.cursor()

# creates a new variable player ids 
player_ids = cursor.execute("SELECT DISTINCT id_person FROM player_birthplaces")

# get the query return
player_id_results = cursor.fetchall()

# print the list of unique player ids now contained in the player_id_results variable
print(player_id_results)

# closes the connection
cursor.close()
```

ALTERNATE WORKFLOW:

```Python
# import sqlite3 module
import sqlite3

# establish connection to database
connection = sqlite3.connect("data.db")

# creates the cursor object
cursor = connection.cursor()

# save query as string
sql_command = """SELECT id_person FROM player_birthplaces;"""

# creates a new variable player ids 
player_ids = cursor.execute(sql_command)

# get the query return
player_id_results = cursor.fetchall()

# print the list of unique player ids now contained in the player_id_results variable
print(player_id_results)

# closes the connection
cursor.close()
```

The `cursor.execute()` function runs the `SELECT DISTINCT` statement. The new `player_ids` variable contains the `player_id` column from the `Player_Birthplaces` table as a list in Python.

Another example using the `WHERE` statement for filtering. The original SQL query:

```SQL
-- select all values from table where specific condition is met
SELECT *
FROM player_birthplaces
WHERE country='DO';
```

This query returns all columns from the `Player_Birthplaces` table where data in the `country` field is equal to `DO`. The modified syntax for this query in Python using `sqlite3`:

```Python
# establish connection to database
connection = sqlite3.connect("data.db")

# creates the cursor object
cursor = connection.cursor()

# creates a new variable for query results
do_players = cursor.execute("SELECT * FROM player_birthplaces WHERE country='DO'")

# get the query return
player_country_results = cursor.fetchall()

# print the new do_players variable
print(player_country_results)

# closes the connection
cursor.close()
```

ALTERNATE WORKFLOW:

```Python
# import sqlite3 module
import sqlite3

# establish connection to database
connection = sqlite3.connect("data.db")

# creates the cursor object
cursor = connection.cursor()

# save query as string
sql_command = """SELECT * FROM player_birthplaces WHERE country='DO';"""

# creates a new variable player ids 
do_players = cursor.execute(sql_command)

# get the query return
player_country_results = cursor.fetchall()

# print the list of unique player ids now contained in the player_id_results variable
print(player_country_results)

# closes the connection
cursor.close()
```

Let's say we wanted to just see a query result and not load it to a new variable. A modified version of the previous example:

```Python
# import sqlite3 module
import sqlite3

# establish connection to database
connection = sqlite3.connect("data.db")

# creates the cursor object
cursor = connection.cursor()

# creates a new variable for query results
cursor.execute("SELECT * FROM player_birthplaces WHERE country='DO'")

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

<blockquote>Q2: How is Python storing the output for the last query from step 24 (variable, data structure, etc)? What could we do with this output?</blockquote>

# Creating a Pandas `DataFrame`

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=bc5f5fcb-fb11-4473-9d54-ae35010efe74">Creating a Pandas DataFrame</a></td>
  </tr>
  </table>

The previous section of the lab included sample code for a query that returns all columns from the `Player_Birthplaces` table where data in the `country` field is equal to `DO`.

```Python
# establish connection to database
connection = sqlite3.connect("data.db")

# creates the cursor object
cursor = connection.cursor()

# creates a new variable for query results
do_players = cursor.execute("SELECT * FROM player_birthplaces WHERE country='DO'")

# get the query return
player_country_results = cursor.fetchall()

# print the new do_players variable
print(player_country_results)

# closes the connection
cursor.close()
```

In this example, the query results are being stored to the `player_country_results` variable as a list with sublists or nested lists. We can use `pd.DataFrame` to create a Pandas `DataFrame` from that list with sublists.

```Python
# import pandas
import pandas as pd

# establish connection to database
connection = sqlite3.connect("data.db")

# creates the cursor object
cursor = connection.cursor()

# creates a new variable for query results
do_players = cursor.execute("SELECT * FROM player_birthplaces WHERE country='DO'")

# get the query return
player_country_results = cursor.fetchall()

# closes the connection
cursor.close()

# create dataframe
df = pd.DataFrame(player_country_results)

# show df
df
```


ALTERNATE WORKFLOW:

```Python
# import sqlite3 module
import sqlite3

# establish connection to database
connection = sqlite3.connect("data.db")

# creates the cursor object
cursor = connection.cursor()

# save query as string
sql_command = """SELECT * FROM player_birthplaces WHERE country='DO';"""

# creates a new variable player ids 
do_players = cursor.execute(sql_command)

# get the query return
player_country_results = cursor.fetchall()

# closes the connection
cursor.close()

# create dataframe
df = pd.DataFrame(player_country_results)

# show df
df
```

From there, a single line of code will save the `DataFrame` as a `.csv` file.

```Python
# save to csv
df.to_csv("output.csv", index=False)
```

Modified syntax that takes the column names from SQL and maps those onto your `DataFrame`.

```Python
# runs query
results = cursor.execute(sql_command)

# store column names as list
cols = [column[0] for column in results.description]

# create dataframe with query results and column names
df = pd.DataFrame.from_records(data= results.fetchall(), columns=cols)

# close connection
cursor.close()
```

# Additional Considerations

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=2364e173-0e6b-4f82-91e1-ae350110bf80">Additional Considerations</a></td>
  </tr>
  </table>

Why would we want to work with a relational database from within Python? The short answer is program performance and memory load. Storing your data in an SQLite database and loading query results into Python requires significantly less memory than storing all the database data in Python. Less stored data = lower memory needs = improved program performance.

Interacting with a database from Python can also be a workflow consideration. Imagine your company has data stored in a relational database system (Oracle, AWS, Microsft Access, etc.) and there are specific aggregations or calculations that need to be performed on that data regularly (say, for quarterly or annual reports). Provided the underlying data structure remains largely consistent (this is  true for most proprietary/commercial database systems), you could write a program in Python to automatically generate those aggregations and calculations.

# Additional Lab Notebook Questions

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=2364e173-0e6b-4f82-91e1-ae350110bf80">Additional Lab Notebook Questions</a></td>
  </tr>
  </table>

Q3: Take at least 3 of the queries you wrote for the Queries and Joins lab and modify them to run within a Python environment. Include code + comments.
- For this question, your program needs to:
  * Establish a connection to the database
    * `sqlite3.connect()`
  * Create the cursor object
    * `connection.cursor()`
  * Include modified query syntax
    * `cursor.execute()`
  * Get query return and store to variable
    * `cursor.fetchall()`
  * Close connection
    * `cursor.close()`

Q4: For at least one of the Q3 queries, create a Pandas `DataFrame` from the query results and write to a `.csv` file. Include code + comments.
- For this question, your program needs to:
  * Create the dataframe
    * `pd.DataFrame()`
  * Save as CSV 
    * `pd.to_csv()`

Q5: What changes did you have to make to the query syntax to be able to run SQL queries in Python? What challenges did you encounter in this workflow? How did you solve them?

# Lab Notebook Questions

[Link to lab notebook template (Jupyter Notebook)](https://colab.research.google.com/drive/1LFkXDBg0IUQ-Rpa_-ZVvAx1agKZ-2Hft?usp=sharing)

Q1: Describe the basic steps of how to establish a connection with a SQLite database from within Python.

Q3: How is Python storing the output for the last query from step 24 (variable, data structure, etc)? What could we do with this output?

Q3: Take at least 3 of the queries you wrote for the Queries and Joins lab and modify them to run within a Python environment. Include code + comments.
- For this question, your program needs to:
  * Establish a connection to the database
    * `sqlite3.connect()`
  * Create the cursor object
    * `connection.cursor()`
  * Include modified query syntax
    * `cursor.execute()`
  * Get query return and store to variable
    * `cursor.fetchall()`
  * Close connection
    * `cursor.close()`

Q4: For at least one of the Q3 queries, create a Pandas `DataFrame` from the query results and write to a `.csv` file. Include code + comments.
- For this question, your program needs to:
  * Create the dataframe
    * `pd.DataFrame()`
  * Save as CSV 
    * `pd.to_csv()`

Q5: What changes did you have to make to the query syntax to be able to run SQL queries in Python? What challenges did you encounter in this workflow? How did you solve them?
