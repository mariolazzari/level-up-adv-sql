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
-- Summarise sales per year by using a CTE
WITH
  cte AS (
    SELECT
      strftime ('%Y', soldDate) AS soldYear,
      salesAmount
    FROM
      sales
  )
SELECT
  soldYear,
  FORMAT ("$%.2f", sum(salesAmount)) AS AnnualSales
FROM
  cte
GROUP BY
  soldYear
ORDER BY
  soldYear
```

### Case statement

```sql
-- Display cars sold for each employee by month
-- 1. start with a query to get the needed data
SELECT
      emp.firstName,
      emp.lastName,
      sls.soldDate,
      sls.salesAmount
FROM
      sales sls
      INNER JOIN employee emp ON sls.employeeId = emp.employeeId
WHERE
      sls.soldDate >= '2021-01-01'
      AND sls.soldDate < '2022-01-01'
      -- 2. implement case statements for each month
SELECT
      emp.firstName,
      emp.lastName,
      CASE
            WHEN strftime ('%m', soldDate) = '01' THEN salesAmount
      END AS JanSales,
      CASE
            WHEN strftime ('%m', soldDate) = '02' THEN salesAmount
      END AS FebSales,
      CASE
            WHEN strftime ('%m', soldDate) = '03' THEN salesAmount
      END AS MarSales,
      CASE
            WHEN strftime ('%m', soldDate) = '04' THEN salesAmount
      END AS AprSales,
      CASE
            WHEN strftime ('%m', soldDate) = '05' THEN salesAmount
      END AS MaySales,
      CASE
            WHEN strftime ('%m', soldDate) = '06' THEN salesAmount
      END AS JunSales,
      CASE
            WHEN strftime ('%m', soldDate) = '07' THEN salesAmount
      END AS JulSales,
      CASE
            WHEN strftime ('%m', soldDate) = '08' THEN salesAmount
      END AS AugSales,
      CASE
            WHEN strftime ('%m', soldDate) = '09' THEN salesAmount
      END AS SepSales,
      CASE
            WHEN strftime ('%m', soldDate) = '10' THEN salesAmount
      END AS OctSales,
      CASE
            WHEN strftime ('%m', soldDate) = '11' THEN salesAmount
      END AS NovSales,
      CASE
            WHEN strftime ('%m', soldDate) = '12' THEN salesAmount
      END AS DecSales
FROM
      sales sls
      INNER JOIN employee emp ON sls.employeeId = emp.employeeId
WHERE
      sls.soldDate >= '2021-01-01'
      AND sls.soldDate < '2022-01-01'
ORDER BY
      emp.lastName,
      emp.firstName
      -- 3. finally group the data
SELECT
      emp.firstName,
      emp.lastName,
      SUM(
            CASE
                  WHEN strftime ('%m', soldDate) = '01' THEN salesAmount
            END
      ) AS JanSales,
      SUM(
            CASE
                  WHEN strftime ('%m', soldDate) = '02' THEN salesAmount
            END
      ) AS FebSales,
      SUM(
            CASE
                  WHEN strftime ('%m', soldDate) = '03' THEN salesAmount
            END
      ) AS MarSales,
      SUM(
            CASE
                  WHEN strftime ('%m', soldDate) = '04' THEN salesAmount
            END
      ) AS AprSales,
      SUM(
            CASE
                  WHEN strftime ('%m', soldDate) = '05' THEN salesAmount
            END
      ) AS MaySales,
      SUM(
            CASE
                  WHEN strftime ('%m', soldDate) = '06' THEN salesAmount
            END
      ) AS JunSales,
      SUM(
            CASE
                  WHEN strftime ('%m', soldDate) = '07' THEN salesAmount
            END
      ) AS JulSales,
      SUM(
            CASE
                  WHEN strftime ('%m', soldDate) = '08' THEN salesAmount
            END
      ) AS AugSales,
      SUM(
            CASE
                  WHEN strftime ('%m', soldDate) = '09' THEN salesAmount
            END
      ) AS SepSales,
      SUM(
            CASE
                  WHEN strftime ('%m', soldDate) = '10' THEN salesAmount
            END
      ) AS OctSales,
      SUM(
            CASE
                  WHEN strftime ('%m', soldDate) = '11' THEN salesAmount
            END
      ) AS NovSales,
      SUM(
            CASE
                  WHEN strftime ('%m', soldDate) = '12' THEN salesAmount
            END
      ) AS DecSales
FROM
      sales sls
      INNER JOIN employee emp ON sls.employeeId = emp.employeeId
WHERE
      sls.soldDate >= '2021-01-01'
      AND sls.soldDate < '2022-01-01'
