![SQL Server Tinitiate Image](../sqlserver.png)

# SQL Server Tutorial
&copy; TINITIATE.COM

##### [Back To Contents](../README.md)

# DQL - Common Table Expressions (CTEs)

> **[sqlserver-dql-cte.sql](../code/sqlserver-dql-cte.sql) [CTRL + CLICK]**
* In SQL Server, a Common Table Expression (CTE) is a temporary result set that you can reference within a SELECT, INSERT, UPDATE, or DELETE statement.
* CTEs provide a way to write more readable and maintainable queries by breaking down complex queries into simpler parts.
* CTEs are defined using the `WITH` keyword.

## Creating a CTE
* To create a CTE, you use the `WITH` keyword.
```sql
-- Define a CTE to get employees with job title 'manager'
WITH Managers AS (
    SELECT empno, ename, job, sal
    FROM employees.emp
    WHERE job = 'manager'
)
-- Retrieve employees with job title 'manager' using the CTE
SELECT *
FROM Managers;

-- Define a CTE to count employees in each department
WITH DeptEmployeeCount AS (
    SELECT deptno, COUNT(*) AS num_employees
    FROM employees.emp
    GROUP BY deptno
)
SELECT d.deptno, d.dname, dec.num_employees
FROM employees.dept d
JOIN DeptEmployeeCount dec ON d.deptno = dec.deptno;

-- Define a CTE to calculate the average salary by department
WITH AvgSalary AS (
    SELECT deptno, AVG(sal) AS avg_sal
    FROM employees.emp
    GROUP BY deptno
)
SELECT d.deptno, d.dname, a.avg_sal
FROM employees.dept d
JOIN AvgSalary a ON d.deptno = a.deptno;

-- Define a CTE to list employees and their associated projects
WITH EmployeeProjects AS (
    SELECT e.empno, e.ename, ep.projectno, p.budget
    FROM employees.emp e
    JOIN employees.emp_projects ep ON e.empno = ep.empno
    JOIN employees.projects p ON ep.projectno = p.projectno
)
SELECT empno, ename, projectno, budget
FROM EmployeeProjects;

-- Define a CTE to rank employees by salary within each department
WITH RankedEmployees AS (
    SELECT empno, ename, deptno, sal,
           ROW_NUMBER() OVER (PARTITION BY deptno ORDER BY sal DESC) AS row_num,
           RANK() OVER (PARTITION BY deptno ORDER BY sal DESC) AS rank,
           DENSE_RANK() OVER (PARTITION BY deptno ORDER BY sal DESC) AS dense_rank
    FROM employees.emp
)
SELECT empno, ename, deptno, sal, row_num, rank, dense_rank
FROM RankedEmployees;

-- Define a CTE to calculate running total of salaries within each department
WITH RunningTotal AS (
    SELECT empno, ename, deptno, sal,
           SUM(sal) OVER (PARTITION BY deptno ORDER BY empno ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
    FROM employees.emp e 
)
SELECT empno, ename, deptno, sal, running_total
FROM RunningTotal;
```

```output
Output:
Managers CTE: 5 rows -> jones, blake, clark, tony, futureman.
Department counts:
   | deptno | dname       | num_employees |
   |     10 | accounting  |             9 |
   |     20 | research    |            13 |
   |     30 | sales       |            15 |
   |     40 | operations  |             2 |
   |     50 | techsupport |             3 |
EmployeeProjects CTE: 24 rows returned.
RankedEmployees and RunningTotal return one result row per employee.
```
## Using Multiple CTEs
* You can define multiple CTEs in a single query, separated by commas.
```sql
-- Define CTEs to calculate total salary and average salary by department
WITH TotalSalary AS (
    SELECT deptno, SUM(sal) AS total_sal
    FROM employees.emp
    GROUP BY deptno
),
AvgSalary AS (
    SELECT deptno, AVG(sal) AS avg_sal
    FROM employees.emp
    GROUP BY deptno
)
-- Retrieve department numbers, total salary, and average salary
SELECT t.deptno, t.total_sal, a.avg_sal
FROM TotalSalary t
JOIN AvgSalary a ON t.deptno = a.deptno;

WITH TotalSalary AS (
    SELECT deptno, SUM(sal) AS total_sal
    FROM employees.emp d 
    GROUP BY deptno
),
TotalBudget AS (
    SELECT e.deptno, SUM(p.budget) AS total_budget
    FROM employees.emp e
    JOIN employees.emp_projects ep ON e.empno = ep.empno
    JOIN employees.projects p ON ep.projectno = p.projectno
    GROUP BY e.deptno
)
-- Retrieve department numbers, total salary, and total project budget
SELECT ts.deptno, ts.total_sal, tb.total_budget
FROM TotalSalary ts
JOIN TotalBudget tb ON ts.deptno = tb.deptno;
```

```output
Output:
Total/average salary by department:
   | deptno | total_sal | avg_sal |
   | NULL   |   1000.00 | 1000.00 |
   | 10     |  20200.00 | 2244.44 |
   | 20     |  25875.00 | 1990.38 |
   | 30     |  21100.00 | 1507.14 |
   | 40     |   2550.00 | 1275.00 |
   | 50     |   1500.00 |  500.00 |
The second query returns total salary and assigned project budget by department.
```
## Recursive CTEs
* Recursive CTEs are used to perform operations like traversing hierarchical data or generating sequences.
* Recursive CTEs consist of two parts: an anchor member and a recursive member.
```sql
-- Define a recursive CTE to generate a sequence of numbers
WITH Sequence AS (
    -- Anchor member
    SELECT 1 AS num
    UNION ALL
    -- Recursive member
    SELECT num + 1
    FROM Sequence
    WHERE num < 10
)
SELECT num
FROM Sequence;

-- Define a recursive CTE to find the management hierarchy
WITH EmpHierarchy AS (
    -- Anchor member: select the top-level manager (president)
    SELECT empno, ename, job, mgr
    FROM employees.emp
    WHERE mgr IS NULL
    
    UNION ALL
    
    -- Recursive member: select employees managed by the current level
    SELECT e.empno, e.ename, e.job, e.mgr
    FROM employees.emp e
    INNER JOIN EmpHierarchy eh ON e.mgr = eh.empno
)
-- Retrieve the management hierarchy
SELECT *
FROM EmpHierarchy;
```

```output
Output:
Sequence CTE:
   1
   2
   3
   4
   5
   6
   7
   8
   9
   10

The hierarchy query returns the top-level employees and recursively follows manager relationships.
```
## Common Use Cases for CTEs
* Breaking down complex queries into simpler, more manageable parts.
* Improving readability and maintainability of SQL code.
* Recursively traversing hierarchical data.
* Generating sequences or performing iterative calculations.
## Benefits of Using CTEs
* Enhanced code readability and organization.
* Ability to reference the same CTE multiple times in a query.
* Simplified complex queries by breaking them into smaller, logical components.

##### [Back To Contents](../README.md)
***
| &copy; TINITIATE.COM |
|----------------------|
