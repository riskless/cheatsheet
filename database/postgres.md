
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
- \>	Greater than
- <	Less than
- \>=	Greater than or equal
- <=	Less than or equal
- <> or !=	Not equal
- AND	Logical operator AND
- OR	Logical operator OR

### Basic CRUD SQL
```js
// Create a Table
CREATE TABLE <table_name> (
 <column_name1> <table_constraint>,
 <column_name2> TYPE <column_constraint>
) INHERITS <existing_table_name>;

CREATE TABLE Students (
    ID  SERIAL PRIMARY KEY,
    STUDENTNAME TEXT    NOT NULL,
    AGE int,
    DESCRIPTION TEXT    NOT NULL
);

// Read from table:
SELECT <column_list> FROM <table name>;
SELECT ID, STUDENTNAME FROM Students;
SELECT * FROM Students;

// Insert the table:
INSERT INTO table(column1, column2, ...)
VALUES (value1, value2, ...);
INSERT INTO Students (STUDENTNAME,AGE,DESCRIPTION) VALUES ('Bell', '28', 'Sometext');

// Delete From Table:
DELETE FROM <table_name> WHERE <condition>;
DELETE FROM Students WHERE id = 2;
```

### Table Operations
- CREATE TABLE & CREATE TABLE AS: 
	- Helps us create a table or Create a table by populating its data from the result of another Query.
- ALTER TABLE
	- Helps you make changes to the table structure (Ex: add a new column, rename existing column, change table name, add constraint to table field, etc)
- DROP (Delete):
	- Helps delete table/table row/database
- Example
```js
// create table
CREATE TABLE Students (
    ID  SERIAL PRIMARY KEY,
    STUDENTNAME TEXT    NOT NULL,
    AGE int,
    DESCRIPTION TEXT    NOT NULL
);

// create table as
CREATE TABLE <TableName> AS <Query>;
CREATE TABLE old_students AS SELECT * FROM students WHERE age>28;

// alter table
ALTER TABLE students ADD COLUMN email TEXT;
ALTER TABLE students DROP COLUMN email;
ALTER TABLE students RENAME COLUMN email TO student_email;
ALTER TABLE students RENAME TO my_students;
ALTER TABLE students ADD PRIMARY KEY (ID,email);
ALTER TABLE students ALTER COLUMN email TYPE VARCHAR;
ALTER DATABASE mydbname RENAME TO newdbname;

// drop table
DROP TABLE IF EXISTS students;
TRUNCATE TABLE students;
```

### Fetching Data
- ORDER BY 
	- Helps sort the result set form a query.
- SELECT DISTINCT
	- Helps remove duplicate rows, based on the specified column list.
- GROUP BY & HAVING
	- Divides the result set in to distinct groups and lets us apply aggregate function on each one of them.
- Aggregate Functions
	- SUM() - return the sum of all or distinct values.
	- AVG() - return the average value.
	- COUNT() - return the number of values.
	- MAX() - return the maximum value.
	- MIN() - return the minimum value.
- WHERE, IN & BETWEEN
	- Helps us fetch specific data from a table, based on the specified condition.
- FETCH 
	- Helps us limit the number of rows returned from a query.
- LIKE 
	- Helps us limit the number of rows returned from a query, based on matching pattern.
- Example
```js
// order by
SELECT * FROM students ORDER BY age ASC;
SELECT * FROM students ORDER BY age DSC;
SELECT * FROM students ORDER BY studentname ASC;

// distinct 
SELECT DISTINCT age FROM students;

// group by
SELECT studentname FROM students GROUP BY studentname;
SELECT studentname,SUM(fee) FROM students GROUP BY studentname;
SELECT studentname,SUM(fee) FROM students GROUP BY studentname HAVING SUM(fee) > 5000;

// where, in & between
SELECT id,studentname FROM students WHERE age>25 AND studentname =’Krish’;
SELECT id,studentname FROM students WHERE age=25 OR age=28;
SELECT id,studentname FROM students WHERE age IN(25,28);
SELECT id,studentname FROM students WHERE age>25 AND age<35;
SELECT id,studentname FROM students WHERE age BETWEEN 25 AND 35;
SELECT id,studentname FROM students WHERE age NOT BETWEEN 25 AND 35;

// fetch
SELECT id,studentname FROM students WHERE age>25 FETCH FIRST ROW ONLY;
SELECT id,studentname FROM students WHERE age>25 FETCH FIRST 3 ROW ONLY;

// like
SELECT * FROM students WHERE studentname LIKE ‘%ar%’;
SELECT * FROM students WHERE studentname NOT LIKE ‘%ar%’;
SELECT * FROM students WHERE studentname LIKE ‘_s_’;
```

### Joins
- INNER JOIN
	- Helps select common rows from two or more tables based on the condition.
- LEFT JOIN
	- Similar to Inner Join. Except, it will also select all the rows of the table on the left side of the Join. The rows for which there is no matching row on the table on the right side of Join, would have values as null.
- RIGHT JOIN
	- Opposite to left Join. Let’s us select all rows on the table on the right hand side of Join, as well as the common rows.
- FULL JOIN
	- Combination of left and right Joins.
- CROSS JOIN (or CARTESIAN JOIN)
	- Combination is join of each row in one table to each row in another table. The is the default join when there is no condition specified (WHERE Clause) 
