# chapter 3 - Keys and superkeys

## Get to know SELECT COUNT DISTINCT

Your database doesn't have any defined keys so far, and you don't know which columns or combinations of columns are suited as keys.
There's a simple way of finding out whether a certain column (or a combination) contains only unique values – and thus identifies the records in the table.
You already know the SELECT DISTINCT query from the first chapter. Now you just have to wrap everything within the COUNT() function and PostgreSQL will return the number of unique rows for the given columns:

```
SELECT COUNT(DISTINCT(column_a, column_b, ...))
FROM table;
```

1. First, find out the number of rows in universities.

```SQL
-- Count the number of rows in universities
SELECT count(*) FROM universities;
```

2. Then, find out how many unique values there are in the university_city column.

```SQL
-- Count the number of distinct values in the university_city column
SELECT count(distinct(university_city))
FROM universities;
```

## Identify keys with SELECT COUNT DISTINCT

> Query the database to find out the number of rows for the different combination of columns:

```
SELECT COUNT(DISTINCT(column_a, column_b, ...))
FROM professors;
```

If the resulting number equals 551, remove a column to see whether 551 decreases or not. If it doesn't, you have found the correct combination of attributes that form a key.

```SQL
SELECT COUNT(DISTINCT(firstname, lastname))
FROM professors;
```

> Indeed, the only combination that uniquely identifies professors is `{firstname, lastname}`.
> `{firstname, lastname, university_shortname}` is a superkey, and all other combinations give duplicate values. Hopefully, the concept of superkeys and keys is now a bit more clear. Let's move on to primary keys!

# Primary keys

- One primary key per database table, chosen from candidate keys
- Uniquely identifies records, e.g. for referencing in other tables
- Unique and not-null constraints both apply

## Identify the primary key

> Have a look at the example table below; As the database designer, you have to make a wise choice as to which column should be the primary key.

```
     license_no     | serial_no |    make    |  model  | year
--------------------+-----------+------------+---------+------
 Texas ABC-739      | A69352    | Ford       | Mustang |    2
 Florida TVP-347    | B43696    | Oldsmobile | Cutlass |    5
 New York MPO-22    | X83554    | Oldsmobile | Delta   |    1
 California 432-TFY | C43742    | Mercedes   | 190-D   |   99
 California RSK-629 | Y82935    | Toyota     | Camry   |    4
 Texas RSK-629      | U028365   | Jaguar     | XJS     |    4
```

- Which of the following column or column combinations could best serve as primary key?

- [ ] PK = {make}
- [ ] PK = {model, year}
- [x] PK = {license_no}
  > Correct! A primary key consisting solely of "license_no" is probably the wisest choice, as license numbers are certainly unique across all registered cars in a country.

## ADD key CONSTRAINTs to the tables

> Two of the tables in your database already have well-suited candidate keys consisting of one column each: organizations and universities with the organization and university_shortname columns, respectively.

In this exercise, you'll rename these columns to id using the RENAME COLUMN command and then specify primary key constraints for them. This is as straightforward as adding unique constraints (see the last exercise of Chapter 2):

```
ALTER TABLE table_name
ADD CONSTRAINT some_name PRIMARY KEY (column_name)
```

> Note that you can also specify more than one column in the brackets.

- Rename the organization column to id in organizations.
- Make id a primary key and name it organization_pk.

```SQL
-- Rename the organization column to id
alter table organizations
rename column organization TO id;

-- Make id a primary key
ALTER TABLE organizations
add constraint organization_pk primary KEY (id);
```

- Rename the university_shortname column to id in universities.
- Make id a primary key and name it university_pk.

```SQL
-- Rename the university_shortname column to id
alter table universities
rename column university_shortname to id;

-- Make id a primary key
alter table universities
add constraint university_pk primary key (id);
```

# Surrogate keys

> Surrogate keys are sort of an **artificial primary key**. In other words, they are not based on a native column in your data, but on a column that just exists for the sake of having a primary key. Why would you need that?

