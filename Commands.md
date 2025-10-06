# PostgreSQL Reference Guide

-- Connect / Disconnect
psql -U username -d dbname    # Connect to a database
\q                             # Exit psql

-- List Databases / Tables
\l                  # List all databases
\dt                 # List all tables in current database
\d tablename        # Describe table structure

-- Database Management
CREATE DATABASE dbname;   # Create a new database
DROP DATABASE dbname;     # Drop a database

-- Table Management
CREATE TABLE tablename (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    age INT,
    email VARCHAR(100) UNIQUE
);                         # Create a table

DROP TABLE tablename;      # Drop a table
ALTER TABLE tablename ADD COLUMN phone VARCHAR(15);              # Add column
ALTER TABLE tablename ALTER COLUMN age SET DEFAULT 18;           # Modify column
ALTER TABLE tablename DROP COLUMN email;                         # Drop column

-- Data Manipulation (CRUD)
INSERT INTO tablename (name, age) VALUES ('Alice', 25);          # Insert data
UPDATE tablename SET age = 26 WHERE name = 'Alice';              # Update data
DELETE FROM tablename WHERE name = 'Alice';                      # Delete data
SELECT * FROM tablename;                                          # Select all data
SELECT name, age FROM tablename WHERE age > 20 ORDER BY age DESC; # Conditional select

-- Joins
SELECT a.id, a.name, b.salary
FROM employees a
INNER JOIN salaries b ON a.id = b.emp_id;                        # Inner Join

SELECT a.id, a.name, b.salary
FROM employees a
LEFT JOIN salaries b ON a.id = b.emp_id;                         # Left Join

SELECT a.id, a.name, b.salary
FROM employees a
RIGHT JOIN salaries b ON a.id = b.emp_id;                        # Right Join

SELECT a.id, a.name, b.salary
FROM employees a
FULL OUTER JOIN salaries b ON a.id = b.emp_id;                   # Full Outer Join

-- Subqueries
SELECT name FROM employees
WHERE id IN (SELECT emp_id FROM salaries WHERE salary > 5000);    # Simple subquery

SELECT name, age
FROM employees e
WHERE age > (SELECT AVG(age) FROM employees WHERE dept_id = e.dept_id);  # Correlated subquery

-- Temporary Tables
CREATE TEMP TABLE temp_employees (
    id SERIAL,
    name VARCHAR(50),
    age INT
);                                                                 # Create temp table

INSERT INTO temp_employees (name, age) VALUES ('Bob', 30);        # Insert into temp table
SELECT * FROM temp_employees;                                      # Select from temp table
DROP TABLE temp_employees;                                         # Drop temp table

-- Indexes & Constraints
CREATE INDEX idx_name ON tablename(name);                          # Create index
DROP INDEX idx_name;                                               # Drop index

PRIMARY KEY (id)                                                   # Primary key
FOREIGN KEY (dept_id) REFERENCES departments(id)                  # Foreign key
UNIQUE (email)                                                     # Unique constraint
CHECK (age >= 18)                                                  # Check constraint

-- Aggregates & Grouping
SELECT COUNT(*), SUM(salary), AVG(salary), MAX(salary), MIN(salary)
FROM employees;                                                    # Aggregate functions

SELECT dept_id, AVG(salary)
FROM employees
GROUP BY dept_id
HAVING AVG(salary) > 5000;                                         # Group by with HAVING

-- Views
CREATE VIEW high_salary AS
SELECT name, salary FROM employees WHERE salary > 5000;           # Create view

SELECT * FROM high_salary;                                         # Select from view
DROP VIEW high_salary;                                             # Drop view

-- Functions & Stored Procedures
CREATE OR REPLACE FUNCTION get_age(emp_id INT) RETURNS INT AS $$
DECLARE
    emp_age INT;
BEGIN
    SELECT age INTO emp_age FROM employees WHERE id = emp_id;
    RETURN emp_age;
END;
$$ LANGUAGE plpgsql;                                              # Create function

SELECT get_age(1);                                                # Call function

-- Transactions
BEGIN;                                                             # Start transaction
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;                                                            # Commit transaction
ROLLBACK;                                                          # Rollback transaction

-- PostgreSQL CLI Shortcuts
\du             # Show current users
\c              # Show current database
\s              # Show SQL history
\conninfo       # Show current connection info

-- Backup & Restore
pg_dump dbname > backup.sql      # Backup database
psql dbname < backup.sql         # Restore database

-- Using PostgreSQL with Docker
docker run -d --name pg_container -e POSTGRES_PASSWORD=mysecret -p 5432:5432 postgres
psql -h localhost -U postgres -d postgres
