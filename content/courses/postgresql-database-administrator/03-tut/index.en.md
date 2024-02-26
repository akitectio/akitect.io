---
categories:
  - database
date: 2023-11-19T08:00:00+08:00
description: PostgreSQL is an object-relational database management system (ORDBMS). This means it combines features of both relational databases (RDBMS) and object-oriented databases (OODBMS). The object model in PostgreSQL provides features such as user-defined data types, inheritance, and polymorphism, enhancing its capabilities beyond a typical SQL-based RDBMS.
draft: false
series:
  - postgresql-dba-series
featuredImage: /series/database/postgresql-dba-03-object-model-trong-postgresql.webp
images:
  - /postgresql-dba-03-object-model-trong-postgresql/images/index.png
  - /series/database/postgresql-dba-03-object-model-trong-postgresql.webp
license: <a rel=***license external nofollow noopener noreffer*** href=***https://creativecommons.org/licenses/by-nc/4.0/*** target=***_blank***>CC BY-NC 4.0</a>
tags:
  - Database
  - PostgreSQL
  - RDBMS Concepts
  - Object Model
  - ORDBMS
  - Object Model PostgreSQL
  - PostgreSQL DBA
title: Lesson 3 - Object Model PostgreSQL
url: /postgresql-dba-03-object-model-postgresql
weight: 3
---

## What is an Object Model?

PostgreSQL is an object-relational database management system (ORDBMS). This means it combines features of both relational databases (RDBMS) and object-oriented databases (OODBMS). The object model in PostgreSQL provides features such as user-defined data types, inheritance, and polymorphism, enhancing its capabilities beyond a typical SQL-based RDBMS.

### Database

A database is a collection of organized data that is stored and managed to provide information or support specific business activities. Databases are commonly used to store, retrieve, and manage data efficiently.

#### Creating a Database

To create a database, you can use the SQL **_CREATE DATABASE_** command or take advantage of PostgreSQL utilities like createdb. Here is an example of the SQL **_CREATE DATABASE_** command:

```sql
CREATE DATABASE HUMANRESOURCESMANAGEMENT;
```

#### Database Management

PostgreSQL provides several SQL commands and utilities for database management, including:

1. Listing databases: Use the **_\l_** command in the **_psql_** command-line interface, or execute the SQL statement **_SELECT name FROM pg_database;_**.

2. Switching databases: Use the **_\connect_** or **_\c_** command, followed by the database name in the **_psql_** command-line interface.

3. Renaming a database: Use the SQL statement **_ALTER DATABASE old_name RENAME TO new_name;_**.

4. Deleting a database: Use the SQL statement **_DROP DATABASE database_name;_** or the **_dropdb_** utility. Be careful when deleting a database, as it will permanently delete all of its data and objects.

#### Database Attributes

Each PostgreSQL database has several attributes that you can configure to adjust its behavior and performance, such as:

1. **_Encoding_**: Determines the character encoding used in the database. By default, PostgreSQL uses encoding similar to the server operating system (for example, UTF-8 on most Unix-based systems).
2. **_Collation_**: Determines the string sorting rules in the database. By default, PostgreSQL uses the default sorting rules of the server operating system.
3. **_Tablespaces_**: Controls where database files are stored on the file system. By default, PostgreSQL uses the server default tablespace. You can create additional tablespaces to store data on different disks or file systems, to improve performance or for backup purposes.

GitHub Copilot: Here is the English translation of your selected text:

### Table

In PostgreSQL, "Tables" are the basic data structure used to store and manage data. A table in PostgreSQL is a collection of rows and columns, where rows represent data records, and each column contains data for a specific field.

**_Example_** :

1. Creating a Table

```sql
CREATE TABLE employees (
   id SERIAL PRIMARY KEY,
   name VARCHAR(100),
   position VARCHAR(50),
   salary DECIMAL(10, 2),
   hire_date DATE
);
```

In this example, an employees table is created with the columns **_id_**, **_name_**, **_position_**, **_salary_**, and **_hire_date_**.

2. Adding data to the table

```sql
INSERT INTO employees (name, position, salary, hire_date)
VALUES ('John Doe', 'Software Engineer', 75000.00, '2021-01-15');
```

This code adds a row to the **_employees_** table.

3. Querying data from the table

```sql
SELECT * FROM employees;
```

This code queries all rows from the **_employees_** table.

### User-Defined Data Types

User-Defined Data Types in databases are a feature that allows users to create custom data types based on their specific needs. Instead of using standard data types available in the database management system (DBMS), users can define distinct and more complex data types to store data in the way they want.

Applications of User-Defined Data Types include:

1. Creating data types that align with the data model of a specific application.
2. Reducing dependence on standard data types and creating custom data types that help manage data more effectively.
3. Increasing the clarity and self-description of the database by using meaningful custom data type names in the data model.

For example, suppose you are developing a library management application and want to store information about books in the database. Instead of using standard data types like string or integer for book information fields, you could create a custom data type **_Book_** that includes the following fields:

- Title: String data type.
- Author: String data type.
- PublicationYear: Integer data type.
- ISBN: String data type.
- Genre: String data type.

You can define the **_Book_** data type in PostgreSQL as follows:

```sql
CREATE TYPE Book AS (
    Title       text,
    Author      text,
    PublicationYear integer,
    ISBN        text,
    Genre       text
);
```

Then, when you create a table to store book information, you can use the **_Book_** data type for a specific field:

```sql
CREATE TABLE Library (
    BookID serial PRIMARY KEY,
    BookInfo Book
);
```

When adding data to the Library table, you can use the **_Book_** data type to represent book information naturally and clearly:

```sql
INSERT INTO Library (BookInfo)
VALUES (
    ('The Great Gatsby', 'F. Scott Fitzgerald', 1925, '978-0-7432-7356-5', 'Novel')
);
```

Using User-Defined Data Types makes your database easier to read and manage, while providing consistency in your data model.

### Inheritance

In PostgreSQL, **_Inheritance_** is a feature that allows you to create a new data structure by inheriting from an existing table or data type. This feature allows you to create a linkage between tables or data types, enabling shared common information and optimizing data structure.

For example, consider a situation where you have a project management application and you want to track information about different types of jobs such as **_Development Jobs_**, **_Testing Jobs_**, and **_Deployment Jobs_**. Each of these job types has some unique information, but they also share some common information, such as **_Job Title_**, **_Job Description_**, **_Assignee_**, and **_Deadline_**.

You can use the inheritance feature to create a base data type called **_Job_** and then create child data types for each job type:

```sql
-- Create the base data type ***Job***
CREATE TABLE Job (
    JobID serial PRIMARY KEY,
    Title text,
    Description text,
    Assignee text,
    Deadline date
);

-- Create the child data type ***DevelopmentJob*** inheriting from ***Job***
CREATE TABLE DevelopmentJob (
    TechStack text,
    EstimatedHours integer
) INHERITS (Job);

-- Create the child data type ***TestingJob*** inheriting from ***Job***
CREATE TABLE TestingJob (
    TestCases text[],
    TestEnvironment text
) INHERITS (Job);

-- Create the child data type ***DeploymentJob*** inheriting from ***Job***
CREATE TABLE DeploymentJob (
    TargetServer text,
    DeploymentScript text
) INHERITS (Job);
```

When you use this inheritance feature, you can add data for each job type independently and query information about all jobs as the base **_Job_**. Any changes you make in the base **_Job_** data type will also affect all child data types.

For example, you can add a development job as follows:

```sql
INSERT INTO DevelopmentJob (Title, Description, Assignee, Deadline, TechStack, EstimatedHours)
VALUES ('Develop Feature A', 'Develop Feature A for the application', 'John Doe', '2023-12-31', 'React', 40);
```

This will create a new record in both the **_Job_** and **_DevelopmentJob_** tables simultaneously.

### Polymorphism

Polymorphism is the ability of a data object or function to perform different actions based on the type of object or input parameters. This means you can use the same method or function to manipulate different objects naturally, regardless of the object type.

For example, consider a situation where you have a database storing information about different types of animals, including **_Dogs_** and **_Cats_**. Each type of animal has some unique attributes such as **_Name_**, **_Age_**, and **_Sound_**. You want to perform a function to emit the sound of the animal without needing to know specifically whether it's a **_Dog_** or a **_Cat_**.

In SQL language, you can use polymorphism to accomplish this. Below is an example of how you can implement polymorphism in PostgreSQL:

```sql
-- Create a table to store animal information
CREATE TABLE Animals (
    AnimalID serial PRIMARY KEY,
    Name text,
    Age integer,
    Sound text
);

-- Create two records for Dog and Cat
INSERT INTO Animals (Name, Age, Sound)
VALUES ('Fido', 3, 'Woof');

INSERT INTO Animals (Name, Age, Sound)
VALUES ('Whiskers', 2, 'Meow');

-- Create a polymorphic function to emit the sound of the animal
CREATE OR REPLACE FUNCTION MakeSound(animal Animals) RETURNS text AS $$
BEGIN
    RETURN animal.Sound;
END;
$$ LANGUAGE plpgsql;
```

In this example, we created a table **_Animals_** to store animal information and added two records for **_Dog_** and **_Cat_**. Then, we created a function **_MakeSound_** that can perform polymorphism on the **_Animals_** object. This function returns the sound of the animal, and it can be used with any **_Animals_** object without needing to know the specific type of that animal.

Example of using the **_MakeSound_** function with both **_Dog_** and **_Cat_** animals:

```sql
SELECT Name, Age, MakeSound(Animals) AS AnimalSound FROM Animals;
```

The result will return a list of animals with their names, ages, and sounds, and the **_MakeSound_** function has been applied polymorphically to return the exact sound for each animal without needing to know in advance the type of that animal.