GROUP BY
      emp.firstName,
      emp.lastName
ORDER BY
      emp.lastName,
      emp.firstName
```

### Subqueries

```sql
-- Find sales of cars which are electric by using a subquery
-- 1. join sales and inventory
SELECT
  *
FROM
  sales sls
  INNER JOIN inventory inv ON sls.inventoryId = inv.inventoryId
  -- 2. review the model table
Select
  *
from
  model
limit
  10;

-- 3. lookup the modelId for the electric models
SELECT
  modelId
FROM
  model
WHERE
  EngineType = 'Electric';

-- Final query
SELECT
  sls.soldDate,
  sls.salesAmount,
  inv.colour,
  inv.year
FROM
  sales sls
  INNER JOIN inventory inv ON sls.inventoryId = inv.inventoryId
WHERE
  inv.modelId IN (
    SELECT
      modelId
    FROM
      model
    WHERE
      EngineType = 'Electric'
  )
```

## Windowing functions

Use to partition query results for analysis

### Ranking

```sql
-- For each sales person rank the car models they've sold most
-- First join the tables to get the necessary data
SELECT
  emp.firstName,
  emp.lastName,
  mdl.model,
  sls.salesId
FROM
  sales sls
  INNER JOIN employee emp ON sls.employeeId = emp.employeeId
  INNER JOIN inventory inv ON inv.inventoryId = sls.inventoryId
  INNER JOIN model mdl ON mdl.modelId = inv.modelId
  -- apply the grouping
SELECT
  emp.firstName,
  emp.lastName,
  mdl.model,
  count(model) AS NumberSold
FROM
  sales sls
  INNER JOIN employee emp ON sls.employeeId = emp.employeeId
  INNER JOIN inventory inv ON inv.inventoryId = sls.inventoryId
  INNER JOIN model mdl ON mdl.modelId = inv.modelId
GROUP BY
  emp.firstName,
  emp.lastName,
  mdl.model
  -- add in the windowing function
SELECT
  emp.firstName,
  emp.lastName,
  mdl.model,
  count(model) AS NumberSold,
  rank() OVER (
    PARTITION BY
      sls.employeeId
    ORDER BY
      count(model) desc
  ) AS Rank
FROM
  sales sls
  INNER JOIN employee emp ON sls.employeeId = emp.employeeId
  INNER JOIN inventory inv ON inv.inventoryId = sls.inventoryId
  INNER JOIN model mdl ON mdl.modelId = inv.modelId
GROUP BY
  emp.firstName,
  emp.lastName,
  mdl.model
```

### Sales per month with annual total

```sql
-- Create a report showing sales per month and an annual total
-- get the needed data
SELECT
  strftime ('%Y', soldDate) AS soldYear,
  strftime ('%m', soldDate) AS soldMonth,
  salesAmount
FROM
  sales
  -- apply the grouping
SELECT
  strftime ('%Y', soldDate) AS soldYear,
  strftime ('%m', soldDate) AS soldMonth,
  SUM(salesAmount) AS salesAmount
FROM
  sales
GROUP BY
  soldYear,
  soldMonth
ORDER BY
  soldYear,
  soldMonth
  -- add the window function - simplify with cte
with
  cte_sales as (
    SELECT
      strftime ('%Y', soldDate) AS soldYear,
      strftime ('%m', soldDate) AS soldMonth,
      SUM(salesAmount) AS salesAmount
    FROM
      sales
    GROUP BY
      soldYear,
      soldMonth
  )
SELECT
  soldYear,
  soldMonth,
  salesAmount,
  SUM(salesAmount) OVER (
    PARTITION BY
      soldYear
    ORDER BY
      soldYear,
      soldMonth
  ) AS AnnualSales_RunningTotal
FROM
  cte_sales
ORDER BY
  soldYear,
  soldMonth
```

### Cars sold per month

```sql
-- Displays the number of cars sold this month, and last month
-- Get the data
SELECT
  strftime ('%Y-%m', soldDate) AS MonthSold,
  COUNT(*) AS NumberCarsSold
FROM
  sales
GROUP BY
  strftime ('%Y-%m', soldDate)
  -- Apply the window function
SELECT
  strftime ('%Y-%m', soldDate) AS MonthSold,
  COUNT(*) AS NumberCarsSold,
  LAG (COUNT(*), 1, 0) OVER calMonth AS LastMonthCarsSold
FROM
  sales
GROUP BY
  strftime ('%Y-%m', soldDate)
WINDOW
  calMonth AS (
    ORDER BY
      strftime ('%Y-%m', soldDate)
  )
ORDER BY
  strftime ('%Y-%m', soldDate)
```
