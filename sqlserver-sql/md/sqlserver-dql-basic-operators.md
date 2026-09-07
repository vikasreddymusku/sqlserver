![SQL Server Tinitiate Image](../sqlserver.png)

# SQL Server Tutorial
&copy; TINITIATE.COM

##### [Back To Contents](../README.md)

# DQL - Basic Operators

> **[sqlserver-dql-basic-operators.sql](../code/sqlserver-dql-basic-operators.sql) [CTRL + CLICK]**
* In SQL Server, operators are symbols or keywords used to perform operations on data, such as arithmetic operations, logical comparisons, and pattern matching.

## Commonly used operators in SQL Server:
### Equality Operator (=):
* The equality operator (=) is used in a WHERE clause to filter rows where the value in a column is equal to a specified value.
* It returns true if the values are equal, otherwise false.
```sql
-- Retrieve employees with a specific job title
SELECT *
FROM employees.emp
WHERE job = 'manager';
```

```output
Output:
5 rows: jones, blake, clark, tony, futureman.
```
### Inequality Operator (<>):
* The inequality operator (<>), also known as the not equal to operator, is used in a WHERE clause to filter rows where the value in a column is not equal to a specified value.
* It returns true if the values are not equal, otherwise false.
```sql
-- Retrieve employees with a job title other than 'manager'
SELECT *
FROM employees.emp
WHERE job <> 'manager';
```

```output
Output:
38 rows returned (NULL job values are not returned by <>).
```
### IN Operator:
* The IN operator is used to specify multiple values in a WHERE clause.
* It allows you to filter data based on whether a specified value matches any value in a list.
```sql
-- Retrieve employees from departments 10, 20, and 30
SELECT *
FROM employees.emp
WHERE deptno IN (10, 20, 30);
```

```output
Output:
37 rows returned from departments 10, 20, and 30.
```
### NOT IN Operator:
* The NOT IN operator is the negation of the IN operator.
* It is used to exclude rows where the value in a specified column matches any value in a list.
```sql
-- Retrieve employees not from departments 10, 20, and 30
SELECT *
FROM employees.emp
WHERE deptno NOT IN (10, 20, 30);
```

```output
Output:
5 rows:
   | ename   | deptno |
   | bob     |     40 |
   | richard |     40 |
   | eve     |     50 |
   | halen   |     50 |
   | lin     |     50 |
```
### LIKE Operator:
* The LIKE operator is used in a WHERE clause to search for a specified pattern in a column.
* It allows you to perform pattern matching using wildcard characters: '%' (matches zero or more characters) and '_' (matches any single character).
```sql
-- Retrieve employees whose names start with 's'
SELECT *
FROM employees.emp
WHERE ename LIKE 's%';

-- Retrieve employees whose names start with 'S'
-- and are followed by exactly four characters
SELECT *
FROM employees.emp
WHERE ename LIKE 's____';

-- Retrieve employees whose names have 'e' as the second character
SELECT *
FROM employees.emp
WHERE ename LIKE '_e%';

-- Retrieve employees whose names end with 'd'
-- and are preceded by exactly three characters
SELECT *
FROM employees.emp
WHERE ename LIKE '___d';

-- Retrieve employees whose names contain 'a' anywhere in the name
SELECT *
FROM employees.emp
WHERE ename LIKE '%a%';

-- Retrieve employees whose names have 'a' as the second character
-- and 'es' as the fourth, fifth character
SELECT *
FROM employees.emp
WHERE ename LIKE '_a_es';
```

```output
Output:
s%      -> smith, scott, steve, sam
s____   -> smith, scott, steve
_e%     -> newhire, denver, jerry
___d    -> ward, ford
%a%     -> 20 rows returned
_a_es   -> james
```
### NOT LIKE Operator:
* The NOT LIKE operator is the negation of the LIKE operator.
* It is used to exclude rows where a specified pattern does not match in a column.
```sql
-- Retrieve employees whose names do not start with 's'
SELECT *
FROM employees.emp
WHERE ename NOT LIKE 's%';
```

```output
Output:
40 rows returned.
```
### BETWEEN Operator:
* The BETWEEN operator is used to filter results within a specified range.
* It is inclusive, meaning it includes the boundary values specified in the range.
```sql
-- Retrieve employees hired between '1982-01-01' and '1983-01-01'
SELECT *
FROM employees.emp
WHERE hiredate BETWEEN '1982-01-01' AND '1983-01-01';
```

```output
Output:
4 rows: scott, miller, newhire, sam.
```
### Greater Than (>):
* The greater than operator (>), when used in a WHERE clause, filters rows where the value in a column is greater than a specified value.
```sql
-- Retrieve employees with salaries greater than 2500
SELECT *
FROM employees.emp
WHERE sal > 2500;
```

```output
Output:
11 rows: jones, blake, scott, king, ford, tony, steve, carol, Dr. GOOD, tintin, futureman.
```
### Greater Than or Equal To (>=):
* The greater than or equal to operator (>=) filters rows where the value in a column is either greater than or equal to a specified value.
```sql
-- Retrieve employees hired on or after '1982-01-01'
SELECT *
FROM employees.emp
WHERE hiredate >= '1982-01-01';
```

```output
Output:
31 rows returned.
```
### Less Than (<):
* The less than operator (<) filters rows where the value in a column is less than a specified value.
```sql
-- Retrieve employees with salaries less than 1500
SELECT *
FROM employees.emp
WHERE sal < 1500;
```

```output
Output:
23 rows returned.
```
### Less Than or Equal To (<=):
* The less than or equal to operator (<=) filters rows where the value in a column is either less than or equal to a specified value.
```sql
-- Retrieve employees hired on or before '1982-01-01'
SELECT *
FROM employees.emp
WHERE hiredate <= '1982-01-01';
```

```output
Output:
12 rows returned.
```
### EXISTS Operator:
* The EXISTS operator is used to test for the existence of rows returned by a subquery.
* If the subquery returns any rows, the EXISTS condition is true.
```sql
-- Retrieve employees who have worked on a project
SELECT *
FROM employees.emp e
WHERE EXISTS (
    SELECT 1
    FROM employees.emp_projects ep
    WHERE e.empno = ep.empno
);
```

```output
Output:
24 employees have at least one project assignment.
```
### NOT EXISTS Operator:
* The NOT EXISTS operator is the negation of the EXISTS operator.
* It is used to test for the absence of rows returned by a subquery.
* If the subquery returns no rows, the NOT EXISTS condition is true.
```sql
-- Retrieve employees who have not worked on any project
SELECT *
FROM employees.emp e
WHERE NOT EXISTS (
    SELECT 1
    FROM employees.emp_projects ep
    WHERE e.empno = ep.empno
);
```

```output
Output:
20 employees have no project assignment.
```

##### [Back To Contents](../README.md)
***
| &copy; TINITIATE.COM |
|----------------------|
