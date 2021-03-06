# From SQLite into Python

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>
This tutorial is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

## Lab Goals

This lab covers how to connect to an SQLite database from Python using `sqlite3`. It also covers how to modify SQL query syntax when working in the Python programming environment.

By the end of this lab, students will be able to:
- Understand the core components of `sqlite3`
- Connecting to a SQLite database from Python
- Modify SQL queries for the Python programming environment
- Understand the basics of data loading, storage, memory, and program performance

[Click here](https://raw.githubusercontent.com/kwaldenphd/sqlite-to-python/main/sqlite-to-python.ipynb) and select the "Save as" option to download this lab as a Jupyter Notebook.

## Acknowledgements

The author consulted the following resources when writing this tutorial.
- Chapter 6, "Data Loading, Storate, and File Formats" section 6.4 "Interacting With Databases" (pp. 191-193) from Wes McKinney's [*Python for Data Analysis: Data Wrangling With pandas, Numpy, and IPython*](https://www.oreilly.com/library/view/python-for-data/9781491957653/) (O'Reilly, 2017).
- David Muller, "[How To Use the `sqlite3` Module in Python 3](https://www.digitalocean.com/community/tutorials/how-to-use-the-sqlite3-module-in-python-3)" *Digital Ocean* (2 June 2020)
- Data Carpentries tutorial, "[Data Analysis and Visualization in Python for Ecologists](https://datacarpentry.org/python-ecology-lesson/09-working-with-sql/index.html)"
- Chapter 15 "Using Databases and SQL" (pp. 185-208) from Charles Severance's [*Python for Everybody: Exploring Data in Python 3*](https://www.py4e.com/) (2016).
- Nik Piepenbreier, "[Python SQLite Tutorial - The Ultimate Guide](https://towardsdatascience.com/python-sqlite-tutorial-the-ultimate-guide-fdcb8d7a4f30)", *Towards Data Science* (1 April 2020).

