
### Components of DBMS (DataBase Management System)
-	Hardware
-	Software
-	Data
-	Procedures
-	Query Language
-	Query Processor
-	DB Engine
-	DB Manager

### Transaction Management
- A Transaction is a set of logically related sequence of actions.
- ACID Properties
	- Atomicity
		-	Atomicity ensures that either entire transaction takes place or nothing. But not in between.
	- Consistency
		-	Ensures database remain in consistent state even after transaction
	-	Isolation
		-	Ensures transactions are run in isolation
	- Durability
		-	Ensures transactions doesn’t lead to inconsistent database in case of power outage, network outage, overload, etc.

### Relational DBMS
- What’s a Relational model?
	- A Relational Model is a technique used to organize the data using Relations (Or Tables)
- Keys in Database?
	- Keys are used to uniquely identify a Record/Tuple/Row in a Table.
	- Keys are also used to establish relationship between tables.
- Super Key
	- A Super Key is a set of attributes within a table that uniquely identify each row/record/tuple.
- Candidate Key
	- A Candidate Key is minimal set	of attributes, which can uniquely identify each record in a Table.
- Primary Key
	- A Primary key is any key that is chosen to uniquely identify a record in the table (Other unused keys are called Secondary Key or Alternate Key)
	- Typically, ‘ID’ column of a table is often used as primary Key.
- Foreign Key
	- A Foreign key is a field (column) in one table that refers to key (Typically Primary key) in another table to create link between the two tables.
	- The only possible values that can go in the foreign key column is the key values of another table that it is referring to.
- Composite Key
	- A Composite key is a key of two or more attributes that uniquely Identifies a row, but each of the individual attribute doesn’t form a Key.
- Schema / Relation Schema / Database Schema
	- A Schema is like a blueprint that describes how the database is structured and organized. Typically used by database designers to design the database at a high level. It illustrates how the data is organized but doesn’t talk about what data is stored.
	- Relation Schema represents the structure of a relation (or table).
	- Database Schema represents the structure of the entire database. Basically, it’s collection of Relation schemas.
	- Schemas are used by teams to collaborate and interact with the developer and sometimes customers on designing the Database for a given application.

### Client tools
1. SQL Shell (psql)
1. pgAdmin

### SQL Shell (psql)
- connect
	- psql -U <username>
- list of databases
	- \list -> \l
- drop database
	-  drop database <db_name>
- show tables 
	- \dt
- drop tables
	- drop table <table_name>

### Constraints
- Not Null
- UNIQUE
- CHECK
- Primary Key
- Foreign Key
- Example
```js
CREATE TABLE students (
	id serial PRIMARY KEY,
	course_name TEXT NOT NULL,
	fee integer NOT NULL CHECK(fee < 100000),
	email VARCHAR(50) UNIQUE
)
```

### Operations
- =	Equal
- >	Greater than
- <	Less than
- >=	Greater than or equal
- <=	Less than or equal
- <> or !=	Not equal
- AND	Logical operator AND
- OR	Logical operator OR
