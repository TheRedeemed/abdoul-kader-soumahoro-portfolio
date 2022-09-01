---
title: "SELECT data from a Database"
description: "This article is a primer on the different possible way to select data in Database"
date: "2022-08-28"
banner:
  src: "../../images/fahrul-azmi-cFUZ-6i83vs-unsplash.jpg"
  alt: "SELECT data"
  caption: 'Photo by <u><a href="https://unsplash.com/photos/cFUZ-6i83vs">Fahrul Azmi</a></u>'
categories:
  - "Database"
keywords:
  - "SELECT"
  - "LIKE"
  - "IN"
  - "NOT IN"
  - "COUNT"
  - "ORDER BY"
  - "SUM"
  - "AVG"
  - "MIN"
  - "MAX"
  - "GROUP BY"
  - "LIMIT"
  - "ROUND"
  - "FLOOR"
  - "CEIL"
---

## Overview
A database is a container that holds tables and SQL structures related to those tables.
While we can insert, update or delete data, we can also retrieve the data stored in a database.
In this post we learn different ways to retrieve data.

We will create the following table:

```sql
CREATE TABLE my_friends (
	first_name VARCHAR(50),
	last_name VARCHAR(50),
	birthdate DATE,
	email VARCHAR(50),
	city VARCHAR(50),
	state CHAR(2)
);
```

We will insert some data in our table with the INSERT statement below:

```sql
INSERT INTO my_friends
VALUES 
('Andrew', 'James', '1987/02/15', 'a.james@email.com', 'Jacksonville', 'FL'),
('Adam', 'Davis', '1988/11/14', 'a.davis@email.com', 'New Orleans', 'LA'),
('Anthony', 'Williams', '1980/04/12', 'a.williams@email.com', 'Tampa', 'FL'),
('Anthony', 'Fisher', '1990/04/18', 'a.fisher@email.com', 'Dallas', 'TX'),
('Robert', 'Rodriguez', '1975/10/26', 'r.rodriguez@email.com', 'San Fransico', 'CA'),
('Brian', 'Apple', '1981/09/16', 'b.apple@email.com', 'Cincinnati', 'OH'),
('Kim', 'Jackson', '2000/05/19', 'k.jackson@email.com', 'Atlanta', 'GA'),
('Larry', 'O\'Connor', '1970/07/09', 'l.oconnor@email.com', 'Raleigh', 'NC'),
('Tim', 'Washer', '1965/07/09', 't.washer@email.com', 'Houston', 'TX'),
('Jim', 'Grear', '1982/07/09', 'j.grear@email.com', 'San Antonio', 'TX');
```

# SELECT ALL

The select all is a very basic command that allows us to query all the rows in a table. In the command below, the '*' means **'ALL'**.

```sql
SELECT * FROM my_friends;
```

The result of this query will print all the rows and columns currently present in the table. 

This works well without any performance concern when our table has a few rows.
However, most tables in application we use everyday have millions upon millions of record. It would take a really long time to retrieve millions of rows.
A more efficient option would be to narrow down the result of our **SELECT** statement with specific search criteria.

# SELECT with search criteria

The query below will return all the rows where the *first_name* of our friens is 'Anthony'

```sql
SELECT * 
FROM my_friends
WHERE first_name = 'Anthony';
```

In some instances, we may not need all the columns.
Let's use the query below to retrieve the *first_name*, *last_name* and *email* for all our friends with first_name as Anthony.

```sql
SELECT first_name, last_name, email
FROM my_friends
WHERE first_name = 'Anthony';
```

<u>Note:</u> 

1. It is possible to use either the **UPPER** or **LOWER** function to ensure the correct data is retrieved regarless of the case it was enterded in. 

The query below shows the use of the UPPER function.

```sql
SELECT first_name, last_name, email
FROM my_friends
WHERE UPPER(first_name) = UPPER('Anthony');
```

2. In cases where the string in the search criteria contains an apostrophe, we need to use an escape character to ensure the query runs correctly. 
In order to Larry's information based on his last name, we will do the following:

```sql
SELECT first_name, last_name, email
FROM my_friends
WHERE last_name = 'O\'Connor';
```

The same result can be retrieved with the query below.

```sql
SELECT first_name, last_name, email
FROM my_friends
WHERE last_name = 'O''Connor';
```

The backslash **" \ "** and the extra apostrophe **" ' "** are called escape characters. The backslash " \ " tends to be the preferred escape option as it is easy to spot in a query.

# SELECT with multiple search criterias and comparison operators

So far we only had one search criteria with the equal **"="** comparison operator. 
In some instances we may need more than one earch criterias and a comparison operator other than equal operator.

The query below will return the first_name, last_name, age, city and state of our friends who are at least 30 years old or who don't live in Florida.

```sql
SELECT 
	first_name, 
	last_name, 
    EXTRACT(YEAR FROM current_date()) - EXTRACT(YEAR FROM birthdate) as age,
	city, 
	state
FROM my_friends
WHERE EXTRACT(YEAR FROM current_date()) - EXTRACT(YEAR FROM birthdate) >= 35
OR state <> 'CA';
```

