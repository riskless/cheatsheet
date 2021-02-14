### insert
Create Database EmployeeDB
Go

Use EmployeeDB
Go

Create table Employees
(
	code nvarchar(50) primary key,
	name nvarchar(50),
	gender nvarchar(50),
	annualSalary decimal(18,3),
	dateOfBirth nvarchar(50)
)
Go

Insert into Employees values ('emp101', 'Tom', 'Male', 5500, '6/25/1988')
Insert into Employees values ('emp102', 'Alex', 'Male', 5700.95, '9/6/1982')
Insert into Employees values ('emp103', 'Mike', 'Male', 5900, '12/8/1979')
Insert into Employees values ('emp104', 'Mary', 'Female', 6500.826, '10/14/1980')
Insert into Employees values ('emp105', 'Nancy', 'Female', 6700.826, '12/15/1982')
Insert into Employees values ('emp106', 'Steve', 'Male', 7700.481, '11/18/1979')




















################################################################################
### JDBC
################################################################################
#  Java & database access : javavids
https://www.youtube.com/watch?v=17bdLbIaRhc&list=PL8BD670C8E74B1627

################################################################################
### mysql
################################################################################
*** Downloading and Installing MySQL
https://dev.mysql.com/downloads/mysql/

-- root account
root / zero

-- login
> mysql -u root -p
> exit

-- Creating MySQL Database
> show databases;
> create database mydb
> use mydb
> show tables;

-- Creating a new User
> create user 'harold'@'localhost' identified by password 'harold';
> grant all privileges on mydb.* to 'harold'@'localhost';
> flush privileges;
> alter user 'harold'@'localhost' identified with mysql_native_password by 'harold';
> exit
> mysql -u harold -p
> describe mytable;


CREATE USER 'harold'@'localhost' IDENTIFIED BY 'harold';

> select user,host from mysql.user;
> grant all privileges on *.* to 'someuser'@'localhost' with grant option;



grant all privileges on photo_app.* to 'harold'@'localhost';


drop user someuser;
flush privileges;

=============

spring.datasource.url=jdbc:mysql://localhost:3306/ngspringblog?useUnicode=true&useLegacyDatetimeCode=false&serverTimezone=UTC&createDatabaseIfNotExist=true&allowPublicKeyRetrieval=true&useSSL=false;

################################################################################
### Spring Data Support
################################################################################
-- Spring Data Support
https://www.youtube.com/watch?v=eR_JFtqyNL4&list=PL1A506B159E5BD13E&index=1





################################################################################
### Hibernate
################################################################################
-- Java Brains: Hibernate: ~12
https://www.youtube.com/watch?v=Yv2xctJxE-w&list=PL4AFF701184976B25   


################################################################################
### PostgreSQL
################################################################################
-- PostgreSQL Tutorial for Beginners
https://www.youtube.com/watch?v=FCa4HsG-hb4&list=PLS1QulWo1RIa-sDLWbP01sEnlm_Bxmvqs

> psql -U postgres
> \list -> \l
> create database test;
> \l
> drop database test;
> \c postgres;
create table employee(
name varchar(10),
id int,
primary key(id)
)

> \d
> \d employee

> drop table employee



### deploy javablog app in Heroku 
	<!-- Heroku -->
<!-- 	<bean class="java.net.URI" id="dbUrl">
		<constructor-arg value="#{ systemEnvironment['DATABASE_URL'] }" />
	</bean>

	<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
		<property name="url" value="#{ 'jdbc:postgresql://' + @dbUrl.getHost() + ':' + @dbUrl.getPort() + @dbUrl.getPath() }" />
		<property name="username" value="#{ @dbUrl.getUserInfo.split(':')[0] }" />
		<property name="password" value="#{ @dbUrl.getUserInfo.split(':')[1] }" />
	</bean>	 -->



### installation
==> go to a specific database: \c test
==> run cmd -> psql -U postgres -> CREATE DATABASE test 
==> run pgAdmin 


