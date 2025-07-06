# SQL Reference Guide

A **database** is a collection of data stored in a format that can be easily accessed, managed, and updated.

A **Database Management System (DBMS)** is a software system used to manage and manipulate data in databases.

A **Relational DBMS (RDBMS)** stores information in the form of **tables**.

- Example: `classicmodels.employees` → `classicmodels` is the database, `employees` is the table.

---

## 📘 SQL vs NoSQL

| Feature | SQL (Relational) | NoSQL (Non-Relational) |
|--------|------------------|------------------------|
| Scalability | Vertically scalable | Horizontally scalable |
| Schema | Fixed schema | Dynamic schema |
| Storage | Tables (rows & columns) | Documents, Key-Value, Graph, Column |

---

## 🟦 SELECT Statements

### Basic Syntax

```sql
SELECT [column1, column2] FROM table_name;

🟩 SELECT Clause with Alias
SELECT MRP AS "Selling Price";
SELECT (MRP * 0.90 + 1) AS "Discount Price";


🟨 WHERE Clause

SELECT * FROM orders WHERE status != 'shipped';
-- or use <>

🟥 Logical Operators
SELECT * 
FROM payments 
WHERE NOT (amount >= 40000 AND amount <= 50000);

SELECT * 
FROM payments 
WHERE (amount <= 40000 OR amount >= 60000) 
AND paymentDate >= '2005-05-25';

🟦 IN Operator
SELECT * FROM employees 
WHERE officeCode IN (1, 2, 4)
ORDER BY officeCode;
SELECT * FROM employees 
WHERE officeCode NOT IN (1, 2, 4)
ORDER BY officeCode;


🟩 BETWEEN Operator

SELECT * FROM customers 
WHERE creditLimit BETWEEN 20000 AND 40000;

🟨 LIKE Operator

-- Starts with
SELECT * FROM employees WHERE firstName LIKE 'A%';

-- Ends with
SELECT * FROM employees WHERE firstName LIKE '%y';

-- 3rd letter is 'a'
SELECT * FROM employees WHERE firstName LIKE '__a%';


🟥 REGEXP Operator
Syntax	Meaning
^	Starts with
$	Ends with
`	`
-- Contains 'sale'
SELECT * FROM employees WHERE jobTitle REGEXP 'sale';

-- Starts with 'sale'
SELECT * FROM employees WHERE jobTitle REGEXP '^sale';

-- Ends with 'rep'
SELECT * FROM employees WHERE jobTitle REGEXP 'rep$';

-- Starts with J or L
SELECT * FROM employees WHERE jobTitle REGEXP '^J|^L';

-- Starts with a-h or ends with 'lie'
SELECT * FROM employees WHERE jobTitle REGEXP '^[a-h]|lie$';


🟦 IS NULL Operator
SELECT * FROM orders WHERE comments IS NULL;
SELECT * FROM orders WHERE shippedDate IS NOT NULL;

🟩 ORDER BY Clause
-- Descending sort
SELECT contactLastName, contactFirstName 
FROM customers 
ORDER BY city DESC;

-- Multiple fields
SELECT customerName, contactLastName, contactFirstName, city 
FROM customers 
ORDER BY city DESC, contactLastName;

🟨 LIMIT Clause
-- Top N rows
SELECT * FROM employees LIMIT 5;

-- Skip N rows, then get M
SELECT * FROM employees LIMIT 5, 10;
🔁 Querying Multiple Tables
INNER JOIN
SELECT t1.column1, t2.column2 
FROM table1 t1 
INNER JOIN table2 t2 ON t1.id = t2.id;

JOIN with Alias
SELECT o.orderNumber, o.status, o.customerNumber,
       e.firstName AS "Sales Person Name", e.jobTitle
FROM orders o
JOIN customers c ON o.customerNumber = c.customerNumber
JOIN employees e ON c.salesRepEmployeeNumber = e.employeeNumber;

🔂 SELF JOIN
SELECT emp.employeeNumber, emp.firstName, emp.jobTitle,
       mgr.firstName AS "Manager Name", mgr.jobTitle AS "Manager Title"
FROM employees emp
JOIN employees mgr ON emp.reportsTo = mgr.employeeNumber;

🧩 IMPLICIT JOIN (CROSS JOIN)
SELECT p.customerNumber, paymentDate, amount, customerName
FROM payments p, customers c
WHERE p.customerNumber = c.customerNumber;


🔄 OUTER JOINs
1. INNER JOIN:
-- Only matched records from both tables
2. LEFT JOIN:
-- All records from left table, matched from right

3. RIGHT JOIN:
-- All records from right table, matched from left

4. FULL OUTER JOIN:
-- All records from both sides with NULLs where no match

💡 Venn Diagram Reference
Inner Join: Intersection

Left Join: Entire left table

Right Join: Entire right table

Full Join: Union of both tables

🔁 SELF OUTER JOIN
Useful for hierarchies or incomplete data.
-- Similar to self join but ensures inclusion of all rows

🔤 USING Clause
SELECT * 
FROM orders 
JOIN customers 
USING (customerNumber);

🌐 NATURAL JOIN:
SELECT orderNumber, customerNumber, customerName
FROM orders
NATURAL JOIN customers;


✅ Summary
Use SELECT to fetch data

Use JOINs to combine multiple tables

Use WHERE, LIKE, REGEXP, BETWEEN for filtering

Use ORDER BY, LIMIT for sorting and pagination

Use IS NULL, IN, NOT IN, and Aliases to enhance readability

Use USING or NATURAL JOIN for simplified syntax

# 📘 DBMS and SQL Interview Questions

This guide contains important and frequently asked DBMS and SQL interview questions with brief explanations and examples.

---

## 🟦 1. What is the difference between DBMS and RDBMS?

| Feature | DBMS | RDBMS |
|--------|------|--------|
| Storage | Data is stored as files | Data is stored in tables |
| Normalization | Not supported | Supported |
| Relationships | No relationships | Supports relationships via foreign keys |
| Example | File System, XML | MySQL, PostgreSQL, Oracle |

---

## 🟩 2. What are the types of keys in SQL?

- **Primary Key**: Uniquely identifies each record.
- **Foreign Key**: Refers to the primary key of another table.
- **Candidate Key**: A field that can qualify as a primary key.
- **Super Key**: Set of attributes that can uniquely identify a record.
- **Composite Key**: A key made up of two or more attributes.

---

## 🟨 3. What is Normalization?

Normalization is the process of organizing data to minimize redundancy.

### Types:
- **1NF**: Atomic values
- **2NF**: No partial dependency
- **3NF**: No transitive dependency
- **BCNF**: Each determinant is a candidate key

---

## 🟥 4. What are Joins? Types?

Used to combine rows from two or more tables.

- **INNER JOIN**: Common rows
- **LEFT JOIN**: All rows from left + matched from right
- **RIGHT JOIN**: All rows from right + matched from left
- **FULL JOIN**: All rows from both
- **SELF JOIN**: Join table with itself
- **CROSS JOIN**: Cartesian product

---

## 🟦 5. Difference between WHERE and HAVING?

| Clause | Usage |
|--------|--------|
| WHERE | Filters rows before grouping |
| HAVING | Filters after grouping (with GROUP BY) |

---

## 🟩 6. What is ACID property?

Used to ensure reliability in transactions:

- **A** – Atomicity
- **C** – Consistency
- **I** – Isolation
- **D** – Durability

---

## 🟨 7. What is a View?

A **View** is a virtual table based on the result of an SQL query.

```sql
CREATE VIEW view_name AS
SELECT column1, column2 FROM table_name WHERE condition;
```

---

## 🟥 8. What is the difference between DELETE, TRUNCATE, and DROP?

| Command | Description | Rollback |
|---------|-------------|----------|
| DELETE | Deletes rows with condition | Yes |
| TRUNCATE | Removes all rows | No |
| DROP | Deletes table structure | No |

---

## 🟦 9. What are indexes?

Indexes are used to speed up data retrieval.

Types:
- **Single Column Index**
- **Composite Index**
- **Unique Index**
- **Full-Text Index**

---

## 🟩 10. What is a Stored Procedure?

A saved block of SQL code that you can reuse.

```sql
CREATE PROCEDURE procedure_name()
BEGIN
   SELECT * FROM table_name;
