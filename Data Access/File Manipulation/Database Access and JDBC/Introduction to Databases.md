### Understanding Relational Databases

#### **What is a Relational Database?**
A **relational database** is a type of database that stores and provides access to data points that are related to one another. Data is organized into tables (also known as relations) consisting of rows and columns. Each table represents a specific entity (e.g., `Users`, `Products`), and each row in a table represents a record of that entity. Columns represent the attributes of the entity.

#### **Key Concepts:**
- **Table:** A collection of related data entries consisting of rows and columns.
- **Row (Record):** A single, implicitly structured data item in a table.
- **Column (Field):** A set of data values of a particular type, one for each row of the table.
- **Primary Key:** A unique identifier for a row within a table.
- **Foreign Key:** A field (or collection of fields) in one table that uniquely identifies a row of another table, establishing a relationship between the two tables.
- **Normalization:** The process of organizing data to minimize redundancy and improve data integrity.

#### **Example:**
Consider a simple `Users` table :
![[Pasted image 20241027142613.png]]
And a `Posts` table:![[Pasted image 20241027142635.png]]
**Explanation:**
- **Primary Key (PK):** `user_id` in `Users` and `post_id` in `Posts` uniquely identify each record.
- **Foreign Key (FK):** `user_id` in `Posts` references `user_id` in `Users`, establishing a relationship where each post is associated with a user.
### Basics of SQL
**SQL (Structured Query Language)** is the standard language for managing and manipulating relational databases. It allows you to perform various operations like querying data, updating records, creating tables, and managing permissions.
#### **Common SQL Commands:**
- **DDL (Data Definition Language):**
    - `CREATE`: Create new tables or databases.
    - `ALTER`: Modify existing database objects.
    - `DROP`: Delete tables or databases.
- **DML (Data Manipulation Language):**
    - `SELECT`: Retrieve data from one or more tables.
    - `INSERT`: Add new records to a table.
    - `UPDATE`: Modify existing records.
    - `DELETE`: Remove records from a table.
- **DCL (Data Control Language):**
    - `GRANT`: Give users access privileges.
    - `REVOKE`: Remove user access privileges.
### CRUD Operations

**CRUD** stands for **Create, Read, Update, Delete**, which are the four basic functions of persistent storage. In the context of databases:
- **Create:** Insert new records into tables.
- **Read:** Retrieve existing records from tables.
- **Update:** Modify existing records.
- **Delete:** Remove existing records.