> There are several reasons for creating an artificial surrogate key. As mentioned before, a primary key is ideally constructed from as few columns as possible. Secondly, the primary key of a record should never change over time. If you define an artificial primary key, ideally consisting of a unique number or string, you can be sure that this number stays the same for each record. **Other attributes might change, but the primary key always has the same value for a given record**.

## Add a SERIAL surrogate key

1. Add a new column id with data type serial to the professors table.

```SQL
-- Add the new column to the table
ALTER TABLE professors
add column id serial;
```

2. Make id a primary key and name it professors_pkey.

```SQL
-- Add the new column to the table
ALTER TABLE professors
ADD COLUMN id serial;

-- Make id a primary key
ALTER TABLE professors
ADD CONSTRAINT professors_pkey PRIMARY KEY (id);

```

3. Write a query that returns all the columns and 10 rows from professors.

```
-- Have a look at the first 10 rows of professors
select * from professors limit 10;
```

## CONCATenate columns to a surrogate key

> Another strategy to add a surrogate key to an existing table is to concatenate existing columns with the CONCAT() function.

Let's think of the following example table:

```SQL
CREATE TABLE cars (
make varchar(64) NOT NULL,
model varchar(64) NOT NULL,
mpg integer NOT NULL
)
```

The table is populated with 10 rows of completely fictional data.
Unfortunately, the table doesn't have a primary key yet. None of the columns consists of only unique values, so some columns can be combined to form a key.
In the course of the following exercises, you will combine make and model into such a surrogate key.

1. Count the number of distinct rows with a combination of the make and model columns.

```SQL
-- Count the number of distinct rows with columns make, model
select count(distinct(make, model))
FROM cars;
```

2. Add a new column id with the data type varchar(128).

```SQL
-- Count the number of distinct rows with columns make, model
SELECT COUNT(DISTINCT(make, model))
FROM cars;

-- Add the id column
ALTER TABLE cars
add column id varchar(128);
```

3. Concatenate make and model into id using an UPDATE table_name SET column_name = ... query and the CONCAT() function.

```SQL
-- Count the number of distinct rows with columns make, model
SELECT COUNT(DISTINCT(make, model))
FROM cars;

-- Add the id column
ALTER TABLE cars
ADD COLUMN id varchar(128);

-- Update id with make + model
UPDATE cars
set id = concat(make, model);
```

4. Make id a primary key and name it id_pk.

```SQL
-- Count the number of distinct rows with columns make, model
SELECT COUNT(DISTINCT(make, model))
FROM cars;

-- Add the id column
ALTER TABLE cars
ADD COLUMN id varchar(128);

-- Update id with make + model
UPDATE cars
SET id = CONCAT(make, model);

-- Make id a primary key
ALTER TABLE cars
ADD constraint id_pk primary key(id);

-- Have a look at the table
SELECT * FROM cars;
```

## Test your knowledge before advancing

> Let's think of an entity type "student". A student has:

