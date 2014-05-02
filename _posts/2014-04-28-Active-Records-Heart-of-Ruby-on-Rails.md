---
layout: post
title:  "Active Records Heart of Ruby on Rails"
date:   2014-04-28 
desc: "Active Records the Heart of Ruby on Rails. where it controls the database of the application and play as a intereface between user and database for easy handling of database"

---


##SQL:

####SQL is language which talks to database 

Setup information is stored in specific file called Schema. and this is updated whenever structure of database is changed


####Statements:

+ SELECT
+ CREATE TABLE
+ DROP TABLE
+ CREATE INDEX
+ DROP INDEX
+ UPDATE
+ DELETE
+ INSERT INTO
+ CREATE DATABASE
+ DROP DATABASE
+ COMMIT (concept)
+ ROLLBACK (concept)
+ Clauses:

####DISTINCT

+ WHERE
+ IN
+ AND
+ OR
+ BETWEEN
+ LIKE
+ ORDER BY
+ COUNT
+ Functions

####GROUP BY

+ HAVING
+ AVG
+ COUNT
+ MIN
+ MAX
+ SUM



* CREATE INDEX will do all hardwork of sorting up the tables it will make database much faster.


####UPDATE QUERY:

```sh
    UPDATE Users 
    SET name='barfoo', email='bar@foo.com' 
    WHERE email='foo@bar.com';`
```

####JOINS :

+ Inner Joins : Keeps the common data matches in two tables.

```sh
Select * from users JOIN posts ON user_id = posts.user_id
```

+ Left Outer Joins: Keeps all rows in left table and joins the rows in right table which is common to left table.

+ Right Outer Joins: Keeps all data in right table and joins the rows in left table which is common to right table.


+ Full Outer Join: Keep all rows from all tables even if there are mismatch between them.set the mismactched rows to null.

```sh
SELECT * FROM users JOIN posts ON users.id = posts.user_id WHERE users.id = 42.
```

####WHERE WONT WORK ON AGGREGATE FUNCTION:

When using aggregate functions like 'count' 'max' 'min' to narrow down the records 'Where' wont work here we need to use 'Having'.

```sh
 SELECT users.name, COUNT(posts.*) AS posts_written
    FROM users
    JOIN posts ON users.id = posts.user_id
    GROUP BY users.name
    HAVING posts_written >= 10;
```

###WHY ACTIVE RECORD ??
 
 + What a good web developer needs is logical thinking breaking the problem in to small modules and then make it to one.

 + logical thinking is required when setting up data model properly. Data is the essential for a simple blog to complex facebook application. 

+ Active Record is the interface between database and web application which Rails provides us.

####What is ORM ?
+ Active Record Gem that takes care of database stuff its known as ORM.

+ ORM stands for Object Relational Mapping it takes care of database and allows user to interact the database using ruby objects.

So if I want to get an array containing a listing of all the users, instead of writing code to initiate a connection to the database, then doing some sort of SELECT * FROM users query, and converting those results into an array, I can just type User.all and Active Record gives me that array filled with User objects that I can play with as I'd like. Wow!


###Working with Models:

+ In order to create a new record in the table then Active Records provides Ruby objects 

```sh
u = User.new({:name => "Sven", :email => "sven@theodinproject.com"})
```

If you don't pass a hash, you'll need to manually add the attributes by setting them like with any other Ruby object: u.name = "Sven". The second step is to actually save that model instance into the database. Until now, it's just been sitting in memory and evaporates if you don't do anything with it. To save, simply call u.save. You can run both steps at once using the #create method:
```sh
u = User.create({:name => "Sven", :email => "sven@theodinproject.com"})
This saves you time, but, as you'll see later, you'll sometimes want to separate them in your application.
```

###MIGRATION :
+ Migrations are basically a scripts that tells rails how we need to set up/change  the database.we need to specify correct ruby method(create_table) and its parameters.

####Advantages:
* access database using user friendly ruby objects.
* if we want ot blow away database and re-run the migration its easy to do.
* no need to worry about what kind of database ,Active Record will take care of those stuffs.
* easy to rollback using rake db:rollback
* or can create remove_column method in new migration.

###VALIDATION

Validation is nothing but verifying whether right data gets in to database  by the authorized people.

###3 Kinds of Validation

+ Client side validation using javascript to check whether username field is present but this can be easily hacked my unauthorized user.
+ server side validation in Model of MVC this fails when there are multiple servers running with centralized database and here if two servers enters with same data at same time then there will be a mess in DB.
+ Validation done in Database is best way.