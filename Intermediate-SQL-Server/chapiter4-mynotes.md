# INTERMEDIATE SQL SERVER by Ginger Grant

## Chapiter 4 - Window functions in T-SQL

### Window Functions in T-SQL

#### Window functions with aggregations (I)

> Write a T-SQL query that returns the sum of OrderPrice by creating partitions for each TerritoryName.

```SQL
SELECT OrderID, TerritoryName,
       -- Total price for each partition
       SUM(OrderPrice)
       -- Create the window and partitions
       OVER(PARTITION BY TerritoryName) AS TotalPrice
FROM Orders
```

#### Window functions with aggregations (II)

> Count the number of rows in each partition.
> Partition the table by TerritoryName.

```SQL
SELECT OrderID, TerritoryName,
       -- Number of rows per partition
       COUNT(*)
       -- Create the window and partitions
       OVER(PARTITION BY TerritoryName) AS TotalOrders
FROM Orders
```

### Common window functions

#### Do you know window functions?

> Which of the following statements is **incorrect** regarding window queries?

- [ ] he window functions LEAD(), LAG(), FIRST_VALUE(), and LAST_VALUE() require ORDER BY in the OVER() clause.
- [x] The standard aggregations like SUM(), AVG(), and COUNT() require ORDER BY in the OVER() clause.
- [ ] If the query contains OVER() and PARTITION BY the table is partitioned.
- [ ] The first row in a window where the LAG() function is used is NULL.

#### First value in a window

> Write a T-SQL query that returns the first OrderDate by creating partitions for each TerritoryName.

```SQL
SELECT TerritoryName, OrderDate,
       -- Select the first value in each partition
       FIRST_VALUE(OrderDate)
       -- Create the partitions and arrange the rows
       OVER(PARTITION BY TerritoryName ORDER BY OrderDate) AS FirstOrder
FROM Orders
```

#### Previous and next values

> Write a T-SQL query that for each territory:
>
> - Shifts the values in OrderDate one row down. Call this column PreviousOrder.
> - Shifts the values in OrderDate one row up. Call this column NextOrder. You will need to PARTITION BY the territory

```SQL
SELECT TerritoryName, OrderDate,
       -- Specify the previous OrderDate in the window
       LAG(OrderDate)
       -- Over the window, partition by territory & order by order date
       OVER(PARTITION BY TerritoryName ORDER BY OrderDate) AS PreviousOrder,
       -- Specify the next OrderDate in the window
       LEAD(OrderDate)
       -- Create the partitions and arrange the rows
       OVER(PARTITION BY TerritoryName ORDER BY OrderDate) AS NextOrder
FROM Orders
```

### Increasing window complexity

#### Creating running totals

> Create the window, partition by TerritoryName and order by OrderDate to calculate a running total of OrderPrice.

```SQL
SELECT TerritoryName, OrderDate,
       -- Create a running total
       SUM(OrderPrice)
       -- Create the partitions and arrange the rows
       OVER(PARTITION BY TerritoryName ORDER BY OrderDate) AS TerritoryTotal
FROM Orders
```

#### Assigning row numbers

> Write a T-SQL query that assigns row numbers to all records partitioned by TerritoryName and ordered by OrderDate.

```SQL
SELECT TerritoryName, OrderDate,
       -- Assign a row number
       ROW_NUMBER()
       -- Create the partitions and arrange the rows
       OVER(PARTITION BY TerritoryName ORDER BY OrderDate) AS OrderCount
FROM Orders
```

### Using windows for statistical functions

> CTE _(aka Common Table Expression)_
> )

#### Calculating standard deviation

> Create the window, partition by TerritoryName and order by OrderDate to calculate a running standard deviation of OrderPrice.

```SQL
SELECT OrderDate, TerritoryName,
       -- Calculate the standard deviation
	   STDEV(OrderPrice)
       OVER(PARTITION BY TerritoryName ORDER BY OrderDate) AS StdDevPrice
FROM Orders
```

#### Calculating mode (I)

> - Create a CTE ModePrice that returns two columns (OrderPrice and UnitPriceFrequency).
> - Write a query that returns all rows in this CTE.

```SQL
-- Create a CTE Called ModePrice which contains two columns
WITH ModePrice (OrderPrice, UnitPriceFrequency)
AS
(
	SELECT OrderPrice,
	ROW_NUMBER()
	OVER(PARTITION BY OrderPrice ORDER BY OrderPrice) AS UnitPriceFrequency
	FROM Orders
)
-- Select everything from the CTE
SELECT * FROM ModePrice

```

#### Calculating mode (II)

> Use the CTE ModePrice to return the value of OrderPrice with the highest row number.

```SQL
-- CTE from the previous exercise
WITH ModePrice (OrderPrice, UnitPriceFrequency)
AS
(
	SELECT OrderPrice,
	ROW_NUMBER()
    OVER (PARTITION BY OrderPrice ORDER BY OrderPrice) AS UnitPriceFrequency
	FROM Orders
)

-- Select the order price from the CTE
SELECT OrderPrice AS ModeOrderPrice
FROM ModePrice
-- Select the maximum UnitPriceFrequency from the CTE
WHERE UnitPriceFrequency IN (SELECT MAX(UnitPriceFrequency) FROM ModePrice)
```

####

```SQL

```

####

```SQL

```