<u>Note:</u>  
 1. We use double arouond the search criteria quote ' ' when the field for that criteria is a string. 
 2. The **"as age"** in the query is called an alias, we use the **"as"** keyword to tell SQL to put the result of the calculation in the alias "age".
 3. The **EXTRACT** is a function used to extract a specific unit in a date. In this example, we extract the year from the current date and the birth year to calculate the age.
 4. It would have been ideal to use the alias column in the **WHERE** clause. However, this is not allowed in SQL because the value of the alias column may not be available by the time the **WHERE** clause is evaluated. This is why the EXTRACT command is repeated in the **WHERE** clause.
 5. In this query, we used the **"OR"** logical operator to add an additional search criteria. 
    This means that either of the given conditions need to be satisfied. In case we want all condition to be satisfied, we use the *"AND"* logical operator.
 6. See below a table containing the comparison operators with their definitions
 
    | Operator      |  Definition             |
	| :-----------: | :---------------------: |
	|    =          |  Equal                  |
	|    <>         |  Not Equal              |
	|    <          |  Less Than    		  |
	|    >          |  Greater Than           |
	|    <=         |  Less Than Or Equal     |
	|    >=         |  Greater Than Or Equal  |

# SELECT with LIKE

At times we may need to retrieve rows with a search criteria that's based on a specific pattern. In these cases, we can use wildcard characters like **'%'** or **'_'**. 

**%** : is used for any number of unknown characters

**_** : is used for one unknown character


In order to find all of our friends whose first_name starts with 'A', We would ran the following query:

```sql
SELECT first_name, last_name
FROM my_friends
WHERE first_name LIKE 'A%';
```

In case we want to find all our friends whose name end with with "im" and we just don't know the first character, we would do the following:

```sql
SELECT first_name, last_name
FROM my_friends
WHERE first_name LIKE '_im';
```

# SELECT with IN and NOT IN keywords

The IN keyword is used for a select command that checks if the criteria is in a list of values. NOT IN is the opposite of IN

The command below returns the first name and last name of all our friends who live in FL and TX

```sql
SELECT first_name, last_name, city, state
FROM my_friends
WHERE state IN ('FL', 'TX');
```

To find friends who do not live in FL, TX and CA, we would run the following query

```sql
SELECT first_name, last_name, city, state
FROM my_friends
WHERE state NOT IN ('FL', 'TX', 'CA');
```

# SELECT with ORDER BY

It is possible to retrieve data from a table with a specific order.

The query below will return data in our table ordered by the last_name.

```sql
SELECT first_name, last_name
FROM my_friends
ORDER BY last_name;
```

By default, the result of the query above will be in *ascending order*. In order to return the result of our queries in descending order, we use the keyword **DESC**.

```sql
SELECT first_name, last_name
FROM my_friends
ORDER BY last_name DESC;
```

We can also order with multtiple fields. The query below returns our friends list order by state and then by last_name.

```sql
SELECT first_name, last_name, city, state
FROM my_friends
ORDER BY state, last_name;
```

# SELECT with Aggregate functions

We will use the following scripts to create a table and insert data in that table:

```sql
CREATE TABLE sales_tracker (
	seller_name VARCHAR(50),
	sale DEC(7,2),
	sale_date DATE
);
```

```sql
INSERT INTO sales_tracker
VALUES 
	('Lindsay', 32.02, '2007-3-6'),
	('Paris', 26.53, '2007-3-6'),
	('Britney', 11.25, '2007-3-6'),
	('Nicole', 18.96, '2007-3-6'),
	('Lindsay', 9.16, '2007-3-7'),
	('Paris', 1.52, '2007-3-7'),
	('Britney', 43.21, '2007-3-7'),
	('Nicole', 8.05, '2007-3-7'),
	('Lindsay', 17.62, '2007-3-8'),
	('Paris', 24.19, '2007-3-8'),
	('Britney', 3.40, '2007-3-8'),
	('Nicole', 15.21, '2007-3-8'),
	('Lindsay', 0, '2007-3-9'),
	('Paris', 31.99, '2007-3-9'),
	('Britney', 2.58, '2007-3-9'),
	('Nicole', 0, '2007-3-9'),
	('Lindsay', 3.24, '2007-3-10'),
	('Paris', 13.44, '2007-3-10'),
	('Britney', 8.78, '2007-3-10'),
	('Nicole', 26.82, '2007-3-10'),
	('Lindsay', 3.71, '2007-3-11'),
	('Paris', .56, '2007-3-11'),
	('Britney', 34.19, '2007-3-11'),
	('Nicole', 7.77, '2007-3-11'),
	('Lindsay', 16.23, '2007-3-12'),
	('Paris', 0, '2007-3-12'),
	('Britney', 4.50, '2007-3-12'),
	('Nicole', 19.22, '2007-3-12');
```

#### SELECT Count

In order to count the number of rows in a table, we can use the **COUNT** function.

The query below will give us the number of rows in the my_friends table

