

### 1. RDBMS

* Entity - is anything we store data about, as well as the types of data we store such as their name, their lastname password and more.
* Attribute - is the data that we store
* If we combined Entitty and Attribute together we get Attribute types and Attribute values that gets stored into tables
  * Attribute types 
    * name
    * username
    * password 
  * Attribute values
    * Patrick
    * PatrickSunico
    * PS123

```ruby
------------------------------------
    name      username         pw   -- > Attr. types
#1. Patrick   PatrickSunico    PS123 --> Attr. values for Entity Patrick
#2. Bill      BillyJoe         pizza --> Attr. values for Entity Bill
------------------------------------
```

#### Entity type

*  An Entity type can be summed up as a category on which the entities that were stored.
*  From the above example, an example of an Entity can be called Patrick or Bill, Or something else. But when it comes to an Entity type we are refering from the table something that relates Patrick or Bill together which we can call a "user" or a "customer" inside the table above. In summary

#### Attribute type

* The attribute types are basically the categories of the attribute values.

#### Database Design

* Data integrity - is a design pattern that most developers follow to make sure if your data connectivity and asociations are correct.
* Conceptual Schema - how we think, on how to design our database to cater our own usecase.
* Logical Schema - how the data relates to each other, or how table rows can be matched together to form a singular joined table. 
* Physical Schema - how we're going to implement the actual concept and logic of our database

#### Types of Data Integrity

* Entity Integrity - ensures that no two rows in a table can have the exact same values in all the columns.
  Domain Integrity - ensures that the values in a table must be within a specified range. It is enforced by restricting the range, type and format of data. I.E a group of employees for recruitment only from 20-50 years old.
* Referential Integrity - ensures consistency in data among tables in a database when records are modified. I.E an assigned task for an employee once finished will be deleted including all reference to the completed project.
* User Defined Integrity - refers to the rules defined by a user.



#### Integrity Constraints

* **Unique Constraint** -  ensures that a column or a group of columns in a table does not contain duplicate values. Enforces entity integrity in the table.
* **CHECK Constraint** - restricts the values entered in a column by defining a valid range or a valid format for entered values. This enforces domain integrity in the table.
* **PRIMARY KEY Constraint** - PRIMARY KEY constraint defined on a column ensures that each record of the column is unique. This ensures entity integrity in the table.
* **FOREIGN KEY Constraint** - creates logical relationships between multiple tables of a database. This ensures referential integrity. 



#### SQL

* **SELECT** - extracts data from a database
* **UPDATE** - updates data in a database
* **DELETE** - deletes data in a database
* **INSERT INTO** - inserts new data into a database
* **CREATE DATABASE** - creates a new database
* **ALTER DATABASE** - modifies a database
* **DROP TABLE** - deletes a table 
* **CREATE INDEX** - creates an index(search key)
* **DROP INDEX** - deletes an index



#### SQL Common SYNTAXES

##### 4 Commonly used syntaxes in handling databases

```sql
/* Shows all databases*/
SHOW DATABASES;
/*Creates a new database*/
CREATE DATABASE db_name;
/*USE database*/
USE db_name;
/*delete database*/
DROP DATABASE db_name;
```

##### Creating a New MySQL User and give that user privelage to our database

```sql
/*Grants all privileges on assigned username*/
/* ".*" means all tables on db_name */
GRANT ALL PRIVILEGES ON db_name.*
TO 'username'@'localhost'
IDENTIFIED BY 'passwordwithquotes';
SHOW GRANTS FOR 'username'@'localhost';
```

##### Display all tables and access those tables

```sql
# Shows all tables
SHOW TABLES;

# Shows all fields from a specific table
SHOW FIELDS FROM table_name;
```

##### SQL SELECT Syntax

```sql
/*SELECT Individual columns*/
SELECT column_name, column_name

/*All the tables*/
SELECT * FROM table_name;
```

##### 

##### To login back to our assigned database

```sql
mysql -u "username" -p
```

#### Common SQL Terminologies

- Column - Set of a single simple type
  - Usually means a type: strings, integers, etc.
- Row - means the data, each row is equal to an object or an instance of a model
  - Single Record of data
  - Example: "Patrick","Sunico","PS@gmail.com"
- Index - Data structure on a table to increase look-up speed, Like the index at the back of a book
- Foreign key - Table column whose values reference rows in another table. They relate rows into one table into another.
- Schema - structural definition of a database, It defines the tables, the columns, the rows, the indexes, basically everything that makes up the database.