* intallation directory: C:\Program Files\PostgreSQL\10
* Data directory: C:\Program Files\PostgreSQL\10\data
* database superuser: postgres/zero
* port: 5432

* pre installation summary
Installation Directory: C:\Program Files\PostgreSQL\10
Server Installation Directory: C:\Program Files\PostgreSQL\10
Data Directory: C:\Program Files\PostgreSQL\10\data
Database Port: 5432
Database Superuser: postgres
Operating System Account: NT AUTHORITY\NetworkService
Database Service: postgresql-x64-10
Command Line Tools Installation Directory: C:\Program Files\PostgreSQL\10
pgAdmin4 Installation Directory: C:\Program Files\PostgreSQL\10\pgAdmin 4
Stack Builder Installation Directory: C:\Program Files\PostgreSQL\10

################################################################################
### DERBY
################################################################################
DERBY_HOME
Path

> startNetworkServer.bat
> stopNetworkServer.bat

> ij.bat
> connect 'jdbc:derby://localhost:1527/db;create=true';
> create table circle(id integer, name char(50));
> insert into circle values(1, 'First circle');
> select * from circle;

################################################################################
### SQLite
################################################################################
* database connection
package com.timbuchalka;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class Main {

    public static void main(String[] args) {

//        try(Connection conn = DriverManager.getConnection("jdbc:sqlite:/Volumes/Production/Courses/Programs/JavaPrograms/TestDB/testjava.db");
//            Statement statement = conn.createStatement()) {
//            statement.execute("CREATE TABLE contacts (name TEXT, phone INTEGER, email TEXT)");
        try {
            Connection conn = DriverManager.getConnection("jdbc:sqlite:/Volumes/Production/Courses/Programs/JavaPrograms/TestDB/testjava.db");
            Statement statement = conn.createStatement();
            statement.execute("CREATE TABLE contacts (name TEXT, phone INTEGER, email TEXT)");

            statement.close();
            conn.close();

//            Connection conn = DriverManager.getConnection("jdbc:sqlite:D:\\databases\\testjava.db");
//            Class.forName("org.sql.JDBC");

        } catch (SQLException e) {
            System.out.println("Something went wrong: " + e.getMessage());
        }
    }
}

################################################################################
### Oracle
################################################################################
# Setting up the SQL Development Environment
==> apex.oracle.com
Workspace Name: howtostudyora
First Name: Haraold
Last Name: Kim
Administrator Email: riskless2k@gmail.com


Workspace:	howtostudyora
Username:	riskless2k@gmail.com / zero0313
Environment:	https://apex.oracle.com/pls/apex/


################################################################################
### MongoDB
################################################################################


### tutorials

--MongoDB Tutorial for Beginners | Getting Started with MongoDB | MongoDB Training | Edureka
https://www.youtube.com/watch?v=CaKoJ9rFo8c

--MongoDB Tutorial for Beginners ( ProgrammingKnowledge)
https://www.youtube.com/watch?v=GtD93tVZDX4&list=PLS1QulWo1RIZtR6bncmSaH8fB81oRl6MP

-- MongoDB Crash Course 2019
https://www.youtube.com/watch?v=-56x56UppqQ




### installation


-- install
https://www.youtube.com/watch?v=FwMwO8pXfq0

- install path: C:\Program Files\MongoDB\Server\4.0\bin


> mongod
> mongo
> show dbs
> use mydb -> create database
> db.books.insert({"name":"mongodb book"})
> show dbs
> show collections;
> db.books.find()
> db.dropDatabase() -> delete database


> mongod // start server
> mongo		// start mongo shell
> use natours
> db.tours.insertOne({name:"The Forest Hiker", price:297,rating:4.7})
> db.tours.insertMany([{name:"The Sea Explorer", price:497, rating:4.8},{name:"The Snow Adventure",price:997,rating:4.9,difficulty:"easy"}])

> show dbs
> show collections
> quit()

