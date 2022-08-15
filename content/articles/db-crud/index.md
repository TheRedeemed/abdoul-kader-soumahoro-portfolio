---
title: "DML Operations"
description: "This article is a primer on the DML operations in a Database"
date: "2022-08-08"
banner:
  src: "../../images/campaign-creators-IKHvOlZFCOg-unsplash.jpg"
  alt: "DML Operations"
  caption: 'Photo by <u><a href="https://unsplash.com/photos/IKHvOlZFCOg">Campaign Creators</a></u>'
categories:
  - "Database"
keywords:
  - "DDL"
  - "DML"
  - "Insert"
  - "Update"
  - "Delete"
---

## Overview

A Database is a container that is used to store information. It holds tables and other SQL structures related to those tables.

**DDL** or **Data Definition Language** is used to create and modify tables and other SQL structures. After creating a database and tables, we can manipulate data within those tables with the help of **DML** Operations.

**DML** or **Data Manipulation Language** is used to perform different type of operations to manipulate data within a database. Those operations are:

- **I**nsert
- **U**pdate
- **D**elete.

Below is a DDL statement to create the *"my_friends"* table which we will use in order to learn more about **DML** operations.

```sql
CREATE TABLE my_friends (
	first_name VARCHAR(50),
	last_name VARCHAR(50),
	birthday DATE,
	email VARCHAR(200),
	phone_number VARCHAR(50),
	profession VARCHAR(50),
	status VARCHAR(20),
	location VARCHAR(50),
	interest VARCHAR(200),
	seeking VARCHAR(200)
);
```


## Insert

In order to insert data in a table, we use an **INSERT** statement. 
We will review different ways in which the INSERT statement can be used.

#### <u>INSERT with all column names</u>

The following command is used to insert data in the "my_friends" table:

```sql
INSERT INTO my_friends (first_name, last_name, birthday, email, phone_number, profession, status, location, interest, seeking)
VALUES ('Mike', 'Miller', '1980/05/16', 'm.miller@mail.com', '123-456-7788', 'Database Administrator', 'Married', 'Jacksonville, FL, USA', 'Coding, Reading, Sport', 'Friends');
```

**<u>Note</u>**: The number of values <u>has to match</u> the number of columns. Also the order of the columns does not matter as long as, the values is in the same order as the columns.

We switched the order of the columns *first_name* and *last_name* in the **INSERT** command below. We should expect the same result as the one above.

```sql
INSERT INTO my_friends (last_name, first_name, birthday, phone_number, email,  profession, status, location, interest, seeking)
VALUES ('James', 'Trevor', '1985/10/26', '321-216-1542', 'l.james@mail.com', 'Software Engineer', 'Married', 'Los Angeles, CA, USA', 'Coding, Reading', 'Friends');
```


#### <u>INSERT Without Column names</U>

It is also possile to run an INSERT command without specifying the column names. 
This type of INSERT is used when we want to insert data for ALL the columns in the table.

Here is an example:

```sql
INSERT INTO my_friends
VALUES ('Hamilton', 'Daniel', '1975/01/22', 'd.hamilton@mail.com', '215-425-4512', 'Surgeon', 'Single', 'Tampa, FL, USA', 'Martial Arts, Reading', 'Friends');
```


#### <u>INSERT Specific columns</u>

In some instances, we may just want to insert value for a few fields. 

The command below shows how to insert for specific fields:

```sql
INSERT INTO my_friends (first_name, last_name, birthday, email, phone_number, profession)
VALUES ('Dwight', 'Anderson', '1981/12/06', 'd.anderson@mail.com', '135-495-4216', 'Personal Trainer');
```

This works when other fields we want to omit in the **INSERT** command are **NULLABLE**.

However, if the table was created as follows:

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

Since, **"NOT NULL"** makes a field required. The statement we ran earlier to insert values for first_name, last_name, birthday, email, phone_number, profession will fail.

Thankfully, there's a workaround to prevent this failure to occur. While creating the table with the **NOT NULL** field, we can use the **DEFAULT** keyword to provide a default value for the **NOT NULL** fields. 

