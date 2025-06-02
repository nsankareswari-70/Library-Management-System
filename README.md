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
#### 2. CURD Operations
#### Create: 
Inserted sample records into the books table.
#### Read: 
Retrieved and displayed data from various tables.
#### Update: 
Updated records in the Mambers table.
Delete: Removed records from the Issued Status Table

### Questions
#### Question 1:
Create a New Book Record -- "978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.')"
```sql
INSERT INTO books(isbn, book_title, category, rental_price, status, author, publisher)
VALUES
('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.');
SELECT * FROM books;
```
#### Question 2:
Update an Existing Member's Address
```
UPDATE members
SET member_address = '125 Main St'
WHERE member_id = 'C101';
SELECT * FROM members;
```
####  Question 3: 
Delete a Record from the Issued Status Table  
Objective: Delete the record with issued_id = 'IS121' from the issued_status table.
```sql
select * from issued_status where issued_id='IS121';

DELETE FROM issued_status
WHERE issued_id = 'IS121';
```
#### Question 4:
Retrieve All Books Issued by a Specific Employee   
Objective: Select all books issued by the employee with emp_id = 'E101'.
```sql
SELECT * FROM issued_status
WHERE issued_emp_id = 'E101';
```
#### Question 5:
List Members Who Have Issued More Than One Book  
 Objective: Use GROUP BY to find members who have issued more than one book.
 ```sql
 select issued_emp_id, count(*) as "Number of books issued" from issued_status
 group by issued_emp_id having count(*)>1 order by issued_emp_id ;
 ```
 #### Question 6:
 Create Summary Tables: Used CTAS to generate new tables based on query results - each book and total times issued
 ```sql
create table issue_count as
select b.isbn,book_title,count(*)as total_issued from books b
join issued_status iss on
b.isbn = iss.issued_book_isbn
group by b.isbn,book_title;

select * from issue_count;
```

#### Question 7:
 Retrieve All Books in a Specific Category:
```sql
SELECT * FROM books
WHERE category = 'Classic';
```
#### Question 8:
Find Total Rental Income by Category:

```sql
SELECT
    b.category,
    SUM(b.rental_price),
    COUNT (*) as Bookcount
FROM books as b
JOIN
issued_status as ist
ON ist.issued_book_isbn = b.isbn
GROUP BY 1
```
#### Question 9
 List Members Who Registered in the Last 180 Days:
```sql
SELECT * FROM members
WHERE reg_date >= CURRENT_DATE - INTERVAL '180 days'  ; 
```

#### Question 10
  List Employees with Their Branch Manager's Name and their branch details:
```sql
SELECT 
    e1.*,
    b.manager_id,
    e2.emp_name as manager
FROM employees as e1
JOIN  
branch as b
ON b.branch_id = e1.branch_id
JOIN
employees as e2
ON b.manager_id = e2.emp_id

```
### Question 11. 
Create a Table of Books with Rental Price Above a Certain Threshold 7USD
```sql
CREATE TABLE books_price_greater_than_seven
AS    
SELECT * FROM Books
WHERE rental_price > 7

select * from books_price_greater_than_seven;
```
#### Question 12: Retrieve the List of Books Not Yet Returned
```sql
SELECT 
    DISTINCT ist.issued_book_name
FROM issued_status as ist
LEFT JOIN
return_status as rs
ON ist.issued_id = rs.issued_id
WHERE rs.return_id IS NULL

```