Peer review and editing was provided by Spring 2021 graduate teaching assistant [Subhadyuti Sahoo](https://github.com/SDSAHOO703).

# Table of Contents

- [From SQLite into Python](#from-sqlite-into-python)
  * [Establishing a Connection](#establishing-a-connection)
  * [Modifying SQL Query Syntax](#modifying-sql-query-syntax)
  * [Additional Considerations](#additional-considerations)
- [Practice Problems](#practice-problems)
- [Lab Notebook Questions](#lab-notebook-questions)

# From SQLite into Python

1. In SQLite lab, we created a relational database with information about U.S. professional baseball players and teams.

2. We also explored how SQL syntax can let us interact with a relational database.

4. But imagine we want to work with a relational database from within Python.

5. We could work in SQL to create a new table that combines specific fields we want to work with, then export that table as a `.csv` file and load it into Python.

5. But we also have the option to interact with a SQL-based relational database from within Python, using a couple of key packages.

## Establishing a Connection

6. The `sqlite3` module "provides a SQL interface" from within Python ([`sqlite3` documentation](https://docs.python.org/3/library/sqlite3.html)).

7. `sqlite3` lets us establish a connection with a SQL database from within Python and then we can interact with the database using modified SQL commands.

8. The basic steps for `sqlite3`:
- Install and import `sqlite3` module
- Establish a `connection` with the database
- Create a `cursor` object 
- Use the `execute()` method in combination with the `cursor` object to execute SQL queries
- Close the connection using `close()` when done working with the database

9. To express this programmatically in Python:
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

<blockquote>Q1: Describe the basic steps of how to establish a connection with a SQLite database from within Python.</blockquote>

## Modifying SQL Query Syntax

10. Now we can use our `cursor` object to interact with the database using modified SQL syntax.

11. The basic template for `sqlite3` syntax:
```Python
cursor.execute("SQL QUERY/COMMANDS GO HERE")
```

12. `cursor.execute()` followed by the SQL command (in quotation marks) lets us use SQL statements on our database from within Python.

13. Check out the [`sqlite3` documentation](https://docs.python.org/3/library/sqlite3.html) to learn more about the module's functionality and syntax.

14. For example, let's say we wanted to select the unique player ids from the `Player_Birthplaces` table in our database.

15. Remember the original SQL syntax for this query:
```SQL
SELECT DISTINCT player_ids
FROM Player_Birthplaces;
```

16. The modified syntax for this query in Python using `sqlite3`:
```Python
# import sqlite3 module
import sqlite3

# establish connection to database
connection = sqlite3.connect("EoCII_Database_Lab.db")

# creates the cursor object
cursor = connection.cursor()

# creates a new variable player ids 
player_ids = cursor.execute("SELECT DISTINCT id_person FROM Player_PoB")

# get the query return
player_id_results = cursor.fetchall()

# print the list of unique player ids now contained in the player_id_results variable
print(player_id_results)
```

17. The `cursor.execute()` function runs the `SELECT DISTINCT` statement.

18. The new `player_ids` variable contains the `player_id` column from the `Player_Birthplaces` table as a list in Python.

19. Another example using the `WHERE` statement for filtering.

20. The original SQL query:
```SQL
SELECT *
FROM Player_Birthplaces
WHERE country='DO';
```

21. This query returns all columns from the `Player_Birthplaces` table where data in the `country` field is equal to `DO`.

22. The modified syntax for this query in Python using `sqlite3`:
```Python
# establish connection to database
connection = sqlite3.connect("EoCII_Database_Lab.db")

# creates the cursor object
cursor = connection.cursor()

# creates a new variable for query results
do_players = cursor.execute("SELECT * FROM Player_PoB WHERE country='DO'")

# get the query return
player_country_results = cursor.fetchall()

# print the new do_players variable
print(player_country_results)

# closes the connection
cursor.close()
```

23. Let's say we wanted to just see a query result and not load it to a new varible.

24. A modified version of the previous example:
```Python
# import sqlite3 module
import sqlite3

# establish connection to database
connection = sqlite3.connect("EoCII_Database_Lab.db")

# creates the cursor object
cursor = connection.cursor()

# creates a new variable for query results
cursor.execute("SELECT * FROM Player_PoB WHERE country='DO'")

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

25. In this example, the `for` loop iterates over the rows in the database and uses the `count` variable to track how many rows in the database meet the `WHERE` condition.

<blockquote>Q2: Take at least 3 of the queries you wrote for the Queries and Joins lab and modify them to run within a Python environment. Include code + comments. What changes did you have to make to the query syntax? Any challenges you encountered (and how did you solve them)?</blockquote>

<blockquote>Q3: How is Python storing the output for the last query from step 24? What could we do with this output?</blockquote>

### Additional Considerations

26. Why would we want to work with a relational database from within Python?

27. The short answer is program performance and memory load.

28. Storing your data in an SQLite database and loading query results into Python requires significantly less memory than storing all the database data in Python.

29. Less stored data = lower memory needs = improved program performance.

30. Interacting with a database from Python can also be a workflow consideration.

31. Imagine your company has data stored in a relational database system (Oracle, AWS, Microsft Access, etc.) and there are specific aggregations or calculations that need to be performed on that data regularly (say, for quarterly or annual reports).

32. Provided the underlying data structure remains largely consistent (which in most proprietary database systems it will), you could write a program in Python to automatically generate those aggregations and calculations.

# Practice Problems

33. No practice problems for this lab. Spend this time working on the final project.

# Lab Notebook Questions

Q1: Describe the basic steps of how to establish a connection with a SQLite database from within Python.

Q2: Take at least 3 of the queries you wrote for the Queries and Joins lab and modify them to run within a Python environment. Include code + comments. What changes did you have to make to the query syntax? Any challenges you encountered (and how did you solve them)?

Q3: How is Python storing the output for the last query from step 24? What could we do with this output?
