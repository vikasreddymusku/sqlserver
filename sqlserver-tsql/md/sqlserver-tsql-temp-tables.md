![Tinitiate SQLSERVER Training](../../sqlserver-sql/sqlserver.png)

# SQL Server - TSQL Tutorial
&copy; TINITIATE.COM

##### [Back To Contents](../README.md)

# Temp Tables
Temporary tables are a form of temporary storage in SQL Server. A normal local temporary table exists for its session (or the creating stored procedure's scope) until it is explicitly dropped or its scope ends; global temporary tables have separate lifetime rules.

## Local Temp Tables (#)
Local temporary tables are physical tables created in the tempdb system database. Their names are prefixed with a single hash (#) and their scope is limited to the session in which they were created. They are fully logged to the tempdb transaction log, which can cause performance bottlenecks and contention in high-volume environments. However, a major advantage is that SQL Server's query optimizer treats them like a regular table and creates statistics on them. This allows the optimizer to build efficient query plans, making them the preferred choice for handling large datasets and complex queries with multiple joins.

```sql
BEGIN
  -- Create a local temp table
  CREATE TABLE #temp_employees (
      emp_id INT, 
      employee_name VARCHAR(102)
  );

  INSERT INTO #temp_employees (emp_id, employee_name) 
  SELECT empno, ename FROM employees.emp;

  -- The optimizer will use statistics on #temp_employees
  SELECT t.employee_name, d.dname
  FROM #temp_employees AS t
  JOIN employees.emp AS e ON t.emp_id = e.empno
  JOIN employees.dept AS d ON e.deptno = d.deptno;
END
```

## Global Temp Tables (##)
Global temporary tables are also physical tables in tempdb and are visible to other sessions. Their names are prefixed with a double hash (##). SQL Server drops a global temporary table after the session that created it ends and no active task is still referencing it. Because of their global scope, naming and concurrency conflicts require care.

```sql
-- Create a global temp table
CREATE TABLE ##global_report (
    report_id INT, 
    report_data VARCHAR(255)
);

-- This table can be accessed by any active session until all sessions have disconnected.
INSERT INTO ##global_report (report_id, report_data) VALUES (1, 'Initial data');
SELECT * FROM ##global_report;
```

## Table Variables (@)
Table variables are variables backed by tempdb storage rather than purely in-memory objects. Their scope is limited to the batch, stored procedure, or function where they are declared; a `BEGIN...END` block by itself does not create a new variable scope. They generally do not maintain column statistics. Older versions commonly estimated one row, while SQL Server 2019+ with compatibility level 150 can use table-variable deferred compilation to improve cardinality estimates.

```sql
BEGIN
  -- Declare a table variable
  DECLARE @employee_list TABLE (
      emp_id INT,
      employee_name VARCHAR(100)
  );

  INSERT INTO @employee_list (emp_id, employee_name) 
  SELECT empno, ename FROM employees.emp WHERE deptno = 10;
  
  -- Older SQL Server versions often estimate one row for a table variable.
  -- SQL Server 2019+ can use deferred compilation (compatibility level 150+) to improve this estimate.
  SELECT * FROM @employee_list;
END
```
<img width="773" height="104" alt="image" src="https://github.com/user-attachments/assets/486b42cc-04d5-4844-a6e9-1611fce7a00c" />


##### [Back To Contents](../README.md)
***
| &copy; TINITIATE.COM |
|----------------------|
