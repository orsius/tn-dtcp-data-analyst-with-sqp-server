# Time Series Analysis in SQL Server by Kevin Feasel

## Chapiter 1 - Working with Dates and Times

### Building dates

#### Break out a date into year, month, and day

> Use the YEAR(), MONTH(), and DAY() functions to determine the year, month, and day for the current date and time.

```SQL
DECLARE
	@SomeTime DATETIME2(7) = SYSUTCDATETIME();
-- Retrieve the year, month, and day
SELECT
	YEAR(@SomeTime) AS TheYear,
	MONTH(@SomeTime) AS TheMonth,
	DAY(@SomeTime) AS TheDay;
```

#### Break a date and time into component parts

> 1. Using the DATEPART() function, fill in the appropriate date parts. For a list of parts, review https://docs.microsoft.com/en-us/sql/t-sql/functions/datepart-transact-sql

```SQL
DECLARE
	@BerlinWallFalls DATETIME2(7) = '1989-11-09 23:49:36.2294852';

-- Fill in each date part
SELECT
	DATEPART(yy, @BerlinWallFalls) AS TheYear,
	DATEPART(mm, @BerlinWallFalls) AS TheMonth,
	DATEPART(dd, @BerlinWallFalls) AS TheDay,
	DATEPART(dy, @BerlinWallFalls) AS TheDayOfYear,
    -- Day of week is WEEKDAY
	DATEPART(WEEKDAY, @BerlinWallFalls) AS TheDayOfWeek,
	DATEPART(ww, @BerlinWallFalls) AS TheWeek,
	DATEPART(ss, @BerlinWallFalls) AS TheSecond,
	DATEPART(ns, @BerlinWallFalls) AS TheNanosecond;
```

> 2. Using the DATENAME() function, fill in the appropriate function calls.

```sql
DECLARE
	@BerlinWallFalls DATETIME2(7) = '1989-11-09 23:49:36.2294852';

-- Fill in the function to show the name of each date part
SELECT
	DATENAME(YEAR, @BerlinWallFalls) AS TheYear,
	DATENAME(MONTH, @BerlinWallFalls) AS TheMonth,
	DATENAME(DAY, @BerlinWallFalls) AS TheDay,
	DATENAME(DAYOFYEAR, @BerlinWallFalls) AS TheDayOfYear,
    -- Day of week is WEEKDAY
	DATENAME(WEEKDAY, @BerlinWallFalls) AS TheDayOfWeek,
	DATENAME(WEEK, @BerlinWallFalls) AS TheWeek,
	DATENAME(SECOND, @BerlinWallFalls) AS TheSecond,
	DATENAME(NANOSECOND, @BerlinWallFalls) AS TheNanosecond;
```

> 3. How many DATENAME() results differ from their DATEPART() counterparts?
>    `2` ; The only two date parts which differ are MONTH and WEEKDAY, which return _locale-sensitive string_ results for `DATENAME()` and _numeric values_ for `DATEPART()`

#### Date math and leap years

> 1. Fill in the date parts and intervals needed to determine how SQL Server works on February 29th of a leap year.
>    2012 was a leap year. The leap year before it was 4 years earlier, and the leap year after it was 4 years later.

```SQL

```

####

>

```SQL

```

####

>

```SQL

```

####

>

```SQL

```

####

>

```SQL

```

####

>

```SQL

```

####

>

```SQL

```

####

>

```SQL

```

####

>

```SQL

```

####

>

```SQL

```

####

>

```SQL

```

####

>

```SQL

```

####

>

```SQL

```

####

>

```SQL

```

####

>

```SQL

```

####

>

```SQL

```