```sql
SELECT COUNT(*)
FROM my_friends;
```

#### SUM

In the query below we will use the **SUM** function to get sum of all the sales for a given seller.

```sql
SELECT seller_name, SUM(sale) as total_sales
FROM sales_tracker
WHERE seller_name = 'Lindsay';
```

#### SUM with GROUP BY

The query below will use the **SUM** and **GROUP BY** functions to get the total_sales for each user in the table.

We will also display the result in descending order by sales

```sql
SELECT seller_name, SUM(sale) as total_sales
FROM sales_tracker
GROUP BY seller_name
ORDER BY total_sales DESC;
```

#### AVG with GROUP BY

In order to get the average sale for each seller we will write the following query:

```sql
SELECT seller_name, AVG(sale)
FROM sales_tracker
GROUP BY seller_name;
```

#### ROUND

When getting the average sales for each users, we can round the result by a specific scale.

The query below will give us the average sales for each seller at a scale of 2 decimals

```sql
SELECT seller_name, ROUND(AVG(sale),2)
FROM sales_tracker
GROUP BY seller_name;
```

<u>Note:</u> The ROUND function will also round up to the nearest integer if the decimal value is greater than or equal to .5

<u>Here are a few examples:</u>

1. ROUND(11.711429, 2) = 11.71
	- The scale is 2. 
	- The first 2 digits after the decimal point are .71
	- The value after .71 is 1 
	- And 1 < 5

2. ROUND(11.711429) = 12
	- The scale is 0. 
	- The first digit after the decimal point is .7
	- And 7 > 5

3. ROUND(15.415714, 2) = 15.42 
	- The scale is 2. 
	- The first 2 digits after the decimal point are .41
	- The value after .41 is 5 
	- And 5 = 5

4. ROUND(15.414714, 2) = 15.41 
	- The scale is 2. 
	- The first 2 digits after the decimal point are .41
	- The value after .41 is 4 
	- And 4 < 5

5. ROUND(15.415714) = 15 
	- The scale is 0. 
	- The first digit after the decimal point is .4, 
	- And 4 < 5 

6. ROUND(13.718571, 2) => 13.72 
	- The scale is 2. 
	- The first 2 digits after the decimal point are .71, 
	- The value after .71 is 8 
	- And 8 < 5

7. ROUND(13.718571) => 14 
	- The scale is 0. 
	- The first digit after the decimal point is .7
	- And 7 > 5

#### FLOOR

The **FLOOR** function returns the largest integer value which is less than or equal to a number

The query below will return the integer value of the average sales for each seller

```sql
SELECT seller_name, FLOOR(AVG(sale))
FROM sales_tracker
GROUP BY seller_name;
```

<u>Other Examples</u>

1. SELECT FLOOR(12.45)  = 12

2. SELECT FLOOR(12.98)  = 12

3. SELECT FLOOR(12.13)  = 12

4. SELECT FLOOR(-21.53) = -22

#### CEIL

The CEIL functions returns the smallest integer value which is greater than or equal to a number

<u>Examples:</u>

1. SELECT CEIL(21.53)  = 22

2. SELECT CEIL(21.23)  = 22

3. SELECT CEIL(-21.53) = -21

#### MIN and MAX with GROUP BY

The **MIN** function returns the smallest value in a set of values while the **MAX** functions returns the biggest value. 
They both return a numeric value.

To find out the minimum and maximum sales for each seller we will do the following:

```sql
SELECT seller_name, MIN(sale) as lowest_sale, MAX(sale) as highest_sale
FROM sales_tracker
GROUP BY seller_name;
```

The **GROUP BY** keyword is used to find the min and max sales for each of seller_name.
In order word, the query will group all the rows where seller_name is the same and find the **MIN** and **MAX** for that seller.

#### LIMIT

The **LIMIT** keyword is used to return a specific number of rows. To see to see the top 3 sellers, we will use the following query:

```sql
SELECT seller_name, SUM(sale) as total_sales
FROM sales_tracker
GROUP BY seller_name
ORDER BY total_sales DESC
LIMIT 3;
```

This query gets the sum of all sales for each seller. After ordering the result in descending order, it return the first 3 rows.
Since the result set is in descending order, the first 3 rows will be the top 3 sellers.


If we only want to see the second top seller, We will use the following query:

```sql
SELECT seller_name, SUM(sale) as total_sales
FROM sales_tracker
GROUP BY seller_name
ORDER BY total_sales DESC
LIMIT 1,1;
```

When we have two arguments with the **LIMIT** keyword, the first indicates the index to start from and the second number indicate the number of row we want to return.
Let's say we had a list with 100 records and we wanted to return the 10th through the 20 record, we would use *LIMIT* 9, 10.

# Conclusion

In this article we learned different ways to create a **SELECT** statement to query a database. We learned that we can use one or many search criterias along with multiple comarison operators. We also learned about wilcards in our search criteria. In addition we learned about ordering the result of a query. Finally we went over many aggregate functions available to perform different calculation in the SELECT statement.