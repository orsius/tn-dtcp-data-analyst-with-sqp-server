# INTERMEDIATE SQL SERVER by Ginger Grant

## Chapiter 2 -- Math Functions

### Counts and Totals

#### Calculating the total

> Write a T-SQL query which will return the sum of the Quantity column as Total for each type of MixDesc.

```SQL
-- Write a query that returns an aggregation
SELECT MixDesc, SUM(Quantity) as Total
FROM Shipments
-- Group by the relevant column
GROUP BY MixDesc
```

#### Counting the number of rows

> Create a query that returns the number of rows for each type of MixDesc.

```SQL
-- Count the number of rows by MixDesc
SELECT MixDesc, COUNT(*)
FROM Shipments
GROUP BY MixDesc
```

### Math with Dates

> Which date function should you use?
> Suppose you want to calculate the number of years between two different dates, DateOne and DateTwo. Which SQL statement would you use to perform that calculation?

- [x] `SELECT DATEDIFF(YYYY, DateOne, DateTwo)`

#### Counting the number of days between dates

> Write a query that returns the number of days between OrderDate and ShipDate.

```SQL
-- Return the difference in OrderDate and ShipDate
SELECT OrderDate, ShipDate,
       datediff(DD, OrderDate, ShipDate) AS Duration
FROM Shipments
```

#### Adding days to a date

> Write a query that returns the approximate delivery date as five days after the ShipDate.

```SQL
-- Return the DeliveryDate as 5 days after the ShipDate
SELECT OrderDate,
       dateadd(DD, 5, ShipDate) AS DeliveryDate
FROM Shipments
```

### Rounding and truncating

#### Rounding numbers

> Write a SQL query to round the values in the Cost column to the nearest whole number.

```SQL
-- Round Cost to the nearest dollar
SELECT Cost,
       Round(Cost, 0) AS RoundedCost
FROM Shipments
```

#### Truncating numbers

> Write a SQL query to truncate the values in the Cost column to the nearest whole number.

```SQL
-- Truncate cost to whole number
SELECT Cost,
       ROUND(Cost, 0, 1) AS TruncateCost
FROM Shipments
```

### More math functions

#### Calculating the absolute value

> Write a query that converts all the negative values in the DeliveryWeight column to positive values.

```SQL
-- Return the absolute value of DeliveryWeight
SELECT DeliveryWeight,
       ABS(DeliveryWeight) AS AbsoluteValue
FROM Shipments
```

#### Calculating squares and square roots

> Write a query that calculates the square and square root of the WeightValue column.

```SQL
-- Return the square and square root of WeightValue
SELECT WeightValue,
       SQUARE(WeightValue) AS WeightSquare,
       SQRT(WeightValue) AS WeightSqrt
FROM Shipments
```
