![SQL Server Tinitiate Image](../sqlserver.png)

# SQL Server Tutorial
&copy; TINITIATE.COM

##### [Back To Contents](../README.md)

# DQL - String Functions

> **[sqlserver-dql-string-functions.sql](../code/sqlserver-dql-string-functions.sql) [CTRL + CLICK]**
* String functions in SQL Server are used to manipulate text data.
* They provide a variety of operations to perform tasks such as searching for substrings, modifying case, trimming whitespace, and extracting parts of strings.

## String functions in SQL Server:
### Length Function (LEN):
* Returns the number of characters in a string.
```sql
-- Retrieve the length of the 'ename' column in the employees table
SELECT LEN(ename) FROM employees.emp;
```

```output
Output:
Representative LEN results: smith=5, allen=5, ward=4, jones=5, martin=6.
```
### Substring Function (SUBSTRING):
* Extracts a substring from a string based on a specified start position and optional length.
```sql
-- Extract the first three characters from
-- the 'ename' column in the employees table
SELECT SUBSTRING(ename, 1, 3) FROM employees.emp;
```

```output
Output:
Representative SUBSTRING results: smi, all, war, jon, mar.
```
### Concatenation Operator (+):
* Concatenates two or more strings together.
```sql
-- Concatenate the 'ename' and 'job' columns in the employees table
SELECT ename + ' - ' + job FROM employees.emp;
```

```output
Output:
Representative results: smith - clerk, allen - salesman, ward - salesman, jones - manager, martin - salesman.
```
### Lower Function (LOWER):
* Converts a string to lowercase.
```sql
-- Convert the 'dname' column in the departments table to lowercase
SELECT LOWER(dname) FROM employees.dept;
```

```output
Output:
accounting, research, sales, operations, techsupport, production.
```
### Upper Function (UPPER):
* Converts a string to uppercase.
```sql
-- Convert the 'job' column in the employees table to uppercase
SELECT UPPER(job) FROM employees.emp;
```

```output
Output:
Representative results: CLERK, SALESMAN, SALESMAN, MANAGER, SALESMAN.
```
### Trim Function (TRIM):
* Removes leading and trailing whitespace from a string.
```sql
-- Remove leading and trailing whitespace from the
-- 'ename' column in the employees table
SELECT TRIM(ename) FROM employees.emp;

-- It remove the whitespaces from front and back of a string text
SELECT TRIM('    tinitiate.com     ') AS trimmed_string;
```

```output
Output:
Employee 8015 -> sIMON
trimmed_string -> tinitiate.com
```
### Ltrim Function (LTRIM):
* Removes leading whitespace from a string.
```sql
-- Remove leading whitespace from the 'ename' column in the employees table
SELECT LTRIM(ename) FROM employees.emp;

-- It remove the whitespaces from front of a string text
SELECT LTRIM('    tinitiate.com     ') AS trimmed_lstring;
```

```output
Output:
Employee 8015 -> sIMON  
trimmed_lstring -> tinitiate.com
```
### Rtrim Function (RTRIM):
* Removes trailing whitespace from a string.
```sql
-- Remove trailing whitespace from the 'ename' column in the employees table
SELECT RTRIM(ename) FROM employees.emp;

-- It remove the whitespaces from back of a string text
SELECT RTRIM('    tinitiate.com     ') AS trimmed_rstring;
```

```output
Output:
Employee 8015 ->   sIMON
trimmed_rstring ->     tinitiate.com
```
### Charindex Function (CHARINDEX):
* Returns the position of a substring within a string.
```sql
-- Find the position of 'e' in the 'ename' column of the employees table
SELECT CHARINDEX('e', ename) FROM employees.emp;
```

```output
Output:
Representative CHARINDEX('e') results: smith=0, allen=4, ward=0, jones=4, martin=0.
```
### Left Function (LEFT):
* Extracts a specified number of characters from the left side of a string.
```sql
-- Extract the first three characters from the
-- 'dname' column in the departments table
SELECT LEFT(dname, 3) FROM employees.dept;
```

```output
Output:
acc, res, sal, ope, tec, PRO.
```
### Right Function (RIGHT):
* Extracts a specified number of characters from the right side of a string.
```sql
-- Extract the last three characters from the
-- 'loc' column in the departments table
SELECT RIGHT(loc, 3) FROM employees.dept;
```

```output
Output:
ork, las, ago, ton, tle, ver.
```
### Reverse Function (REVERSE):
* Reverses the characters in a string.
```sql
-- Reverse the 'ename' column in the employees table
SELECT REVERSE(ename) FROM employees.emp;
```

```output
Output:
Representative results: htims, nella, draw, senoj, nitram.
```
### Replace Function (REPLACE):
* Replaces occurrences of a substring within a string with another substring.
```sql
-- Replace 'a' with 'X' in the 'dname' column of the departments table
SELECT REPLACE(dname, 'a', 'X') FROM employees.dept;
```

```output
Output:
Xccounting, reseXrch, sXles, operXtions, techsupport, PRODUCTION.
```
## Conditional and NULL Handling
### CASE Expression (CASE):
* Evaluates conditions and returns a value when the first condition is met.
```sql
-- Decode the value 'clerk' to 'Clerk', 'salesman' to 'Salesman' in the 'job' column
SELECT CASE job
           WHEN 'clerk' THEN 'Clerk'
           WHEN 'salesman' THEN 'Salesman'
           ELSE 'Unknown'
       END AS job_title
FROM employees.emp;
```

```output
Output:
Representative job_title values: Clerk, Salesman, Salesman, Unknown, Salesman.
```
### ISNULL Function (ISNULL):
* Replaces null values with a specified replacement value.
```sql
-- Replace nulls in the 'commission' column with 0
SELECT ename, ISNULL(commission , 0) AS comm
FROM employees.emp;
```

```output
Output:
Representative results:
   | ename  | comm    |
   | smith  |    0.00 |
   | allen  |    0.00 |
   | ward   |    0.00 |
   | jones  |    0.00 |
   | martin | 1400.00 |
```
### Coalesce Function (COALESCE):
* Replaces NULL with a specified value or returns the first non-NULL value in a list of arguments.
```sql
-- Return the first non-null value among 'comm', 'bonus', and 0
SELECT ename, COALESCE(commission , mgr, 0) AS compensation
FROM employees.emp;
```

```output
Output:
Representative first non-NULL values:
   | ename  | compensation |
   | smith  |      7902.00 |
   | allen  |      7698.00 |
   | ward   |      7698.00 |
   | jones  |      7839.00 |
   | martin |      1400.00 |
```

##### [Back To Contents](../README.md)
***
| &copy; TINITIATE.COM |
|----------------------|
