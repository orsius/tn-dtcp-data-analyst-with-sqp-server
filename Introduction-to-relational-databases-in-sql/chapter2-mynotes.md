# chapter 2

## Types of database constraints

> Which of the following is not used to enforce a database constraint?

- [ ] Foreign keys _(Foreign keys are special constraints on attributes that act as links to other database tables)_
- [x] SQL aggregate functions _Exactly! SQL aggregate functions are not used to enforce constraints, but to do calculations on data._
- [ ] The BIGINT data type _(A data type is a simple form of an attribute constraint, leading to a consistent data type across a database column)_
- [ ] Primary keys Wrong. _(Primary keys are special constraints on attributes that uniquely identify each record in a table)_

## Conforming with data types

```SQL
-- database table that only holds three records.
-- The columns have the data types date, integer, and text, respectively.
CREATE TABLE transactions (
 transaction_date date,
 amount integer,
 fee text
);

-- Let's add a record to the table
INSERT INTO transactions (transaction_date, amount, fee)
VALUES ('2018-24-09', 5454, '30');

-- Doublecheck the contents
SELECT *
FROM transactions;
```

- Execute the given sample code.
- As it doesn't work, have a look at the error message and correct the statement accordingly – then execute it again

```
date/time field value out of range: "2018-24-09"
LINE 3: VALUES ('2018-24-09', 5454, '30');
```

- Have a look at the contents of the transactions table.
- The transaction_date accepts date values. According to the PostgreSQL documentation, it accepts values in the form of YYYY-MM-DD, DD/MM/YY, and so forth.
- _have to change date value to match "format"_

```SQL
-- Let's add a record to the table
INSERT INTO transactions (transaction_date, amount, fee)
VALUES ('2018-09-24', 5454, '30');

-- Doublecheck the contents
SELECT *
FROM transactions;
```

## Type CASTs

> type casts are a possible solution for data type issues. If you know that a certain column stores numbers as text, you can cast the column to a numeric form, i.e. to integer.

```SQL
SELECT CAST(some_column AS integer)
FROM table;
```

Now, the some_column column is temporarily represented as integer instead of text, meaning that you can perform numeric calculations on the column.

- Execute the given sample code.

````
operator does not exist: integer + text
LINE 2: SELECT transaction_date, amount + fee AS net_amount
``
- As it doesn't work, add an integer type cast at the right place and execute it again.
```SQL
-- Calculate the net amount as amount + fee
SELECT transaction_date, amount + CAST(fee AS integer) AS net_amount
FROM transactions;
````

# Working with data types

- Enforced on columns (i.e. attributes)
- Deifne the so-called "domain" of a column
- Define what operations are possible
- Enfore conssitent sotrage of values

## Change types with ALTER COLUMN

he syntax for changing the data type of a column is straightforward. The following code changes the data type of the column_name column in table_name to varchar(10):

```SQL
ALTER TABLE table_name
ALTER COLUMN column_name
TYPE varchar(10)
```

1. Have a look at the distinct university_shortname values in the professors table and take note of the length of the strings.

```SQL
-- Select the university_shortname column
SELECT distinct(university_shortname)
FROM professors;
```

2. Now specify a fixed-length character type with the correct length for university_shortname.

```SQL
-- Specify the correct fixed-length character type
ALTER TABLE professors
ALTER COLUMN university_shortname
TYPE char(3);
```

3. Change the type of the firstname column to varchar(64).

```SQL
-- Change the type of firstname
alter table professors alter column firstname
type varchar(64);
```

## Convert types USING a function

If you don't want to reserve too much space for a certain varchar column, you can truncate the values before converting its type.

For this, you can use the following syntax:

```SQL
ALTER TABLE table_name
ALTER COLUMN column_name
TYPE varchar(x)
USING SUBSTRING(column_name FROM 1 FOR x)
```

> You should read it like this: Because you want to reserve only x characters for column_name, you have to retain a SUBSTRING of every value, i.e. the first x characters of it, and throw away the rest. This way, the values will fit the varchar(x) requirement.

- un the sample code as is and take note of the error.

```SQL
-- Convert the values in firstname to a max. of 16 characters
ALTER TABLE professors
ALTER COLUMN firstname
TYPE varchar(16)
```

```
value too long for type character varying(16)
```

- Now use `SUBSTRING()` to reduce firstname to 16 characters so its type can be altered to `varchar(16)`.

```SQL
-- Convert the values in firstname to a max. of 16 characters
ALTER TABLE professors
ALTER COLUMN firstname
TYPE varchar(16)
using substring(firstname from 1 for 16)
```

# The not-null and unique constraints

## Disallow NULL values with SET NOT NULL

1. Add a not-null constraint for the firstname column.

```SQL
-- Disallow NULL values in firstname
alter table professors
ALTER COLUMN firstname SET NOT NULL;
```

2. Add a not-null constraint for the lastname column.

```SQL
-- Disallow NULL values in lastname
alter table professors
alter column lastname set not null;
```

## What happens if you try to enter NULLs?

```SQL
INSERT INTO professors (firstname, lastname, university_shortname)
VALUES (NULL, 'Miller', 'ETH');
```

```
null value in column "firstname" violates not-null constraint
DETAIL:  Failing row contains (null, Miller, ETH).
```

## Make your columns UNIQUE with ADD CONSTRAINT

1. Add a unique constraint to the university_shortname column in universities. Give it the name university_shortname_unq.

```SQL
-- Make universities.university_shortname unique
ALTER table universities
ADD constraint university_shortname_unq UNIQUE(university_shortname);
```

2. Add a unique constraint to the organization column in organizations. Give it the name organization_unq.

```SQL
-- Make organizations.organization unique
alter table organizations
add constraint organization_unq unique(organization)
```
