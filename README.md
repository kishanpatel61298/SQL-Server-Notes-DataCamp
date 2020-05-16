# SQL-Server-Notes-DataCamp
This notes contain all sql queries from SQL Server course on DataCamp along with my personal tricks to understand and remember SQL.

# Introduction to SQL- SELECTion Box

**SQL Server - relational database system developed by Microsoft**

**Transact-SQL (T-SQL) - Microsoft's implementation of SQL, with additional functionality**


*Return 5 rows - Can change the number of rows*

> SELECT TOP(5) artist FROM artists;


*Return top 5% of rows*

> SELECT TOP(5) PERCENT artist FROM artists;


*Return unique rows*

> SELECT DISTINCT nerc_region FROM grid;


*Aliasing column names with AS --Simply return column with Aliase name*

>SELECT demand_loss_mw AS lost_demand FROM grid;

| lost_demand |
|-------------|
| 424         |
| 217         |
| 494         |
| 338         |
| 3900        |
| 3300        |



## ORDER BY

SELECT TOP (10) product_id, year_intro
FROM products
ORDER BY year_intro DESC, product_id;


***DESC keyword will sort the column(yesr_intro) in descending order. If there is no explicite keyword is mentioned,
then it will sort column(product_id) in ascending order.***
***In above query, there are multiple columns to sort with. So, execution will start by first sorting "year_intro" in 
descending order. After that "product_id" will be sorted in ascending order accordingly.***


## What if we only wanted to return rows that met certain criteria?

> SELECT customer_id, total
> FROM invoice
> WHERE total > 15;


*Rows with points greater than 10*
> WHERE points > 10


*Rows with points less than 10*
>WHERE points < 10


*Rows with points greater than or equal to 10*
> WHERE points >= 10


*Rows with points less than or equal to 20*
> WHERE points <= 20


*Character data type*
> WHERE country = 'Spain'


*Date data type*
> WHERE event_date = '2012-01-02'


**For not-equal, use "<>" just like != in JAVA

> SELECT customer_id, total
> FROM invoice
> WHERE total <> 10;


## BETWEEN
> SELECT customer_id, total
> FROM invoice
> WHERE total BETWEEN 20 AND 30;


## NOT BETWEEN
> SELECT customer_id, total
> FROM invoice
> WHERE total NOT BETWEEN 20 AND 30;


## NULL
*NULL indicates there is no value for that record*
*NULLs help highlight gaps in our data*

> SELECT TOP (6) total, billing_state FROM invoice
> WHERE billing_state IS NULL;


## NOT NULL

> SELECT TOP (6) total, billing_state FROM invoice
> WHERE billing_state IS NOT NULL;


## AND/OR for multiple conditions

> SELECT song, artist FROM songlist 
> WHERE artist = 'AC/DC' AND release_year < 1980;


## Using paranthesis ( )


**Poblem:** 

SELECT *                                  SELECT * FROM songlist
FROM songlist                             WHERE artist = 'Green Day'
WHERE                                     AND release_year = 1994;
artist = 'Green Day'
AND release_year = 1994      ----->                OR
OR release_year > 2000;                              
                                           SELECT * FROM songlist 
                                           WHERE release_year > 2000;
                                           
**Answer:**

> SELECT song FROM songlist
> WHERE artist = 'Green Day'
> AND (release_year = 1994
> OR release_year > 2000);


**Another way of writing the query:**

> SELECT song FROM songlist WHERE 
> (artist = 'Green Day'
> AND release_year = 1994)
> OR 
> (artist = 'Green Day'
> AND release_year > 2000);


## IN (Other form of OR)

> SELECT song, release_year FROM songlist
> WHERE release_year IN (1985, 1991, 1992);

***Can be read as 1985 or 1991 or 1992***


## LIKE

> SELECT song FROM songlist
> WHERE song LIKE 'a%';

***Return songs start with "a"***




# Introduction to SQL- Groups, strings, and counting things

## SUM 
> SELECT SUM(affected_customers) AS total_affected
> FROM grid;

## COUNT

> SELECT COUNT(affected_customers) AS count_affected
> FROM grid;

*To count only unique entries*

> SELECT COUNT(DISTINCT affected_customers) AS unique_count_affected
> FROM grid;

## MIN

> SELECT MIN(affected_customers) AS min_affected_custome
> FROM grid;

## MAX

> SELECT MAX(affected_customers) AS max_affected_customers
> FROM grid;

## AVG

> SELECT AVG(affected_customers) AS avg_affected_customers
> FROM grid;

## LEN (Length of string)

> SELECT description, 
> LEN(description) AS description_length
> FROM grid;

## LEFT (Retrun n characters from left side of string)

> SELECT description,
> LEFT(description, 20) AS first_20_left
> FROM grid;


| description                  | first_20_left          |
|------------------------------|-----------------------|
| Severe Weather Thunderstorms | Severe Weather Thun    |
| Severe Weather Thunderstorms | Severe Weather Thun    |
| Severe Weather Thunderstorms | Severe Weather Thun    |
| Fuel Supply Emergency Coal   | Fuel Supply Emergenc   |
| Physical Attack Vandalism    | Physical Attack Van    |

## RIGHT (Retrun n characters from right side of string)

> SELECT description,
> RIGHT(description, 20) AS last_20
> FROM grid;

| description                   | last_20              |
|-------------------------------|----------------------|
| Severe Weather Thunderstorms  | ather Thunderstorms  |
| Severe Weather Thunderstorms  | ather Thunderstorms  |
| Severe Weather Thunderstorms  | ather Thunderstorms  |
| Fuel Supply Emergency Coal    | pply Emergency Coal  |
| Physical Attack Vandalism     | al Attack Vandalism  |


## CHARINDEX (Return index of first accurance of specified character)

> SELECT
> CHARINDEX ('_', url) AS char_location, url
> FROM courses;

| char_location | url                                 |
|---------------|-------------------------------------|
| 34            | datacamp.com/courses/introduction_  |
| 34            | datacamp.com/courses/intermediate_  |
| 29            | datacamp.com/courses/writing_       |
| 29            | datacamp.com/courses/joining_       |
| 27            | datacamp.com/courses/intro_         |


## SUBSTRING 

> SELECT
> SUBSTRING(url, 12, 12) AS target_section, url
> FROM courses;

*In this case, 12 character from index 12.*

| target_section   | url                              |
|------------------|----------------------------------|
| datacamp.com     |https//www.datacamp.com/courses   |


## REPLACE

> SELECT
> TOP(5) REPLACE(url, '_' , '-') AS replace_with_hyphen
> FROM courses;

*In out case, replace _ with -*


| replace_with_hyphen                 |
|-------------------------------------|
| datacamp.com/courses/introduction-  |
| datacamp.com/courses/intermediate-  |
| datacamp.com/courses/writing-       |
| datacamp.com/courses/joining-       |
| datacamp.com/courses/intro-         |

