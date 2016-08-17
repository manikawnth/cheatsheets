# SQLITE3 

## Reference URL 
## [SQLITE3](https://docs.python.org/3/library/sqlite3.html)

## Regular Life-cycle
---------------------

1. import SQLITE3
2. Create a Database Connection object
3. Create a Cursor Object (Optional)
4. Execute CRUD operations on Cursor (preferable) or Connection Objects
5. Perform Commit or Roll-back
6. Close the connection


## Important Objects of sqlite3 package
---------------------------------------

1. Connection Objects
2. Cursor Objects 
3. Row Objects 

## All-in-One Usage
-------------------
 
```python
# import sqlite 
import sqlite3	
```

#### Connection objects
Connection object maintains the all the basic connection of the transaction with the database

```python 
#Create a connection object
conn = sqlite3.connect('example.db')	#path to the example database
conn = sqlite3.connect(':memory:')		#create on the fly in-memory database

#Optionally set some connection properties
conn.isolation_level = None				#isolation-level. 'None' is used for auto-commit mode
conn.isolation_level = 'DEFERRED'
conn.isolation_level = 'IMMEDIATE'
conn.isolation_level = 'EXCLUSIVE'

#Transaction Integrity
conn.commit()	#commits all the changes in the current transaction
conn.rollback()	#rolls-back all the changes in the current transaction since last commit

#Close the connection
conn.close()	#closes the connection

#Security
conn.set_authorizer(callback)	#called on every attempt to access the column in the database

#Dump the table
conn.iterdump()		#Returns an iterator of the complete sql dump of the table which can be written to file

```

#### Cursor objects

```python 

#create a cursor Object
cur = conn.cursor()		#Creates a cursor object 

#execute - execute sql statement on a cursor
cur.execute("create table people (name_last, age)")		#create table people with the columns
cur.execute("insert into people values (?, ?)", ('mani', 32))	#Preferred way of using parms in execute 
cur.execute("select * from people where name_last=:who and age=:age", {"who": who, "age": age})

#executemany - execute sql statement on multiple parms
persons = [
			('mani' , 'kanth'),
			('some' , 'thing')
		]
cur.executemany("insert into person(firstname, lastname) values (?, ?)", persons)

#executescript - execute a script of sql statements like a sql-devloper or query-management-facility
cur.executescript("""
    insert into recipe (name, ingredients) values ('broccoli stew', 'broccoli peppers cheese tomatoes');
    insert into recipe (name, ingredients) values ('pumpkin stew', 'pumpkin onions garlic celery');
    insert into recipe (name, ingredients) values ('broccoli pie', 'broccoli cheese onions flour');
    insert into recipe (name, ingredients) values ('pumpkin pie', 'pumpkin sugar flour butter');
    """)

#fetchone - fetches next row in the cursor
cur.fetchone()

#fetchmany - fetches next n list of rows
cur.fetchmany(5)

#fetchall - fetches all rows in a cursor 
cur.fetchall() 

#close - close the cursor
cur.close()

#Other attributes
cur.rowcount	#current row count in the cursor 
cur.lastrowid	#last row that is modified in the cursor
cur.connection	#refers the current connection object of the cursor

```

#### Row Objects

We can do nothing about these objects. They are read only

```python
row1 = cur.fetchone()	#returns a row
row1.keys()				#keys will return the column names

#row is an iterator
for column_value in row1:
	print(column_value)
```

## Happy Ending
