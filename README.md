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

```
