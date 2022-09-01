---
title: "ALTER tables"
description: "This article is a primer on the different possible way to alter tables in Database"
date: "2022-08-31"
banner:
  src: "../../images/chris-lawton-5IHz5WhosQE-unsplash.jpg"
  alt: "ALTER Table"
  caption: 'Photo by <u><a href="https://unsplash.com/photos/5IHz5WhosQE">Chris Lawton</a></u>'
categories:
  - "Database"
keywords:
  - "ALTER"
  - "RENAME"
  - "CHANGE"
  - "MODIFY"
  - "ADD"
  - "DROP"
---

## Overview
A Database is container that holds tables and other objects related to those tables.
While creating a table we need to provide a table name, the fields for that table along with their types.

Depending on business requirement changes, the structure of an existing table may need to be updated. We may need to either add and/or remove fields, change the type and/or name of existing fields etc...

The **ALTER** command is used to perform these modifications. 

In this article, we will see various ways in which a table can be altered.

In order to run different ALTER commands, We will create the following table:

```sql
CREATE TABLE projekts (
	number INT,
	descriptionofproj VARCHAR(50),
	contractoronjob VARCHAR(10)
);
```

Let's assume we have the following requirement changes:

The **projekts** table is used to maintain a list of our projects. 

We would therefore like to give that table to a name that better reflects how the table is used.
Also, we will need to add a primary key with a unique project number in it. 

In addition, we'll need columns to do the following:

- Describe each project, 
- Store Start date, 
- Store Estimated cost, 
- Store the name, phone number1 and phone number2 of the contractor working on the project.

# RENAME

In order to have a name that better reflects how the table is used, we will rename it to **"project_list"**. 

The command below will be used to rename the table:

```sql
ALTER TABLE projekts
RENAME TO project_list;
```

# CHANGE

The **CHANGE** command can be used along with ALTER to **"change the name and data type"** of an exisiting column.

With the **ALTER** command below, we will change the **"number"** column into the primary key for the unique project number.

```sql
ALTER TABLE project_list
CHANGE COLUMN number project_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY;
```

Using the **ALTER** command below, we will also change the name and size of the **"descriptionofproj"** and **"contractoronjob"** columns.

```sql
ALTER TABLE project_list
CHANGE COLUMN descriptionofproj project_description VARCHAR(100),
CHANGE COLUMN contractoronjob contractor_name VARCHAR(100);
```

# MODIFY

A project definition might be longer that 100 characters, we therefore need to increase the size of the **"project_description"** column.

We will use the command below to update the size of the **project_description** column to 500 characters.

```sql
ALTER TABLE project_list
MODIFY COLUMN project_description VARCHAR(500);
```

# ADD

According to the new requirements, we need to add a few more fields to the **project_list** table.

We will used the command below to add **the start date**, **the estimated cost** and **the phone numbers** of the contractor.

```sql
ALTER TABLE project_list
ADD COLUMN start_date DATE,
ADD COLUMN contractor_phone VARCHAR(10) AFTER contractor_name,
ADD COLUMN estimated_cost DECIMAL(7,2),
ADD COLUMN contractor_phone2 VARCHAR(10);
```

<u>Note</u>: Keywords like **AFTER**, **BEFORE**, **LAST**, **FIRST**, **SECOND**, **THIRD**, can be used to specify where (in which order) we would like the field to be added.

# DROP

Assuming we decide that we only need one phone number column for the contractor, we can drop the other phone number column with the following command:

```sql
ALTER TABLE project_list
DROP COLUMN contractor_phone2;
```


# Conclusion

In this article we covered the possible ways to alter a table. 
We now know how to rename a table, change, modify, add and remove columns and their data types to a table.