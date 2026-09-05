![Tinitiate SQLSERVER Training](../../sqlserver-sql/sqlserver.png)

# SQL Server - TSQL Tutorial
&copy; TINITIATE.COM

##### [Back To Contents](../README.md)

# Loops
* Loops are used to execute a block of code repeatedly based on a condition. 

## WHILE Loop
* The `WHILE` loop executes a block of code repeatedly as long as a specified condition is true.

    While your example is a good introduction, the primary lesson for advanced T-SQL training is that you should almost always avoid WHILE loops for data manipulation. T-SQL is a set-based language, meaning it's optimized     to process entire sets of data at once. Row-by-row processing (often called RBAR, or "Row-By-Agonizing-Row") is incredibly inefficient and can lead to major performance issues on large tables.

The following examples illustrate the correct, set-based approach and provide a better use case for loops in T-SQL.

###  Set-Based Alternative (The Correct Way) 🏆
 Your original example uses a WHILE loop to update salaries one employee at a time. The correct, set-based way to do this is with a single UPDATE statement. This one command performs the same operation on all rows          simultaneously, which is exponentially faster and less resource-intensive.

#### Inefficient RBAR( "Row-By-Agonizing-Row") Method (Your original example):

```sql

DECLARE @EmpID INT;

SELECT @EmpID = MIN(empno) FROM employees.emp;

WHILE @EmpID IS NOT NULL
BEGIN
    UPDATE employees.emp SET sal = sal * 1.10 WHERE empno = @EmpID;
    SELECT @EmpID = MIN(empno) FROM employees.emp WHERE empno > @EmpID;
END

```
#### Efficient Set-Based Method:

```sql

UPDATE employees.emp
SET sal = sal * 1.10;

```
This single statement is the preferred method for updating all employee salaries.

### Appropriate Use of WHILE Loops 💡
A WHILE loop is best suited for tasks that are inherently iterative and cannot be easily performed with a single set-based statement. This includes administrative tasks, dynamic SQL generation, and handling certain types of hierarchical data.

* Example: Finding an Employee's Managerial Hierarchy

This example uses a WHILE loop to find an employee's manager, then that manager's manager, and so on, all the way up the chain to the president. This task is difficult to do in a single, simple set-based query without using more advanced features like Recursive CTEs (Common Table Expressions), making a loop a viable solution.

```sql
DECLARE @CurrentEmpID INT = 7566; -- Start with employee JONES
DECLARE @CurrentEmpName VARCHAR(10);
DECLARE @MgrID INT;
DECLARE @Level INT = 1;

PRINT 'Hierarchy for Employee: JONES';
PRINT '----------------------------';

WHILE @CurrentEmpID IS NOT NULL
BEGIN
    -- Get the current employee's name and their manager's ID
    SELECT @CurrentEmpName = ename, @MgrID = mgr
    FROM employees.emp
    WHERE empno = @CurrentEmpID;
    
    -- Print the current level and employee name
    PRINT 'Level ' + CAST(@Level AS VARCHAR) + ': ' + @CurrentEmpName;

    -- Move up the hierarchy
    SET @CurrentEmpID = @MgrID;
    SET @Level = @Level + 1;
END

```
* In this example, we use a `WHILE` loop to increase the salaries of all employees in the emp table by 10%. The loop continues until there are no more employees to update.

## FOR Loop
* T-SQL doesn't have a native FOR loop construct, but you can effectively replicate its behavior using a WHILE loop combined with a counter variable. This is a common pattern in T-SQL programming.

* Simulating a FOR Loop
    The FOR loop in other languages typically has three parts:

* Initialization: Setting a counter variable to a starting value.

* Condition: The expression that is checked before each iteration to determine if the loop should continue.

* Iteration: The action that modifies the counter variable (e.g., incrementing or decrementing it) after each iteration.

You can combine these three elements within a T-SQL WHILE loop to create the same functionality. The initialization happens before the loop, the condition is part of the WHILE statement, and the iteration happens inside the loop's BEGIN...END block.

``` sql
-- Print Names of First 3 Projects
DECLARE @Counter INT = 1;

WHILE @Counter <= 3
BEGIN
    SELECT project_name FROM employees.projects WHERE projectno = @Counter;
    SET @Counter = @Counter + 1;
END
```

```sql
DECLARE @EmpID INT = 7521; -- Start with WARD
DECLARE @MgrID INT;
DECLARE @MgrName VARCHAR(10);

-- Find the initial manager for the given employee
SELECT @MgrID = mgr
FROM employees.emp
WHERE empno = @EmpID;

-- Loop until the manager ID is NULL (the top of the hierarchy)
WHILE @MgrID IS NOT NULL
BEGIN
    -- Get the name and next manager in the chain
    SELECT @MgrName = ename, @MgrID = mgr
    FROM employees.emp
    WHERE empno = @MgrID;

    PRINT 'Manager Name: ' + @MgrName;
END;
GO
```
* In this example, we use a `WHILE` loop to print the names of the first 3 projects from the projects table. The loop runs as long as the @Counter variable is less than or equal to 3.

## DO WHILE Loop
* T-SQL does not have a native `DO WHILE` statement. To simulate post-test behavior, execute the work first and check the exit condition at the end of an unconditional `WHILE` loop. This guarantees that the body runs at least once.

```sql
-- Simulate DO WHILE: add projects until at least 10 projects exist
DECLARE @Count INT;

WHILE 1 = 1
BEGIN
    SELECT @Count = COUNT(*) FROM employees.projects;

    IF @Count < 10
    BEGIN
        INSERT INTO employees.projects (projectno, project_name, budget)
        VALUES (@Count + 1, 'New Project', 50000.00);
    END;

    SELECT @Count = COUNT(*) FROM employees.projects;

    -- Post-test condition: checked after the body has executed
    IF @Count >= 10
        BREAK;
END;
```

##### [Back To Contents](../README.md)
***
| &copy; TINITIATE.COM |
|----------------------|
