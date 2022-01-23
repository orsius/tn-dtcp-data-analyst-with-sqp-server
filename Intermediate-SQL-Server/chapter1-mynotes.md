# Chapter 1 -- Data analysis with aggregations

## Creating aggregations

1. Write a T-SQL query which will return the average, minimum, and maximum values of the DurationSeconds column.

```SQL
-- Calculate the average, minimum and maximum
SELECT avg(DurationSeconds) AS Average,
       min(DurationSeconds) AS Minimum,
       max(DurationSeconds) AS Maximum
FROM Incidents
```

## Creating grouped aggregations

1. Write a T-SQL query to calculate the average, minimum, and maximum values of the DurationSeconds column grouped by Shape. You need to select an additional column. What is it?

```SQL
-- Calculate the aggregations by Shape
SELECT Shape,
       AVG(DurationSeconds) AS Average,
       MIN(DurationSeconds) AS Minimum,
       MAX(DurationSeconds) AS Maximum
FROM Incidents
group by Shape ;
```

2. Update the query to return only the records where the minimum of DurationSeconds column is greater than 1.

> To filter even further, for example, to find the values for states where the maximum value is greater than 10, you can use the HAVING clause.

```SQL
-- Calculate the aggregations by Shape
SELECT Shape,
       AVG(DurationSeconds) AS Average,
       MIN(DurationSeconds) AS Minimum,
       MAX(DurationSeconds) AS Maximum
FROM Incidents
GROUP BY Shape
-- Return records where minimum of DurationSeconds is greater than 1
HAVING MIN(DurationSeconds) > 1
```

# Dealing with missing data

## Removing missing values

1. Write a T-SQL query which returns only the IncidentDateTime and IncidentState columns where IncidentState is not missing.

```SQL
-- Return the specified columns
select IncidentDateTime, IncidentState
FROM Incidents
-- Exclude all the missing values from IncidentState
WHERE IncidentState IS NOT NULL
```

## Imputing missing values (I)

> In the previous exercise, you looked at the non-missing values in the IncidentState column. But what if you want to replace the missing values with another value instead of omitting them? You can do this using the ISNULL() function. Here we replace all the missing values in the Shape column using the word 'Saucer':

```
SELECT  Shape, ISNULL(Shape, 'Saucer') AS Shape2
FROM Incidents
```

You can also use `ISNULL()` to replace values from a different column instead of a specified word.

1. Write a T-SQL query which only returns rows where IncidentState is missing.
   Replace all the missing values in the IncidentState column with the values in the City column and name this new column Location

```SQL
-- Check the IncidentState column for missing values and replace them with the City column
SELECT IncidentState, ISNULL(IncidentState, City) AS Location
FROM Incidents
-- Filter to only return missing values from IncidentState
WHERE IncidentState IS NULL
```

## Imputing missing values (II)

> What if you want to replace missing values in one column with another and want to check the replacement column to make sure it doesn't have any missing values? To do that you need to use the COALESCE statement.

```SQL
SELECT Shape, City, COALESCE(Shape, City, 'Unknown') as NewShape
FROM Incidents
+----------------+-----------+-------------+
| Shape          |  City     |  NewShape   |
+----------------+-----------+-------------+
| NULL           | Orb       | Orb         |
| Triangle       | Toledo    | Triangle    |
| NULL           | NULL      | Unknown     |
+----------------+-----------+-------------+
```

1. Replace missing values in Country with the first non-missing value from IncidentState or City, in that order. Name the new column Location.

```SQL
-- Replace missing values
SELECT Country, COALESCE(Country, IncidentState, City) AS Location
FROM Incidents
WHERE Country IS NULL
```

## Using CASE statements

1. Create a new column, SourceCountry, defined from these cases:
   When Country is 'us' then it takes the value 'USA'.
   Otherwise it takes the value 'International'.

```SQL
SELECT Country,
       CASE WHEN Country = 'us'  THEN 'USA'
       ELSE 'International'
       END AS SourceCountry
FROM Incidents
```

## Creating several groups with CASE

In this exercise, you will write a CASE statement to group the values in the DurationSeconds into 5 groups based on the following ranges:

| DurationSeconds      | SecondGroup |
| -------------------- | ----------- |
| <= 120               | 1           |
| > 120 and <= 600     | 2           |
| > 600 and <= 1200    | 3           |
| > 1201 and <= 5000   | 4           |
| For all other values | 5           |

1. Create a new column, SecondGroup, that uses the values in the DurationSeconds column based on the ranges mentioned above.

```SQL
-- Complete the syntax for cutting the duration into different cases
SELECT DurationSeconds,
-- Start with the 2 TSQL keywords, and after the condition a TSQL word and a value
       CASE WHEN (DurationSeconds <= 120) THEN 1
-- The pattern repeats with the same keyword and after the condition the same word and next value
	   WHEN (DurationSeconds > 120 AND DurationSeconds <= 600) THEN 2
-- Use the same syntax here
	   WHEN (DurationSeconds > 601 AND DurationSeconds <= 1200) THEN 3
-- Use the same syntax here
	   WHEN (DurationSeconds > 1201 AND DurationSeconds <= 5000) THEN 4
-- Specify a value
       ELSE 5
	   END AS SecondGroup
FROM Incidents
```
