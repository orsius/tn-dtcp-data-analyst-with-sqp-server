# INTERMEDIATE SQL SERVER by Ginger Grant

## Chapiter 3 - WHILE loops

### Using variables in T-SQL

#### Creating and using variables

> Create an integer variable named counter.
> Assign the value 20 to this variable.
> Increment the variable counter by 1 and assign it back to counter.

```SQL
-- Declare the variable (a SQL Command, the var name, the datatype)
DECLARE @counter INT
-- Set the counter to 20
Set @counter = 20
-- Select and increment the counter by one
SET @counter = @counter + 1;
-- Select the counter
SELECT @counter
```

> :information_source: https://docs.microsoft.com/en-us/sql/t-sql/language-elements/variables-transact-sql?view=sql-server-ver15

#### Creating a WHILE loop

> Write a WHILE loop that increments counter by 1 until counter is less than 30

```SQL
DECLARE @counter INT
SET @counter = 20
-- Create a loop
while @counter < 30
-- Loop code starting point
BEGIN
	SELECT @counter = @counter + 1
-- Loop finish
END
-- Check the value of the variable
SELECT @counter
```

### Derived tables

#### Queries with derived tables (I)

> Return MaxGlucose from the derived table.
> Join the derived table to the main query on Age.

```SQL
SELECT a.RecordId, a.Age, a.BloodGlucoseRandom,
-- Select maximum glucose value (use colname from derived table)
       b.MaxGlucose
FROM Kidney a
-- Join to derived table
JOIN (SELECT Age, MAX(BloodGlucoseRandom) AS MaxGlucose FROM Kidney GROUP BY Age) b
-- Join on Age
ON a.Age = b.Age;
```

#### Queries with derived tables (II)

- Create a derived table
  - returning Age and MaxBloodPressure; the latter is the maximum of BloodPressure.
  - is taken from the kidney table.
  - is grouped by Age.
- Join the derived table to the main query on
  - blood pressure equal to max blood pressure.
  - age.

```SQL
SELECT *
FROM Kidney a
-- Create derived table: select age, max blood pressure from kidney grouped by age
JOIN (SELECT Age, MAX(BloodPressure) AS MaxBloodPressure FROM Kidney GROUP BY Age) b
-- JOIN on BloodPressure equal to MaxBloodPressure
ON a.BloodPressure = b.MaxBloodPressure
-- Join on Age
AND a.Age = b.Age

```

### Common Table Expressions _(aka CTE)_

#### Creating CTEs (I)

> Create a CTE BloodGlucoseRandom that returns one column (MaxGlucose) which contains the maximum BloodGlucoseRandom in the table.
> Join the CTE to the main table (Kidney) on BloodGlucoseRandom and MaxGlucose.

```SQL
-- Specify the keyowrds to create the CTE
WITH BloodGlucoseRandom (MaxGlucose)
AS (SELECT MAX(BloodGlucoseRandom) AS MaxGlucose FROM Kidney)

SELECT a.Age, b.MaxGlucose
FROM Kidney a
-- Join the CTE on blood glucose equal to max blood glucose
JOIN BloodGlucoseRandom b
ON a.BloodGlucoseRandom = b.MaxGlucose
```

#### Creating CTEs (II)

> Create a CTE BloodPressure that returns one column (MaxBloodPressure) which contains the maximum BloodPressure in the table.
> Join this CTE (using an alias b) to the main table (Kidney) to return information about patients with the maximum BloodPressure.

```SQL
-- Create the CTE
WITH BloodPressure (MaxBloodPressure)
AS (SELECT MAX(BloodPressure) as MaxBloodPressure FROM Kidney)

SELECT *
FROM Kidney a
-- Join the CTE
JOIN BloodPressure b
ON a.BloodPressure = b.MaxBloodPressure
```