- Example
```js
// inner join
SELECT s.studentname,s.fee FROM  students s
INNER JOIN student_course sc
ON s.id = sc."student_id_FK";

SELECT s.studentname,s.fee,c.name FROM  students s

INNER JOIN student_course sc
ON s.id = sc."student_id_FK"

INNER JOIN course c
ON c.id = sc."course_id_FK";

// left join
SELECT s.studentname,s.fee FROM  students s
LEFT JOIN student_course sc
ON s.id = sc."student_id_FK";

// right join
SELECT s.studentname,s.fee,c.name FROM  students s

INNER JOIN student_course sc
ON s.id = sc."student_id_FK"

RIGHT JOIN course c
ON c.id = sc."course_id_FK";

// full join
// The below SQL doesn’t make sense. There is no point in condition s.id=c.id. This is just for illustration of FULL Join.
SELECT s.studentname,c.name FROM  students s
FULL JOIN course c
ON s.id = c.id;
 
// cross join
SELECT a.studentname,c.name FROM students a, course c;
```

### Dealing with Multiple Queries
- UNION
	- Combines and represents result set of two or more queries in to one result set.
	-	All the queries must have the same number of columns to show and must be have the same or compatible sequence of datatypes.
	-	Removes all the duplicate rows by default. Use "UNION ALL" to not do that!
- INTERSECT 
	- Helps us get the common rows by combining the result set of two of more queries.
- EXCEPT 
	- Helps us get distinct rows from the left query that are not in the output of the right query.
- GROUPING SETS 
	- Helps us write multiple grouping statements in to one.
- CUBE
- ROLL UP
- Example          
```js
// union
SELECT s.id,s.studentname FROM students s
UNION
SELECT c.id,c.name FROM course c;

// intersect
SELECT s.id FROM students s
INTERSECT
SELECT c.id FROM course c;

// except
SELECT s.id FROM students s 
EXCEPT  
SELECT c.id FROM course c;

// groupping sets
SELECT sname, NULL, SUM(fee) FROM stu_course
GROUP BY sname

UNION ALL

SELECT NULL, cname, SUM(fee) FROM stu_course
GROUP BY cname

UNION ALL

SELECT NULL,NULL, SUM(fee) FROM stu_course
GROUP BY fee;

(or)

SELECT sname,cname, SUM(fee) FROM stu_course
GROUP BY GROUPING SETS (
	(sname),
	(cname),
	(fee)
);

// cube
SELECT sname, cname, SUM(fee) FROM stu_course
GROUP BY cube(sname,cname,fee);

(or)

SELECT sname, cname, SUM(fee) FROM stu_course
GROUP BY GROUPING SETS(
	(sname,cname,fee),
	(sname,cname),
	(sname,fee),
	(cname,fee),
	(sname),
	(cname),
	(fee)
);

// rollup
ROLLUP(sname,cname,fee);

(sname,cname,fee)
(sname,cname)
(sname)
()
```

### Dealing with Sub Queries
- Sub Query, ANY, ALL
	- A query that is nested inside another query is called a Sub Query.
- Example
```js
// Sub Query, ANY, ALL
SELECT * FROM students WHERE age > (
	SELECT AVG(age) FROM students
);

SELECT * FROM students WHERE id = ANY(
	SELECT id FROM course
);

SELECT * FROM students WHERE id > ALL(
	SELECT id FROM course
);

```

### Functions
- Functions or Stored procedures are set of statements that are compiled and stored in the database, so that we can run them using the function name. 
- Advantages
	-	Don’t have to send multiple Database requests from the application logic. Instead all the SQL statements can be executed with just one request by using the function name
	-	Testing the database part is easier. No need to run the application to test.
	-	No retranslation required. Once the Function is compiled, the compiled version is stored in the cache memory until the session ends. This will improve performance.
- Disadvantages
	-	Switching to another Database software would need lot of technical expertise and hence would cause delay in project delivery.
	-	Not an ideal choice for simple CRUD operations (In real time, 95% of database operations that a typical web application need, are just simple CRUD operations) They can be better managed using ORM Frameworks.
- Example
```js
// create function
CREATE OR REPLACE FUNCTION <function_name> (arguments) 
RETURNS returntype AS $variable$
   DECLARE
      <Declaration section>;
   BEGIN
      <Function Logic>

      RETURN <variable | literal>
   END; LANGUAGE <Prog Lang like PL/pgSQL, C, Perl, Python>;

CREATE OR REPLACE FUNCTION addTen (arg1 integer) 
RETURNS integer AS $$
   DECLARE
      finalSum integer;
   BEGIN
      finalSum = arg1 + 10;

      RETURN finalSum;
   END; 
   $$ LANGUAGE plpgsql;



// Function with SQL statement.
CREATE OR REPLACE FUNCTION getAvgAge() 
RETURNS numeric AS $$
DECLARE
    avgResult numeric;
BEGIN
    SELECT AVG(age) from students INTO avgResult;
	RETURN avgResult;
END
$$ LANGUAGE 'plpgsql';

// Function that returns table data.
CREATE OR REPLACE FUNCTION getStudentsList()
RETURNS TABLE(sname TEXT, sage integer) AS $$
BEGIN
  RETURN QUERY SELECT studentname,age FROM students;
END;
$$ LANGUAGE plpgsql;

// Constant
PI CONSTANT numeric :=  3.14;

// IF
IF A > B THEN
 str = 'A is greater';
ELSEIF A<B
 str = 'B is greater';
ELSE
 str = 'A is Equal to B';
END IF;

// CASE

CASE fruitName
WHEN ‘banana’ THEN
					taste = ‘Sweet’;
WHEN ‘peach’ THEN
					taste = ‘Sour’;
WHEN ‘avocado’ THEN
					taste = ‘Buttery’;
ELSE
					taste = ‘Nothing’;
END CASE;

RETURN taste;

// Loop
WHILE count <= n LOOP
count = counter + 1 ; 
<Your Logic>
END LOOP ;

// Drop Function: 
DROP FUNCTION addTen;
```


### References
- SQL Commands
	- https://www.postgresql.org/docs/10/sql-commands.html