END;
```

---

## 🟨 11. Difference between UNION and UNION ALL?

- **UNION**: Removes duplicates
- **UNION ALL**: Includes duplicates

---

## 🟥 12. What are aggregate functions in SQL?

- `COUNT()`
- `SUM()`
- `AVG()`
- `MIN()`
- `MAX()`

Used with `GROUP BY`.

---

## 🟦 13. What is the difference between CHAR and VARCHAR?

| Type | Description |
|------|-------------|
| CHAR(n) | Fixed-length string |
| VARCHAR(n) | Variable-length string |

---

## 🟩 14. What is a Subquery?

A query inside another query.

```sql
SELECT name 
FROM employees 
WHERE dept_id = (SELECT id FROM departments WHERE name = 'IT');
```

---

## 🟨 15. What is the difference between Clustered and Non-Clustered Index?

| Index Type | Description |
|------------|-------------|
| Clustered | Alters the physical order of data |
| Non-Clustered | Uses a pointer to the physical data |

---
# 🧠 Advanced DBMS & SQL Interview Questions (With Examples)

---

## 🔄 16. What is a Trigger in SQL?

A **Trigger** is a stored procedure that automatically executes when an event (INSERT, UPDATE, DELETE) occurs in a table.

### Example:

```sql
CREATE TRIGGER before_insert_customer
BEFORE INSERT ON customers
FOR EACH ROW
SET NEW.created_at = NOW();
```

---

## 🧾 17. What is a View? (With Example)

A **View** is a virtual table based on the result of an SQL SELECT query.

### Example:

```sql
CREATE VIEW high_value_customers AS
SELECT customerName, creditLimit
FROM customers
WHERE creditLimit > 50000;
```

---

## 🧩 18. What is Normalization? With Examples

Normalization is used to reduce data redundancy and improve data integrity.

### 1NF (First Normal Form):

- Ensures atomic columns (no multiple values).

```text
Invalid: Skills = "Java, Python"
Valid: Separate rows for Java and Python
```

### 2NF (Second Normal Form):

- Removes partial dependency (only applicable for composite keys).

```text
Split employee_project into employee and project tables.
```

### 3NF (Third Normal Form):

- Removes transitive dependencies.

```text
Move department_name out of employees if stored redundantly.
```

---

## 🏷️ 19. What are Constraints in SQL?

Constraints enforce rules on columns.

- `NOT NULL`
- `UNIQUE`
- `CHECK`
- `DEFAULT`
- `PRIMARY KEY`
- `FOREIGN KEY`

### Example:

```sql
CREATE TABLE students (
  id INT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  age INT CHECK(age >= 18)
);
```

---

## 🔐 20. What is a Transaction and COMMIT/ROLLBACK?

A **Transaction** is a group of SQL operations that are executed as a single unit.

- `COMMIT`: Saves changes
- `ROLLBACK`: Reverts changes

### Example:

```sql
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

