# SQL-Server-Notes-DataCamp
This notes contain all sql queries from SQL Server course on DataCamp along with my personal tricks to understand and remember SQL.

# Introduction to SQL- SELECTion Box

**SQL Server - relational database system developed by Microsoft**

**Transact-SQL (T-SQL) - Microsoft's implementation of SQL, with additional functionality**


- *Return 5 rows - Can change the number of rows*

```SQL
SELECT TOP(5) artist FROM artists;
```


- *Return top 5% of rows*

```SQL
SELECT TOP(5) PERCENT artist FROM artists;
```


- *Return unique rows*

```SQL
SELECT DISTINCT nerc_region FROM grid;
```


- *Aliasing column names with AS --Simply return column with Aliase name*

```SQL
SELECT demand_loss_mw AS lost_demand FROM grid;
```

| lost_demand |
|-------------|
| 424         |
| 217         |
| 494         |
| 338         |
| 3900        |
| 3300        |



## ORDER BY
```SQL
SELECT TOP (10) product_id, year_intro
FROM products
ORDER BY year_intro DESC, product_id;
```

***DESC keyword will sort the column(yesr_intro) in descending order. If there is no explicite keyword is mentioned,
then it will sort column(product_id) in ascending order.***
***In above query, there are multiple columns to sort with. So, execution will start by first sorting "year_intro" in 
descending order. After that "product_id" will be sorted in ascending order accordingly.***


## What if we only wanted to return rows that met certain criteria?
```SQL
 SELECT customer_id, total
 FROM invoice
 WHERE total > 15;
```

- *Rows with points greater than 10*
```SQL
WHERE points > 10
```


- *Rows with points less than 10*
```SQL
WHERE points < 10
```


- *Rows with points greater than or equal to 10*
```SQL
WHERE points >= 10
```


- *Rows with points less than or equal to 20*
```SQL
WHERE points <= 20
```


- *Character data type*
```SQL
WHERE country = 'Spain'
```


- *Date data type*
```SQL
WHERE event_date = '2012-01-02'
```


**For not-equal, use "<>" just like != in JAVA
```SQL
 SELECT customer_id, total
 FROM invoice
 WHERE total <> 10;
```

## BETWEEN
```SQL
SELECT customer_id, total
 FROM invoice
 WHERE total BETWEEN 20 AND 30;
```

## NOT BETWEEN
```SQL
SELECT customer_id, total
 FROM invoice
 WHERE total NOT BETWEEN 20 AND 30;
```

## NULL
*NULL indicates there is no value for that record*
*NULLs help highlight gaps in our data*
```SQL
 SELECT TOP (6) total, billing_state FROM invoice
 WHERE billing_state IS NULL;
```

## NOT NULL
```SQL
 SELECT TOP (6) total, billing_state FROM invoice
 WHERE billing_state IS NOT NULL;
```

## AND/OR for multiple conditions
```SQL
 SELECT song, artist FROM songlist 
 WHERE artist = 'AC/DC' AND release_year < 1980;
```

## Using paranthesis ( )


**Poblem:** 
```SQL
SELECT *                                  SELECT * FROM songlist
FROM songlist                             WHERE artist = 'Green Day'
WHERE                                     AND release_year = 1994;
artist = 'Green Day'
AND release_year = 1994      ----->                OR
OR release_year > 2000;                              
                                           SELECT * FROM songlist 
                                           WHERE release_year > 2000;
   ```                                        
**Answer:**
```SQL
 SELECT song FROM songlist
 WHERE artist = 'Green Day'
 AND (release_year = 1994
 OR release_year > 2000);
```

**Another way of writing the query:**
```SQL
 SELECT song FROM songlist WHERE 
 (artist = 'Green Day'
 AND release_year = 1994)
 OR 
 (artist = 'Green Day'
 AND release_year > 2000);
```

## IN (Other form of OR)
```SQL
 SELECT song, release_year FROM songlist
 WHERE release_year IN (1985, 1991, 1992);
```
***Can be read as 1985 or 1991 or 1992***


## LIKE
```SQL
 SELECT song FROM songlist
 WHERE song LIKE 'a%';
```
***Return songs start with "a"***




# Introduction to SQL- Groups, strings, and counting things

