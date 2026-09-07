![SQL Server Tinitiate Image](../sqlserver.png)

# SQL Server Tutorial
&copy; TINITIATE.COM

##### [Back To Contents](../README.md)

# DQL - Data Query Language

> **[sqlserver-dql.sql](../code/sqlserver-dql.sql) [CTRL + CLICK]**
* In SQL Server, DQL (Data Query Language) refers to the subset of SQL (Structured Query Language) specifically designed for querying and retrieving data from a SQL Server database.
* DQL primarily involves the use of SELECT statements to extract information from one or more tables within the database.

## SELECT Statement
* The fundamental component of DQL is the SELECT statement, which is used to retrieve data from one or more tables in the database.
* It allows users to specify the columns they want to retrieve from one or more tables in the database.
* SELECT statements can also include various clauses for filtering, sorting, and grouping the data.
```sql
-- Retrieve all columns from the emp table
SELECT * FROM employees.emp;

-- Retrieve empno, ename and job columns from the emp table
SELECT empno, ename, job FROM employees.emp;

-- DISTINCT can be used to retrieve unique titles from the column
SELECT DISTINCT job FROM employees.emp;

-- Using alias to display column name as per requirement
SELECT DISTINCT job AS employeejob FROM employees.emp;
```

```output
Output:
SELECT *: 44 rows returned.
First 5 employees by stored row order:
   | empno | ename  | job      |
   |  7369 | smith  | clerk    |
   |  7499 | allen  | salesman |
   |  7521 | ward   | salesman |
   |  7566 | jones  | manager  |
   |  7654 | martin | salesman |

DISTINCT job / employeejob values:
   analyst, assistant, clerk, intern, manager, operator, president, salesman, support, NULL
```

## Commonly used clauses in SQL Server:
* In SQL Server DQL, clauses are components of SQL statements that provide additional instructions or conditions to control the behavior of the query.
* Clauses can be used in various SQL statements such as SELECT, INSERT, UPDATE, DELETE, and more.
* These clauses allow users to filter, manipulate, and organize data according to specific requirements.
### WHERE Clause:
* The WHERE clause is used to filter rows based on specified conditions.
```sql
-- Retrieve employees with a specific job title
SELECT * FROM employees.emp WHERE job = 'manager';

-- Retrieve employees hired after a specific date
SELECT * FROM employees.emp WHERE hiredate > '1982-01-01';

-- Retrieve employees with a salary higher than a certain amount
SELECT * FROM employees.emp WHERE sal > 2000;
```

```output
Output:
job = 'manager': 5 rows -> jones, blake, clark, tony, futureman
hiredate > '1982-01-01': 31 rows returned.
sal > 2000: 12 rows returned.
```
### GROUP BY Clause:
* The GROUP BY clause is used to group rows that have the same values into summary rows.
```sql
-- Calculate the total salary expense for each dept
SELECT deptno, SUM(sal) AS total_salary
FROM employees.emp
GROUP BY deptno;

-- Count the number of employees in each job title
SELECT job, COUNT(*) AS num_employees
FROM employees.emp
GROUP BY job;

-- Calculate the average commission for each dept
SELECT deptno, AVG(commission) AS avg_commission
FROM employees.emp
GROUP BY deptno;
```

```output
Output:
Salary total by department:
   | deptno | total_salary |
   | NULL   |       1000.00 |
   | 10     |      20200.00 |
   | 20     |      25875.00 |
   | 30     |      21100.00 |
   | 40     |       2550.00 |
   | 50     |       1500.00 |

Employee counts by job and average commission by department are returned as grouped result sets.
```
### HAVING Clause:
* The HAVING clause is used to filter groups of rows returned by a GROUP BY clause.
```sql
-- Retrieve departments with more than two employees
SELECT deptno, COUNT(*) AS num_employees
FROM employees.emp
GROUP BY deptno
HAVING COUNT(*) > 2;

-- Retrieve job titles with an average salary greater than 2500
SELECT job, AVG(sal) AS avg_salary
FROM employees.emp
GROUP BY job
HAVING AVG(sal) > 2500;

-- Retrieve departments where the total salary expense exceeds 10000
SELECT deptno, SUM(sal) AS total_salary
FROM employees.emp
GROUP BY deptno
HAVING SUM(sal) > 10000;
```

```output
Output:
Departments with more than two employees:
   | deptno | num_employees |
   | 10     |             9 |
   | 20     |            13 |
   | 30     |            15 |
   | 50     |             3 |

Jobs with average salary > 2500:
   | job       | avg_salary |
   | analyst   |    2950.00 |
   | manager   |    3135.00 |
   | president |    5000.00 |

Departments with salary total > 10000: 10, 20, 30.
```
### ORDER BY Clause:
* The ORDER BY clause is used to sort the result set based on specified columns.
```sql
-- Retrieve employees sorted by salary in descending order
SELECT *
FROM employees.emp
ORDER BY sal DESC;

-- Retrieve employees sorted by hire date in ascending order
SELECT *
FROM employees.emp
ORDER BY hiredate ASC;

-- Retrieve employees sorted by department number in ascending
-- and salary in descending order
SELECT *
FROM employees.emp
ORDER BY deptno ASC, sal DESC;
```

```output
Output:
ORDER BY changes only the display order.
Highest salaries begin with: king (5000), futureman (4500), Dr. GOOD (3200), steve (3100), scott/ford (3000).
```
### TOP Clause:
* The TOP clause is used to limit the number of rows returned by a query.
```sql
-- Retrieve the first 5 employees by employee number
SELECT TOP 5 *
FROM employees.emp
ORDER BY empno;

-- Retrieve the employees with the top 10 highest salaries
SELECT TOP 10 *
FROM employees.emp
ORDER BY sal DESC;
```

```output
Output:
TOP 5 by empno:
   | empno | ename  |
   |  7369 | smith  |
   |  7499 | allen  |
   |  7521 | ward   |
   |  7566 | jones  |
   |  7654 | martin |

TOP 10 highest salaries begin with king, futureman, Dr. GOOD, steve, scott/ford, jones, tony and blake.
The final row can be carol or tintin because both have salary 2700 and the query has no secondary tie-breaker.
```

##### [Back To Contents](../README.md)
***
| &copy; TINITIATE.COM |
|----------------------|