---

## ⚡ 21. What is an Index? Explain Composite Index

An **Index** is used to speed up queries.

- **Composite Index**: Index on multiple columns.

### Example:

```sql
CREATE INDEX idx_name_dept ON employees(lastName, departmentId);
```

---

## 📂 22. Difference Between Temporary Table and View?

| Feature | Temporary Table | View |
|--------|------------------|------|
| Storage | Stored in temp memory | Virtual table |
| Lifetime | Exists for session | Persistent |
| Updatable | Yes | Sometimes, depending on SQL |

---

## 🔄 23. What is a Cursor in SQL?

A **Cursor** is used to retrieve rows one at a time from the result set.

### Example:

```sql
DECLARE myCursor CURSOR FOR SELECT name FROM students;
OPEN myCursor;
FETCH NEXT FROM myCursor;
CLOSE myCursor;
```

---

## 📌 24. What is the difference between DDL, DML, and TCL?

- **DDL**: Data Definition Language (`CREATE`, `DROP`, `ALTER`)
- **DML**: Data Manipulation Language (`INSERT`, `UPDATE`, `DELETE`)
- **TCL**: Transaction Control Language (`COMMIT`, `ROLLBACK`, `SAVEPOINT`)

---

## 🔁 25. Explain WITH Clause / CTE (Common Table Expression)

CTE allows you to define a temporary result set that can be referred within a SELECT, INSERT, UPDATE, or DELETE.

### Example:

```sql
WITH high_salary AS (
  SELECT * FROM employees WHERE salary > 60000
)
SELECT firstName FROM high_salary WHERE departmentId = 5;
```

---
