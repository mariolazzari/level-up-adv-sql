# Level up advanced SQL

## Joins

### Employees list

```sql
-- Create a list of employees and their immediate managers.
SELECT
    emp.firstName,
    emp.lastName,
    emp.title,
    mng.firstName AS ManagerFirstName,
    mng.lastName AS ManagerLastName
FROM
    employee emp
    INNER JOIN employee mng ON emp.managerId = mng.employeeId
```

### Zero sales

```sql
-- Find sales people who have zero sales
SELECT
    emp.firstName,
    emp.lastName,
    emp.title,
    emp.startDate,
    sls.salesId
FROM
    employee emp
    LEFT JOIN sales sls ON emp.employeeId = sls.employeeId
WHERE
    emp.title = 'Sales Person'
    AND sls.salesId IS NULL;
```

### Customer sales

```sql
-- Create a list of employees and their immediate managers.
SELECT
    emp.firstName,
    emp.lastName,
    emp.title,
    mng.firstName AS ManagerFirstName,
    mng.lastName AS ManagerLastName
FROM
    employee emp
    INNER JOIN employee mng ON emp.managerId = mng.employeeId
```

## Grouping

### Cars sold by employee

```sql
-- How many cars has been sold per employee
-- start with this query
SELECT
    emp.employeeId,
    emp.firstName,
    emp.lastName
FROM
    sales sls
    INNER JOIN employee emp ON sls.employeeId = emp.employeeId
    -- then add the group by & count
SELECT
    emp.employeeId,
    emp.firstName,
    emp.lastName,
    count(*) as NumOfCarsSold
FROM
    sales sls
    INNER JOIN employee emp ON sls.employeeId = emp.employeeId
GROUP BY
    emp.employeeId,
    emp.firstName,
    emp.lastName
ORDER BY
    NumOfCarsSold DESC;
```

### Find last and most expensive car

```sql
-- Find the least and most expensive car sold by each employee this year
SELECT
    emp.employeeId,
    emp.firstName,
    emp.lastName,
    MIN(salesAmount) AS MinSalesAmount,
    MAX(salesAmount) as MaxSalesAmount
FROM
    sales sls
    INNER JOIN employee emp ON sls.employeeId = emp.employeeId
WHERE
    sls.soldDate >= date ('now', 'start of year')
GROUP BY
    emp.employeeId,
    emp.firstName,
    emp.lastName
```

### More than 5

```sql
-- Display report for employees who have sold at least 5 cars
SELECT
    emp.employeeId,
    count(*) AS NumOfCarsSold,
    MIN(salesAmount) AS MinSalesAmount,
    MAX(salesAmount) AS MaxSalesAmount
FROM
    sales sls
    INNER JOIN employee emp ON sls.employeeId = emp.employeeId
WHERE
    sls.soldDate >= date ('now', 'start of year')
GROUP BY
    emp.employeeId
HAVING
    count(*) > 5
```

## Advanced select options

### CTE

```sql

```
