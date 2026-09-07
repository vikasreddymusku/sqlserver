![SQL Server Tinitiate Image](../sqlserver.png)
# SQL Server Tutorial
&copy; TINITIATE.COM

##### [Back To Contents](../README.md)

# DQL - Aggregate Functions

> **[sqlserver-dql-aggregate-functions.sql](../code/sqlserver-dql-aggregate-functions.sql) [CTRL + CLICK]**
* Aggregate functions in SQL Server are used to perform calculations on a set of values and return a single value as a result.
* They allow you to derive summary statistics or perform calculations across multiple rows in a table.
* These functions can be combined with other clauses like GROUP BY, HAVING, and DISTINCT to perform more sophisticated analysis and summarization of data in SQL Server.

## Aggregate functions in SQL Server:
### COUNT:
* Counts the number of rows in a result set.
```sql
-- Counts the number of rows in a emp table
SELECT COUNT(*) AS total_employees FROM employees.emp;

-- Count the number of employees in each department
SELECT deptno, COUNT(*) AS num_employees 
FROM employees.emp 
GROUP BY deptno;

-- Count the number of employees hired after 1985 in each department and
-- only display departments with more than 3 employees
SELECT deptno, COUNT(*) AS num_employees 
FROM employees.emp 
WHERE YEAR(hiredate) > 1985
GROUP BY deptno
HAVING COUNT(*) > 3;

-- Identify the number of employees hired each year
SELECT YEAR(hiredate) AS hire_year, COUNT(*) AS num_employees 
FROM employees.emp 
GROUP BY YEAR(hiredate);
```

```output
Output:
total_employees = 44
Department row counts: NULL=2, 10=9, 20=13, 30=15, 40=2, 50=3.
Employees hired after 1985 with >3 employees by department: 10=4, 20=5, 30=5.
The hire-year query returns counts grouped by year.
```
### SUM:
* Calculates the sum of values in a numeric column.
```sql
-- Calculate the sum of salary of all employees
SELECT SUM(sal) AS total_salary FROM employees.emp;

-- Calculate the total salary budget for each department
SELECT deptno, SUM(sal) AS total_salary 
FROM employees.emp 
GROUP BY deptno;

-- Calculate the total salary budget for each department,
-- ordered by the total budget in descending order
SELECT deptno, SUM(sal) AS total_salary 
FROM employees.emp 
GROUP BY deptno
ORDER BY total_salary DESC;

-- Calculate the total commission earned by each department
SELECT deptno, SUM(commission) AS total_commission 
FROM employees.emp 
GROUP BY deptno;

-- Calculate the total commission earned by each department and display
-- departments with commissions exceeding 500, ordered by total commission
SELECT deptno, SUM(commission) AS total_commission 
FROM employees.emp 
GROUP BY deptno
HAVING SUM(commission) > 500
ORDER BY total_commission DESC;
```

```output
Output:
total_salary = 72225.00
Department salary totals: NULL=1000, 10=20200, 20=25875, 30=21100, 40=2550, 50=1500.
Department commission totals: 10=750, 30=2850, 50=150; departments with no non-NULL commission return NULL.
Commission total >500: dept 30=2850, dept 10=750.
```
### AVG:
* Computes the average of values in a numeric column.
```sql
-- Calculates the average value of sal column
SELECT AVG(sal) AS avg_salary FROM employees.emp;

-- Find the average salary of employees in each job position
SELECT job, AVG(sal) AS avg_salary 
FROM employees.emp 
GROUP BY job;

-- Find the average salary of employees hired after 1985
SELECT AVG(sal) AS avg_salary 
FROM employees.emp 
WHERE YEAR(hiredate) > 1985;

-- Identify the departments where the average salary is higher than $1500
SELECT deptno 
FROM employees.emp 
GROUP BY deptno 
HAVING AVG(sal) > 1500;

-- Find the job position with the highest average salary and
-- display the result along with the average salary
SELECT TOP 1 job, AVG(sal) AS avg_salary 
FROM employees.emp 
GROUP BY job
ORDER BY avg_salary DESC;
```