## SUM 
```SQL
SELECT SUM(affected_customers) AS total_affected
 FROM grid;
```
## COUNT
```SQL
 SELECT COUNT(affected_customers) AS count_affected
 FROM grid;
```
*To count only unique entries*
```SQL
 SELECT COUNT(DISTINCT affected_customers) AS unique_count_affected
 FROM grid;
```
## MIN
```SQL
 SELECT MIN(affected_customers) AS min_affected_custome
 FROM grid;
```
## MAX
```SQL
 SELECT MAX(affected_customers) AS max_affected_customers
 FROM grid;
```
## AVG
```SQL
 SELECT AVG(affected_customers) AS avg_affected_customers
 FROM grid;
```
## LEN (Length of string)
```SQL
 SELECT description, 
 LEN(description) AS description_length
 FROM grid;
```
## LEFT (Retrun n characters from left side of string)
```SQL
 SELECT description,
 LEFT(description, 20) AS first_20_left
 FROM grid;
```

| description                  | first_20_left          |
|------------------------------|-----------------------|
| Severe Weather Thunderstorms | Severe Weather Thun    |
| Severe Weather Thunderstorms | Severe Weather Thun    |
| Severe Weather Thunderstorms | Severe Weather Thun    |
| Fuel Supply Emergency Coal   | Fuel Supply Emergenc   |
| Physical Attack Vandalism    | Physical Attack Van    |

## RIGHT (Retrun n characters from right side of string)
```SQL
 SELECT description,
 RIGHT(description, 20) AS last_20
 FROM grid;
```
| description                   | last_20              |
|-------------------------------|----------------------|
| Severe Weather Thunderstorms  | ather Thunderstorms  |
| Severe Weather Thunderstorms  | ather Thunderstorms  |
| Severe Weather Thunderstorms  | ather Thunderstorms  |
| Fuel Supply Emergency Coal    | pply Emergency Coal  |
| Physical Attack Vandalism     | al Attack Vandalism  |


## CHARINDEX (Return index of first accurance of specified character)
```SQL
 SELECT
 CHARINDEX ('_', url) AS char_location, url
 FROM courses;
```
| char_location | url                                 |
|---------------|-------------------------------------|
| 34            | datacamp.com/courses/introduction_  |
| 34            | datacamp.com/courses/intermediate_  |
| 29            | datacamp.com/courses/writing_       |
| 29            | datacamp.com/courses/joining_       |
| 27            | datacamp.com/courses/intro_         |


## SUBSTRING 
```SQL
 SELECT
 SUBSTRING(url, 12, 12) AS target_section, url
 FROM courses;
```
*In this case, 12 character from index 12.*

| target_section   | url                              |
|------------------|----------------------------------|
| datacamp.com     |https//www.datacamp.com/courses   |


## REPLACE
```SQL
 SELECT
 TOP(5) REPLACE(url, '_' , '-') AS replace_with_hyphen
 FROM courses;
```
*In out case, replace _ with -*


| replace_with_hyphen                 |
|-------------------------------------|
| datacamp.com/courses/introduction-  |
| datacamp.com/courses/intermediate-  |
| datacamp.com/courses/writing-       |
| datacamp.com/courses/joining-       |
| datacamp.com/courses/intro-         |


## Grouping error
```SQL
SELECT
SUM(demand_loss_mw) AS lost_demand, description
FROM grid;
```
***Msg 8120, Level 16, State 1, Line 1
Column 'grid.description' is invalid in the select list because it is not contained in
either an aggregate function or the GROUP BY clause.***


*Solution is GROUP BY*
```SQL
SELECT
SUM(demand_loss_mw) AS lost_demand, description
FROM grid
GROUP BY description;
```

| lost_demand | description |
|-------------|-------------------------------------------------------|
| NULL | Actual Physical Attack |
| NULL | Cold Weather Event |
| NULL | Cyber Event with Potential to Cause Impact |
| 40 | Distribution Interruption |
| 2 | Distribution System Interruption |
| NULL | Earthquake |
| NULL | Electrical Fault at Generator |
| 338 | Electrical System Islanding |
| 24514 | Electrical System Separation Islanding |
| 15 | Electrical System Separation Islanding Severe Weather |

```SQL
SELECT
SUM(demand_loss_mw) AS lost_demand, description
FROM grid
WHERE
description LIKE '%storm'
AND demand_loss_mw IS NOT NULL
GROUP BY description;
```

*Result with no NULL*

| lost_demand | description |
|-------------|--------------------------------------------|
| 996 | Ice Storm |
| 420 | Load Shed Severe Weather Lightning Storm |
| 332 | Major Storm |
| 3 | Severe Weather Thunderstorm |
| 413 | Severe Weather Wind Storm |
| 4171 | Severe Weather Winter Storm |
| 1352 | Winter Storm |