> db.tours.find()
> db.tours.find({name:"The Forest Hiker"})

> db.tours.find({ price: {$lte: 500} })
> db.tours.find({ price: {$lt: 500}, rating: {$gte:4.8}  }) // and

> db.tours.find({ $or: [ {price: {$lt: 500}}, {rating: {$gte:4.8}} ]  }) // or

> db.tours.find({ $or: [ {price: {$lt: 500}}, {rating: {$gte:4.8}} ]  }, {name:1}) // projection

> db.tours.updateOne(
	{ name:"The Forest Hiker"}, 
	{ $set: {price:597}}
)

> db.tours.updateMany(
	{ price: {$gt: 200} }, 
	{ $set: {premium:true}}
)

> db.tours.replaceOne(...)
> db.tours.replaceMany(...)

> db.tours.deleteMany(
	{ rating: {$lt: 4.8} }
)

> db.tours.deleteMany({}) // delete all documents


> db.students.remove() -> remove all documents
> db.student.remove(
{"_id":ObjectId("id1")}
)
> db.student.remove(
{"age":"15"},
1 // only 1 removed
)


* projection 
It means selecting only necessary data rather than selecting whole of the data of document.
> db.COLLECTION_NAME.find({},{KEY:1})

> db.students.find()
> db.students.find(
{},
{"name":1}
)

> db.students.find(
{},
{"name":1, "_id":0} // don't select "_id" field
)

> db.students.find({}, {"name" : 1, "age":1, "_id":0}).limit(4)

> db.students.find({}, {"name" : 1, "age":1, "_id":0}).skip(2)

> db.students.find({}, {"name" : 1, "age":1, "_id":0}).skip(2).limit(3)

> db.students.find({}, {"name" : 1, "age":1, "_id":0}).sort({"name":1}) // 1:asc, -1:desc

* indexing
> use temp

for(i=0;i<1000000:++i) {
	db.posts.insert({student_id:i, "nmae":"Mark"});
}

> db.posts.find({"student_id" : 1000})
> db.posts.findOne({"student_id" : 1000})

> db.posts.ensureIndex{{"student_id":1}}

> db.posts.dropIndex{{"student_id":1}}

* aggregation
> use school
> db.students.find()
> db.students.aggregate([{
$group:{_id:"$gender", MyResult:{$sum:1}}
}])

> db.students.aggregate([{
$group:{_id:"$gender", MaxAge:{max:"$age"}}
}])

* restore and backup
> show dbs
> mongodump // create dump folder

> use mydb
> db.dropDatabase()
> use school
> show dbs
> mongorestore

> mongodumb --db mydb
> use mydb
> db.dropDatabase()
> show dbs

> mongorestore --db mydb dump/mydb
> show dbs

> use school
> show collections

> mongodump --db school --collection students

> db.students.drop()
> mongorestore --db school --collection students dump/school/students.bson

> show collections




### practice



* user: harold / fpnKAEVRAsXxpcy6
* mongo shell
mongo "mongodb+srv://cluster0-sfxqx.azure.mongodb.net/angular-node" --username harold

> use angular-node
> help
> show collections
> db.posts.find()


> mongod // start server
> mongo		// start mongo shell


2. natours
user: harold / uOqSBBuEwR8lajo1

-- compass 
mongodb+srv://harold:uOqSBBuEwR8lajo1@cluster0-zwnqu.mongodb.net/test

-- shell
mongo "mongodb+srv://cluster0-zwnqu.mongodb.net/test" --username harold

-- application
mongodb+srv://harold:<password>@cluster0-zwnqu.mongodb.net/test?retryWrites=true&w=majority



-- To get your Windows version
> wmic os get osarchitecture








* azure
mongodb+srv://harold:<password>@cluster0-sfxqx.azure.mongodb.net/test?retryWrites=true&w=majority

mongodb+srv://harold:fpnKAEVRAsXxpcy6@cluster0-sfxqx.azure.mongodb.net/angular-node?retryWrites=true&w=majority