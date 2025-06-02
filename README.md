# Library Management System
## Project Overview
### Project Title:Library Management System
### Database: Library-sql-project

This project demonstrates the implementation of a Library Management System using SQL. It includes creating and managing tables, performing CRUD operations and executing advanced sql queries. The goal is to showcase skills in database design, manipulation and querying.
![image alt](https://github.com/nsankareswari-70/Library-Management-System/blob/732226626012c53607ef7745c819438429a508b9/Libraryimg.png)

## Objectives

 #### 1.Set up the Library Management System Database:
 Create and populate the database with tables for branches, employees, members, books, issued status, and return status.
  #### 2.CRUD Operations: 
Perform Create, Read, Update, and Delete operations on the data.
 #### 3. CTAS (Create Table As Select): 
Utilize CTAS to create new tables based on query results.
 #### 4. Advanced SQL Queries: Develop complex queries to analyze and retrieve specific data.

## Project Structure
#### 1. Database setup

#### Database creation : 
Created a database named Library-sql-project
#### Table Creation : 
Created tables for branches, employees, members, books, issued status, and return status. Each table includes relevant columns and relationships.

```sql
-- creating branch table

DROP TABLE IF EXISTS branch;
CREATE TABLE branch
(
            branch_id VARCHAR(10) PRIMARY KEY,
            manager_id VARCHAR(10),
            branch_address VARCHAR(55),
            contact_no VARCHAR(15)
);

-- Create table "Employee"
DROP TABLE IF EXISTS employees;
CREATE TABLE employees
(
            emp_id VARCHAR(10) PRIMARY KEY,
            emp_name VARCHAR(30),
            position VARCHAR(30),
            salary DECIMAL(10,2),
            branch_id VARCHAR(10),
            FOREIGN KEY (branch_id) REFERENCES  branch(branch_id)
);


DROP TABLE IF EXISTS books;
CREATE TABLE books
(
            isbn VARCHAR(50) PRIMARY KEY,
            book_title VARCHAR(80),
            category VARCHAR(30),
            rental_price DECIMAL(10,2),
            status VARCHAR(15),
            author VARCHAR(35),
            publisher VARCHAR(55)
);

-- Create table "Members"
DROP TABLE IF EXISTS members;
CREATE TABLE members
(
            member_id VARCHAR(10) PRIMARY KEY,
            member_name VARCHAR(30),
            member_address VARCHAR(75),
            reg_date DATE
);

select * from members;

-- Create table "Books"
DROP TABLE IF EXISTS books;
CREATE TABLE books
(
            isbn VARCHAR(50) PRIMARY KEY,
            book_title VARCHAR(80),
            category VARCHAR(30),
            rental_price DECIMAL(10,2),
            status VARCHAR(15),
            author VARCHAR(35),
            publisher VARCHAR(55)
);


select * from books;

-- Create table "IssueStatus"
DROP TABLE IF EXISTS issued_status;
CREATE TABLE issued_status
(
            issued_id VARCHAR(10) PRIMARY KEY,
            issued_member_id VARCHAR(30),
            issued_book_name VARCHAR(80),
            issued_date DATE,
            issued_book_isbn VARCHAR(50),
            issued_emp_id VARCHAR(10),
            FOREIGN KEY (issued_member_id) REFERENCES members(member_id),
            FOREIGN KEY (issued_emp_id) REFERENCES employees(emp_id),
            FOREIGN KEY (issued_book_isbn) REFERENCES books(isbn) 
);

select * from issued_status;

-- Create table "ReturnStatus"
DROP TABLE IF EXISTS return_status;
CREATE TABLE return_status
(
            return_id VARCHAR(10) PRIMARY KEY,
            issued_id VARCHAR(30),
            return_book_name VARCHAR(80),
            return_date DATE,
            return_book_isbn VARCHAR(50),
            FOREIGN KEY (return_book_isbn) REFERENCES books(isbn)
);

alter table return_status
add constraint fk_issued_status
foreign key(issued_id)
references issued_status(issued_id);
```