## HAVING (Use as condition after GROUP BY)

**Without HAVING**

```SQL
SELECT
SUM(demand_loss_mw) AS lost_demand, description
FROM grid
WHERE
description LIKE '%storm'
AND demand_loss_mw IS NOT NULL
GROUP BY description;
```
| lost_demand | description |
|-------------|--------------------------------------------|
| 996 | Ice Storm |
| 420 | Load Shed Severe Weather Lightning Storm |
| 332 | Major Storm |
| 3 | Severe Weather Thunderstorm |
| 413 | Severe Weather Wind Storm |
| 4171 | Severe Weather Winter Storm |
| 1352 | Winter Storm |


**Using HAVING**

```SQL
SELECT
SUM(demand_loss_mw) AS lost_demand,
description
FROM grid
WHERE
description LIKE '%storm'
AND demand_loss_mw IS NOT NULL
GROUP BY description
HAVING SUM(demand_loss_mw) > 1000;
```
| lost_demand | description |
|-------------|--------------------------------------------|
| 4171 | Severe Weather Winter Storm |
| 1352 | Winter Storm |


**Summary**

- GROUP BY splits the data up into combinations of one or more values
- WHERE lters on row values
- HAVING appears after the GROUP BY clause and lters on groups or aggregates



# Introduction to SQL- Joining tables

- Primary keys: Uniquely identify each row in a table

**artist Table**


| artist_id | name |
|-----------|-------------------|
| 1 | AC/DC |
| 2 | Accept |
| 3 | Aerosmith |
| 4 | Alanis Morissette |
| 5 | Alice In Chains |

**album table**
| album_id | title | artist_id |
|----------|-------------------------|-----------|
| 1 | For Those About To Rock | 1 |
| 2 | Balls to the Wall | 2 |
| 3 | Restless and Wild | 2 |
| 4 | Let There Be Rock | 1 |
| 5 | Big Ones | 3 |

## INNER JOIN

**INNER JOIN syntax**

```SQL
SELECT
table_A.columnX,
table_A.columnY,
table_B.columnZ
FROM table_A
INNER JOIN table_B ON table_B.foreign_key = table_A.primary_key;
```

```SQL
SELECT
album_id, title, album.artist_id,
name AS artist_name
FROM album
INNER JOIN artist on artist.artist_id = album.artist_id;
```
- Returns all combinations of all matches between album and artist

| album_id | title | artist_id | artist_name |
|----------|---------------|------------------------|-----------|
| 1 | For Those About To Rock | 1 | AC/DC |
| 4 | Let There Be Rock | 1 | AC/DC |
| 2 | Balls To The Wall | 2 | Accept |
| 3 | Restless and Wild | 2 | Accept |


## Multiple INNER JOINS

```SQL
SELECT
table_A.columnX,
table_A.columnY,
table_B.columnZ table_C columnW
FROM table_A
INNER JOIN table_B ON table_B.foreign_key = table_A.primary_key
INNER JOIN table_C ON table_C.foreign_key = table_B.primary_key;
```

## LEFT and RIGHT JOIN

Admissions table

| Patient_ID | Admitted |
|------------|----------|
| 1 | 1 |
| 2 | 1 |
| 3 | 1 |
| 4 | 1 |
| 5 | 1 |

Discharges table

| Patient_ID | Admitted |
|------------|----------|
| 1 | 1 |
| 3 | 1 |
| 4 | 1 |

**LEFT JOIN**
```SQL
SELECT
Admitted.Patient_ID,
Admitted,
Discharged
FROM Admitted
LEFT JOIN Discharged ON Discharged.Patient_ID = Admitted.Patient_ID;
```
| Patient_ID | Admitted | Discharged |
|------------|----------|------------|
| 1 | 1 | 1 |
| 2 | 1 | NULL |
| 3 | 1 | 1 |
| 4 | 1 | 1 |
| 5 | 1 | NULL |

**RIGHT JOIN**
```SQL
SELECT
Admitted.Patient_ID,
Admitted,
Discharged
FROM Discharged
RIGHT JOIN Admitted ON Admitted.Patient_ID = Discharged.Patient_ID;
```

| Patient_ID | Admitted | Discharged |
|------------|----------|------------|
| 1 | 1 | 1 |
| 2 | 1 | NULL |
| 3 | 1 | 1 |
| 4 | 1 | 1 |
| 5 | 1 | NULL |

## Summary