When a default value is provided, that default value will be inserted if a value is not provided in the insert.

With the following table:

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
	seeking VARCHAR(200) NOT NULL DEFAULT 'Friends'
);
```

When we run the INSERT statement below:

```sql
INSERT INTO my_friends (last_name, first_name, birthday, phone_number, email,  profession, status, location, interest)
VALUES ('Miller', 'Mike', '1980/05/16', '123-456-7788', 'm.miller@mail.com', 'Database Administrator', 'Married', 'Jacksonville, FL, USA', 'Coding, Reading, Sport');
```

We can see that even though a value for the column "seeking" was not provided, we will not get an error and the row will be inserted with the *"Friends"* value for the seeking column.

**<u>Note:</u>** The **DEFAULT** keyword is not only for **NOT NULL** fields. it can also be use for fields that are nullable.


#### <u>INSERT Specific columns from select</u>

Assuming we create another table *my_friends_new* and we need to populate the new table with the existing data in *my_friends* to our new table, 
we can use an *INSERT* statement along with a SELECT statement.

The command below will insert all the select fields from *my_friends* to *my_friends_new*

```sql
INSERT INTO my_friends_new (last_name, first_name, birthday, phone_number, email,  profession, status, location, interest)
SELECT last_name, first_name, birthday, phone_number, email,  profession, status, location, interest
FROM my_friends;
```


## UPDATE

#### <u>Basic Update</u>

The **UPDATE** is pretty straight forward.

The command below will update Mike's first_name:

```sql
UPDATE my_friends
SET first_name = 'Michael'
WHERE first_name = 'Mike'
AND last_name = 'Miller'; 
```

<u>Notes</u>:

- **UPDATE** is used to update one or many columns that meet the criteria in the **"WHERE"** clause.

- It is **HIGHLY** recommended to use **UPDATE** with a **WHERE** clause. If a **WHERE** clause is not provided, the column specified in the command will be updated for all the rows in the table.

For example, the following command will update the *first_name* of all the rows in the table to *"Michael"*

```sql
UPDATE my_friends
SET first_name = 'Michael'; 
```

- Multiple columns can be updated in the same command. The command below will update both the *first_name* and the *location*

```sql
UPDATE my_friends
SET first_name = 'Michael', location = 'Tampa, FL'
WHERE first_name = 'Mike'
AND last_name = 'Miller'; 
```

#### <u>Conditional Update </u>

Let's assume for that we want to add a new column to track whether our friends are over the age 40 or not.

We use the ALTER DML command to add the isOver40 field our table. Since the field is NOT NULL unless we pass a default value the command will fail. 
We will therefore a default value of *N*.

```sql
ALTER TABLE my_friends ADD isOver40 CHAR(1) DEFAULT 'N' NOT NULL;
```

Once the field is added the **UPDATE** command below will be used to set the correct value for each of our friend based on their age.

```sql
UPDATE my_friends
SET isOver40 =
CASE
	WHEN (current_date - birthday) >= 40  THEN 'Y'
	ELSE 'N'
END;
```

## Delete

Just like the **UPDATE**, the **DELETE** operation is pretty straight forward. 

The command below will be used to delete a row with *first_name* and *last_name* Mike Miller from "my_friends":

```sql
DELETE 
FROM my_friends
WHERE first_name = "Mike"
AND last_name = "Miller";
```

Here are a few things to <u>note</u> about the **DELETE** operation:

- Just like with the **UPDATE**, it is **<u>HIGHLY recommended</u>** to used the **DELETE** command with a **WHERE** clause. There can be as many conditions as needed in the **WHERE** clause. This ensures that specific rows are deleted from the table. Therefore one or many rows can be deleted at the same time.

- A **DELETE** command without a **"WHERE"** clause will delete aal the rows in the table. So **BEWARE!!!**

- **DELETE** is used to delete rows and it **cannot** be used to delete a single or multiple columns


## Conclusion

In this article, we took a look at the **DML** operations. 
We learned the different ways we can used the **INSERT** command to created or add data in a table. 
Then we learned how to safely **UPDATE** and **DELETE** data in a table.