- a last name consisting of up to 128 characters (required),
- a unique social security number, consisting only of integers, that should serve as a key,
- a phone number of fixed length 12, consisting of numbers and characters (but some students don't have one).

Given the above description of a student entity, create a table students with the correct column types.

- Add a PRIMARY KEY for the social security number ssn.
- Note that there is no formal length requirement for the integer column. The application would have to make sure it's a correct SSN!

```SQL
-- Create the table
Create table students (
  last_name varchar(128) NOT NULL,
  ssn int primary key,
  phone_no char(12)
);
```

# Model 1:N relationships with foreign keys

> - A foreign key (FK) points to the primary key (PK) of another table
> - Dmain of FK must be equal to domain of PK
> - Each value of FK must exist in PK of the other table (FK constraint or "referential integrity")
> - FKs are not acutal keys _(A foreign key is not necessarily an actual key, because duplicates and "NULL" values are allowed)_

## REFERENCE a table with a FOREIGN KEY

REFERENCE a table with a FOREIGN KEY
In your database, you want the professors table to reference the universities table. You can do that by specifying a column in professors table that references a column in the universities table.

As just shown in the video, the syntax for that looks like this:

```SQL
ALTER TABLE a
ADD CONSTRAINT a_fkey FOREIGN KEY (b_id) REFERENCES b (id);
```

Table a should now refer to table b, via b_id, which points to id. a_fkey is, as usual, a constraint name you can choose on your own.

> Pay attention to the naming convention employed here: Usually, a foreign key referencing another primary key with name id is named x_id, where x is the name of the referencing table in the singular form.

1. Rename the university_shortname column to university_id in professors.

```SQL
-- Rename the university_shortname column
ALTER TABLE professors
rename column university_shortname to university_id;
```

2. Add a foreign key on university_id column in professors that references the id column in universities.
   Name this foreign key professors_fkey.

```SQL
-- Rename the university_shortname column
ALTER TABLE professors
RENAME COLUMN university_shortname TO university_id;

-- Add a foreign key on professors referencing universities
alter table professors
add constraint professors_fkey FOREIGN KEY (university_id) REFERENCES universities (id);
```

Now, the professors table has a link to the universities table. Each professor belongs to exactly one university.

## Explore foreign key constraints

Foreign key constraints help you to keep order in your database mini-world. In your database, for instance, only professors belonging to Swiss universities should be allowed, as only Swiss universities are part of the universities table.

The foreign key on professors referencing universities you just created thus makes sure that only existing universities can be specified when inserting new data. Let's test this!

- Run the sample code and have a look at the error message.
- What's wrong? Correct the university_id so that it actually reflects where Albert Einstein wrote his dissertation and became a professor – at the University of Zurich (UZH)!

```SQL
-- Try to insert a new professor
INSERT INTO professors (firstname, lastname, university_id)
VALUES ('Albert', 'Einstein', 'MIT');
```

```
insert or update on table "professors" violates foreign key constraint "professors_fkey"
DETAIL:  Key (university_id)=(MIT) is not present in table "universities".
```

```SQL
-- Try to insert a new professor
INSERT INTO professors (firstname, lastname, university_id)
VALUES ('Albert', 'Einstein', 'UZH');
```

## JOIN tables linked by a foreign key

Here's a quick recap on how joins generally work:

```SQL
SELECT ...
FROM table_a
JOIN table_b
ON ...
WHERE ...
```

> While foreign keys and primary keys are not strictly necessary for join queries, they greatly help by telling you what to expect. For instance, you can be sure that records referenced from table A will always be present in table B – so a join from table A will always find something in table B. If not, the foreign key constraint would be violated.

- JOIN professors with universities on professors.university_id = universities.id, i.e., retain all records where the foreign key of professors is equal to the primary key of universities.
- Filter for university_city = 'Zurich'.

```SQL
-- Select all professors working for universities in the city of Zurich
SELECT professors.lastname, universities.id, universities.university_city
from professors
join universities
ON professors.university_id = universities.id
where universities.university_city = 'Zurich';
```

# Model more complex relationships

- Create a table
- Add foreign keys for every connected table
- Add additional aributes

```SQL
CREATE TABLE affiliations (
  professor_id integer REFERENCES professors (id),
  organization_id varchar(256) REFERENCES organizations (id),
  function varchar(256)
);
```

- No primary key!
- Possible PK = {professor_id, organization_id, function}

## Add foreign keys to the "affiliations" table

At the moment, the affiliations table has the structure `{firstname, lastname, function, organization}`, as you can see in the preview at the bottom right.
In the next three exercises, you're going to turn this table into the form `{professor_id, organization_id, function}`, with professor_id and organization_id being foreign keys that point to the respective tables.

You're going to transform the affiliations table in-place, i.e., without creating a temporary table to cache your intermediate results.

1. Add a professor_id column with integer data type to affiliations, and declare it to be a foreign key that references the id column in professors.

```SQL
-- Add a professor_id column
ALTER TABLE affiliations
ADD COLUMN professor_id integer REFERENCES professors (id);
```

2. Rename the organization column in affiliations to organization_id.

```SQL
-- Add a professor_id column
ALTER TABLE affiliations
ADD COLUMN professor_id integer REFERENCES professors (id);

-- Rename the organization column to organization_id
alter table affiliations
rename organization TO organization_id;
```

3. Add a foreign key constraint on organization_id so that it references the id column in organizations.

```SQL
-- Add a professor_id column
ALTER TABLE affiliations
ADD COLUMN professor_id integer REFERENCES professors (id);

-- Rename the organization column to organization_id
ALTER TABLE affiliations
RENAME organization TO organization_id;

-- Add a foreign key on organization_id
ALTER TABLE affiliations
ADD CONSTRAINT affiliations_organization_fkey foreign key (organization_id) REFERENCES organizations (id);
```

> Making organization_id a foreign key worked flawlessly because these organizations actually exist in the organizations table.
> That was only the first part, though. Now it's time to update professor_id in affiliations – so that it correctly refers to the corresponding professors.

## Populate the "professor_id" column

Now it's time to also populate professors_id. You'll take the ID directly from professors.

Here's a way to update columns of a table based on values in another table:

```SQL
UPDATE table_a
SET column_to_update = table_b.column_to_update_from
FROM table_b
WHERE condition1 AND condition2 AND ...;
```

This query does the following:

> For each row in table_a, find the corresponding row in table_b where condition1, condition2, etc., are met.
> Set the value of column_to_update to the value of column_to_update_from (from that corresponding row).
> The conditions usually compare other columns of both tables, e.g. table_a.some_column = table_b.some_column. Of course, this query only makes sense if there is only one matching row in table_b.

1. First, have a look at the current state of affiliations by fetching 10 rows and all columns.
2. Update the professor_id column with the corresponding value of the id column in professors. "Corresponding" means rows in professors where the firstname and lastname are identical to the ones in affiliations.
3. Check out the first 10 rows and all columns of affiliations again. Have the professor_ids been correctly matched?

```SQL
-- Have a look at the 10 first rows of affiliations
select * from affiliations limit 10;
-- Update professor_id to professors.id where firstname, lastname correspond to rows in professors
UPDATE affiliations
SET professor_id = professors.id
FROM professors
WHERE affiliations.firstname = professors.firstname AND affiliations.lastname = professors.lastname;

-- Have a look at the 10 first rows of affiliations again
SELECT *
FROM affiliations
LIMIT 10;
```

## Drop "firstname" and "lastname"

- Drop the firstname and lastname columns from the affiliations table.

```SQL
-- Drop the firstname column
alter table affiliations
DROP column firstname;

-- Drop the lastname column
alter table affiliations
drop column lastname;
```

# Referential integrity

- A record referencing another table must refer to an existing record in that table
- Specied between two tables
- Enforced through foreign keys

## Referential integrity violations

```SQL
DELETE FROM universities WHERE id = 'EPF';
```

```
update or delete on table "universities" violates foreign key constraint "professors_fkey" on table "professors"
DETAIL:  Key (id)=(EPF) is still referenced from table "professors".
```

> -> You defined a foreign key on professors.university_id that references universities.id, so referential integrity is said to hold from professors to universities

## Change the referential integrity behavior of a key

So far, you implemented three foreign key constraints:

1. professors.university_id to universities.id
2. affiliations.organization_id to organizations.id
3. affiliations.professor_id to professors.id
   These foreign keys currently have the behavior ON DELETE NO ACTION. Here, you're going to change that behavior for the column referencing organizations from affiliations. If an organization is deleted, all its affiliations (by any professor) should also be deleted.

Altering a key constraint doesn't work with ALTER COLUMN. Instead, you have to DROP the key constraint and then ADD a new one with a different ON DELETE behavior.

For deleting constraints, though, you need to know their name. This information is also stored in information_schema.

1. Have a look at the existing foreign key constraints by querying table_constraints in information_schema.

```SQL
-- Identify the correct constraint name
SELECT constraint_name, table_name, constraint_type
FROM information_schema.table_constraints
WHERE constraint_type = 'FOREIGN KEY';
```

2. Delete the affiliations_organization_id_fkey foreign key constraint in affiliations.

```SQL

-- Drop the right foreign key constraint
ALTER table affiliations
drop CONSTRAINT affiliations_organization_id_fkey;
```

3. Add a new foreign key to affiliations that CASCADEs deletion if a referenced record is deleted from organizations. Name it affiliations_organization_id_fkey.

```SQL
-- Add a new foreign key constraint from affiliations to organizations which cascades deletion
ALTER TABLE affiliations
add constraint affiliations_organization_id_fkey foreign KEY (organization_id) REFERENCES organizations (id) on delete cascade;
```

4. Run the DELETE and SELECT queries to double check that the deletion cascade actually works.

```SQL
-- Delete an organization
DELETE FROM organizations
WHERE id = 'CUREM';

-- Check that no more affiliations with this organization exist
SELECT * FROM affiliations
WHERE organization_id = 'CUREM';
```

# Roundup

## Count affiliations per university

Now that your data is ready for analysis, let's run some exemplary SQL queries on the database. You'll now use already known concepts such as grouping by columns and joining tables.

In this exercise, you will find out which university has the most affiliations (through its professors). For that, you need both affiliations and professors tables, as the latter also holds the university_id.

As a quick repetition, remember that joins have the following structure:
```
SELECT table_a.column1, table_a.column2, table_b.column1, ... 
FROM table_a
JOIN table_b 
ON table_a.column = table_b.column
```
This results in a combination of table_a and table_b, but only with rows where table_a.column is equal to table_b.column.
- Count the number of total affiliations by university.
- Sort the result by that count, in descending order.



```SQL
-- Count the total number of affiliations per university
SELECT count(*), professors.university_id 
FROM affiliations
JOIN professors
ON affiliations.professor_id = professors.id
-- Group by the university ids of professors
GROUP BY professors.university_id 
order by count DESC;
```

## Join all the tables together
1. Join all tables in the database (starting with affiliations, professors, organizations, and universities) and look at the result.




```SQL
-- Join all tables
SELECT *
FROM affiliations
JOIN professors
ON affiliations.professor_id = professors.id
JOIN organizations
ON affiliations.organization_id = organizations.id
JOIN universities
ON professors.university_id = universities.id;
```

2. Now group the result by organization sector, professor, and university city.
   Count the resulting number of rows.

```SQL
-- Group the table by organization sector, professor ID and university city
SELECT count(*), organizations.organization_sector, 
professors.id, universities.university_city
FROM affiliations
JOIN professors
ON affiliations.professor_id = professors.id
JOIN organizations
ON affiliations.organization_id = organizations.id
JOIN universities
ON professors.university_id = universities.id
GROUP BY organizations.organization_sector, 
professors.id, universities.university_city;
```

3. Only retain rows with "Media & communication" as organization sector, and sort the table by count, in descending order.




```SQL
-- Filter the table and sort it
SELECT COUNT(*), organizations.organization_sector, 
professors.id, universities.university_city
FROM affiliations
JOIN professors
ON affiliations.professor_id = professors.id
JOIN organizations
ON affiliations.organization_id = organizations.id
JOIN universities
ON professors.university_id = universities.id
WHERE organizations.organization_sector = 'Media & communication'
GROUP BY organizations.organization_sector, 
professors.id, universities.university_city
ORDER BY count DESC;
```
> The professor with id 538 has the most affiliations in the "Media & communication" sector, and he or she lives in the city of Lausanne. Thanks to your database design, you can be sure that the data you've just queried is consistent. Of course, you could also put university_city and organization_sector in their own tables, making the data model even more formal. However, in database design, you have to strike a balance between modeling overhead, desired data consistency, and usability for queries like the one you've just wrote. Congratulations, you made it to the end!
##

```SQL

```

##

```SQL

```

##

```SQL

```

##

```SQL

```