```output
Output:
avg_salary = 1719.642857...
Average salary after 1985 = 1410.526315...
Departments with average salary >1500: 10, 20, 30.
Highest average-salary job: president = 5000.00.
```
### MAX:
* Retrieves the maximum value in a column.
```sql
-- Retrieves the maximum value in sal column
SELECT MAX(sal) AS max_salary FROM employees.emp;

-- Find the employee(s) with the highest salary
SELECT * 
FROM employees.emp 
WHERE sal = (SELECT MAX(sal) FROM employees.emp);

-- Find the employee(s) with the highest salary in each department
SELECT e.*
FROM employees.emp e
JOIN (
    SELECT deptno, MAX(sal) AS max_salary
    FROM employees.emp
    GROUP BY deptno
) AS max_salaries ON e.deptno = max_salaries.deptno
 AND e.sal = max_salaries.max_salary;
```

```output
Output:
max_salary = 5000.00
Highest salary employee: king (empno 7839) = 5000.00.
Department maximum salaries: 10=5000, 20=3200, 30=2850, 40=1300, 50=500.
```
### MIN:
* Retrieves the minimum value in a column.
```sql
-- Retrieves the minimum value in sal column
SELECT MIN(sal) AS min_salary FROM employees.emp;

-- Find the employee(s) with the lowest commission
SELECT *
FROM employees.emp
WHERE commission = (SELECT MIN(commission) FROM employees.emp
 WHERE commission IS NOT NULL);

-- Find the employee(s) with the lowest salary in each department
SELECT e.*
FROM employees.emp e
JOIN (
    SELECT deptno, MIN(sal) AS min_salary
    FROM employees.emp
    GROUP BY deptno
) AS min_salaries ON e.deptno = min_salaries.deptno
 AND e.sal = min_salaries.min_salary;

-- Find the department(s) with the lowest average salary
SELECT dname, avg_salary
FROM (
    SELECT d.dname, AVG(e.sal) AS avg_salary
    FROM employees.emp e
    JOIN employees.dept d ON e.deptno = d.deptno
    GROUP BY d.dname
) AS dept_avg_salary
WHERE avg_salary = (SELECT MIN(avg_salary) FROM (
                        SELECT AVG(sal) AS avg_salary FROM employees.emp
                         GROUP BY deptno
                    ) AS dept_avg);

-- Using both max and min
-- Determine the highest and lowest salary in each department
SELECT deptno, MAX(sal) AS max_salary, MIN(sal) AS min_salary 
FROM employees.emp 
GROUP BY deptno;

--Find the department(s) with the highest and lowest average salary,
-- along with their respective average salaries
SELECT dname, avg_salary
FROM (
    SELECT d.dname, AVG(e.sal) AS avg_salary
    FROM employees.emp e
    JOIN employees.dept d ON e.deptno = d.deptno
    GROUP BY d.dname
) AS dept_avg_salary
WHERE avg_salary = (SELECT MAX(avg_salary) FROM (
                        SELECT AVG(sal) AS avg_salary FROM employees.emp
                         GROUP BY deptno
                    ) AS dept_avg)
OR avg_salary = (SELECT MIN(avg_salary) FROM (
                        SELECT AVG(sal) AS avg_salary FROM employees.emp
                         GROUP BY deptno
                    ) AS dept_avg);

-- Find the employee(s) with the highest salary and
-- the employee(s) with the lowest salary in each department
SELECT e.*
FROM employees.emp e
JOIN (
    SELECT deptno, MAX(sal) AS max_salary, MIN(sal) AS min_salary
    FROM employees.emp
    GROUP BY deptno
) AS salary_extremes ON e.deptno = salary_extremes.deptno 
AND (e.sal = salary_extremes.max_salary OR e.sal = salary_extremes.min_salary);
```

```output
Output:
min_salary = 500.00
Lowest commission = 0.00 -> turner.
Department minimum salaries: 10=850, 20=600, 30=950, 40=1250, 50=500.
Lowest-average department: techsupport = 500.00.
Highest-average department: accounting = 2244.44.
```

##### [Back To Contents](../README.md)
***
| &copy; TINITIATE.COM |
|----------------------|