- INNER JOIN : Only returns matching rows
- LEFT JOIN (or RIGHT JOIN ): All rows from the main table plus matches from the joining table
- NULL : Displayed if no match is found
- LEFT JOIN and RIGHT JOIN can be interchangeable


## UNION and UNION ALL

*Mainly combine results of two or more table having same number of column and same data type.

**UNION**
```SQL
SELECT album_id, title, artist_id
FROM album
WHERE artist_id IN (1, 3)
UNION
SELECT album_id, title, artist_id
FROM album
WHERE artist_id IN (1, 4, 5);
```
- Duplicate rows are excluded
| album_id | title | artist_id |
|------------|-------------------------|------------|
| 1 | For Those About To Rock | 1 |
| 4 | Let There Be Rock | 1 |
| 5 | Big Ones | 3 |
| 6 | Jagged Little Pill | 4 |
| 7 | Facelift | 5 |

**UNINON ALL**

```SQL
SELECT album_id, title, artist_id
FROM album
WHERE artist_id IN (1, 3)
UNION ALL
SELECT album_id, title, artist_id
FROM album
WHERE artist_id IN (1, 4, 5);
```
- Includes duplicate rows

 album_id | title | artist_id |
|------------|-------------------------|------------|
| 1 | For Those About To Rock | 1 |
| 4 | Let There Be Rock | 1 |
| 5 | Big Ones | 3 |
| 1 | For Those About To Rock | 1 |
| 4 | Let There Be Rock | 1 |
| 6 | Jagged Little Pill | 4 |
| 7 | Facelift | 5 |

## Summary

- UNION or UNION ALL : Combines queries from the same table or different tables
- If combining data from different tables:
  - Select the same number of columns in the same order
  - Columns should have the same data types
-If source tables have different column names
 -Alias the column names
- UNION : Discards duplicates (slower to run)
- UNION ALL : Includes duplicates (faster to run)



# Introduction to SQL Server: You've got the power

## CRUD operations

**CREATE**
- Databases, Tables or views
- Users, permissions, and security groups
**READ**
- Example: SELECT statements
**UPDATE**
- Amend existing database records
**DELETE**

## CREATE

- CREATE TABLE unique table name
-(column name, data type, size)
```SQL
CREATE TABLE test_table(
test_date date,
test_name varchar(20),
test_int int)
```

## INSERT
```SQL
INSERT INTO table_name (col1, col2, col3)
VALUES
('value1','value2', value3)
```
```SQL
INSERT INTO table_name (col1, col2, col3)
SELECT
 column1,
 column2,
 column3
FROM other_table
WHERE
 -- conditions apply
```
- Don't use SELECT *
- Be specic in case table structure changes

## UPDATE
```SQL
UPDATE table
SET column = value,
WHERE
-- Condition(s);
```
- Don't forget the WHERE clause!
```SQL
UPDATE table
SET
column1 = value1,
column2 = value2
WHERE
-- Condition(s);
```

## DELETE
```SQL
DELETE
FROM table
WHERE
-- Conditions
```
```SQL
TRUNCATE TABLE table_name
```
***Clears the entire table at once***


## Variable

###### DECLARE

**Integer variable:**
```SQL
DECLARE @test_int INT
```
**Varchar variable:**
```SQL
DECLARE @my_artist VARCHAR(100)
```

## SET

**Integer variable:**
```SQL
DECLARE @test_int INT
SET @test_int = 5
```
**Assign value to @my_artist :**
```SQL
DECLARE @my_artist varchar(100)
SET @my_artist = 'AC/DC'
```

```SQL
DECLARE @my_artist varchar(100)
DECLARE @my_album varchar(300);

SET @my_artist = 'AC/DC'
SET @my_album = 'Let There Be Rock' ;

SELECT --
FROM --
WHERE artist = @my_artist
AND album = @my_album;
```

## Temporary tables
```SQL
SELECT
 col1,
 col2,
 col3 INTO #my_temp_table
FROM my_existing_table
WHERE
-- Conditions
```
***#my_temp_table exists until connection or session ends***
- Remove table manually
```SQL
DROP TABLE #my_temp_table
```


# What we learned...

- Selecting: SELECT
- Ordering: ORDER BY
- Filtering: WHERE and HAVING
- Aggregating: SUM , COUNT , MIN , MAX and AVG
- Text manipulation: LEFT , RIGHT , LEN and SUBSTRING
- GROUP BY
- INNER JOIN , LEFT JOIN , RIGHT JOIN
- UNION and UNION ALL
- Create, Read, Update and Delete (CRUD)
- Variables
- Temporary tables
