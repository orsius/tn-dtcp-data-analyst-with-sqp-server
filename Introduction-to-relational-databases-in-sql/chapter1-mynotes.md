#

## Query information_schema with SELECT

> information_schema is a meta-database that holds information about your current database. information_schema has multiple tables you can query with the known SELECT \* FROM syntax:
>
> - tables: information about all tables in your current database
> - columns: information about all columns in all of the tables in your current database

1. Get information on all table names in the current database, while limiting your query to the 'public' table_schema.

```SQL
-- Query the right table in information_schema
SELECT table_name
FROM information_schema.tables
-- Specify the correct table_schema value
WHERE table_schema = 'public';
```

2. Now have a look at the columns in university_professors by selecting all entries in information_schema.columns that correspond to that table.

```SQL
-- Query the right table in information_schema to get columns
SELECT column_name, data_type
FROM information_schema.columns
WHERE table_name = 'university_professors' AND table_schema = 'public';
```

3. How many columns does the table university_professors have?

```SQL
SELECT count(column_name)
FROM information_schema.columns
WHERE table_name = 'university_professors' AND table_schema = 'public';
```

4. Finally, print the first five rows of the university_professors table.

```SQL
-- Query the first five rows of our table
select *
from university_professors
LIMIT 1;
```

## CREATE your first few TABLEs

1. Create a table professors with two text columns: firstname and lastname.

```SQL
-- Create a table for the professors entity type
CREATE TABLE professors (
 firstname text,
 lastname text
);
-- Print the contents of this table
SELECT *
FROM professors
```

2. Create a table universities with three text columns: university_shortname, university, and university_city.

```SQL
-- Create a table for the universities entity type
create table universities (
    university_shortname text,
    university text,
    university_city text
);
-- Print the contents of this table
SELECT *
FROM universities
```

## ADD a COLUMN with ALTER TABLE

1. Alter professors to add the text column university_shortname.

```SQL
-- Add the university_shortname column
alter table professors
add column  university_shortname text;

-- Print the contents of this table
SELECT *
FROM professors
```

## RENAME and DROP COLUMNs in affiliations

1. Rename the organisation column to organization in affiliations.
2. Delete the university_shortname column in affiliations.

```SQL
-- Rename the organisation column
ALTER TABLE affiliations
RENAME COLUMN organisation TO organization;

-- Delete the university_shortname column
alter table affiliations
drop column university_shortname;
```

## Migrate data with INSERT INTO SELECT DISTINCT

1. - Insert all DISTINCT professors from university_professors into professors.
   - Print all the rows in professors.

```SQL
-- Insert unique professors into the new table
insert into professors
SELECT DISTINCT firstname, lastname, university_shortname
FROM university_professors;

-- Doublecheck the contents of professors
SELECT *
FROM professors;
```

2. Insert all DISTINCT affiliations into affiliations from university_professors.

```SQL
-- Insert unique affiliations into the new table
insert into affiliations
select distinct firstname, lastname, function, organization
FROM university_professors;

-- Doublecheck the contents of affiliations
SELECT *
FROM affiliations;
```

## Delete tables with DROP TABLE

```SQL
-- Delete the university_professors table
drop table university_professors;
```
