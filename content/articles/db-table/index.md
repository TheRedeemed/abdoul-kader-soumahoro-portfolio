---
title: "Database, table and data type"
description: "This article is a primer on databases and tables and how to create them. It also contains information about data types."
date: "2022-08-02"
banner:
  src: "../../images/maksym-kaharlytskyi-Q9y3LRuuxmg-unsplash.jpg"
  alt: "Database and table"
  caption: 'Photo by <u><a href="https://unsplash.com/photos/Q9y3LRuuxmg">Maksym Kaharlytskyi</a></u>'
categories:
  - "Database"
keywords:
  - "Database"
  - "Table"
  - "Column"
  - "Row"
  - "Data Types"
---

## Overview

Whether you purchased something online, performed an online search, booked a plane ticket or a hotel room, you have interacted with a database.

A database is used to store information. 
**It is a container that holds tables and other SQL structures related to those tables.**

**A table in a database is used to store information of the same type (related).** 
It contains **columns** and **rows**. 
A column could be best described as a category for your data and a row the collection of these categories for each record.

In other words:

**<u>A Column</u>**: Represents each *field* in the table

**<u>A Row</u>**: Represents an *instance* of each element in the the table

## Database

Imagine, we want to keep track of all our friends. Let's create a database for this. 

We will create the *"My_Social_Network"* database and a *"My_Friends"* table in that database.

In order to create our *"My_Social_Network"* database let's run the following command:

```sql
CREATE DATABASE my_social_network;
```

## Table

Before, we create the *"My_Friends"* table, let's think about the type of information we would like to store about our friends.

Let's say we want to store the following:

1. First Name
2. Last Name
3. Birthday
4. Email
5. Phone Number
6. Profession
7. Status
8. Location
9. Interests
10. Seeking

These will be the columns of the table and the collection of all these columns for each of our friends will be a row.

In order to create *"My_Friends"* table let's run the following command.

```sql
CREATE TABLE my_friends (
	first_name VARCHAR(50) NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	birthday DATE NOT NULL,
	email VARCHAR(200) NOT NULL,
	phone_number VARCHAR(50) NOT NULL,
	profession VARCHAR(50) NOT NULL,
	status VARCHAR(20) NOT NULL,
	location VARCHAR(50) NOT NULL,
	interest VARCHAR(200) NOT NULL,
	seeking VARCHAR(200) NOT NULL
);
```

## SQl Data Types

While creating our table, in addition to the column name, we specified a field type for each field. The fields in the *MyFriends* table are of type *VARCHAR* and *DATE*. However these are not the only data types available. 

Below is a list and description of data types in a Database:

**VARCHAR:** Used for Alphanumeric characters. The field has a variable length. Only the size of the given entry is used.

For example, saving "Test" in a VARCHAR(100) field only uses space for 4 characters.

- Min Size: 1
- Max Size: 4000

**CHAR:** Used for Alphanumeric characters. The field has a fixed length. The size of field is used regardless of the given entry.

For example, saving "Test" in a CHAR(100) field will use space for 100 characters even though "test" is only 4 characters.

- Min Size: 1
- Max Size: 2000

**NUMBER(p,s):** Used for numeric data. **p** is for precision (number of decimal digit) and **s** for scale (number of digit to the right decimal point)

- Integer
- Decimals
- Double precision
- Floating point

**DATE:** Used to store date and time related data.

- Date
- Time
- DateTime
- Timestamp

**CLOB:** It stands for **Character Large Object**. It is used to store variable length character data. The max size for this type is 4GB 

**BLOB:** It stands for **Binary Large object** and has a max size of 4GB. It is used for:

- File
- Image
- Document
- Audio
- Video

**BFILE:** This is used to store the location or link to an external file outside the DB. The max size is 4GB.

**RAW** and **LONG RAW:** Used to store Raw binary data. This type can be queried and inserted but it cannot be manipulated once it has been inserted
- RAW Max size: 2000
- Long RAW max size: 2GB

**BOOLEAN:** Used to store true or false values

## Conclusion

We now understand that a database is a container to hold tables and other related SQL structures.

The SQL command to create a database is: 

```sql
CREATE TABLE database_name;
```

<br>

We also learned that a table is an object in a database that contains columns (or fields) and rows (or instance of each element).

The SQL command to create a table is:

```sql
CREATE TABLE table_name (
  column_name TYPE
);
```
