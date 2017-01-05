---
layout: post
title: "Most of SQL on one page"
comments: false
description: ""
keywords: ""
---

To be honest, I don't write sql that often. Every time I do, it's been so long that I've totally forgetten the syntax and need to relearn it. As a result, I've started just writing down the basics as I relearn them to make it easier next time I've forgotten everything. It's actually been pretty helpful to have for me, so maybe it will be helpful to you next time you have to SQL your heart out. Actually sql syntax feels more elegant than I remember it.

* Querying
  * Getting Data
  * Grouping
  * Aggs
  * Sub Queries
  * Joins
  * Aliases
* Modifying Data
  * insert
  * update
  * delete
* Managing DB's and Tables
  * CREATE/DELETE DB
  * Create/Delete Table
  * primary key
  * foreign key
  * constraints

## Getting Data

SELECT (columns)
FROM (table)
WHERE (conditions)
ORDER BY (column) (DESC);

### Select
put * for everything, or choose specific column names

### From
put the name of the table

### where
use boolean logic. Say what values are acceptable for a column
AND
OR
=, >, <, >=, <=, <>
IN (list)

## Modifying a Table

### Adding Data

INSERT INTO (table) (columns)
VALUES (values);

### Changing Data

UPDATE (table)
SET (column) = (value)
WHERE (conditional);

### Deleting Data
DELETE FROM (table)
WHERE (conditional)

its worth noting:
DELETE FROM (table)
will totally scrap the entire table, so try not to do that by accident if you liked having data.

## Creating databases and tables

### Create/Delete a Database

CREATE DATABASE (database name)
DROP DATABASE (database name)

### Create/Delete a Table

CREATE TABLE (table)
(
  (column_name) (column_type),
  (column_name) (column_type),
  ...
);

DROP TABLE (table)


CREATE TABLE advertisements
(
  id int,
  type varchar(20),
  distribution varchar(10),
  amount int
);

## Agg Functions
These guys do what they sound like.
- `count`
- `sum`
- `avg`
- `min`
- `max`

Just wrap it around your return column. For example

    SELECT avg(salary)
    FROM Employees;

## Grouping
We can create groups based on values of columns. We can then compute aggregated based on these groups!

    SELECT (cols + ags you want)
    FROM (table)
    WHERE (filter conditions)
    GROUP BY (cols)
    HAVING (this is like a where statement but for the groups)
    
You can think of this executing like
1. FROM -  choose the table
2. WHERE - filter for rows that meet our conditions
3. GROUP BY -  create out grouping
4. HAVING - filter groups out by agg conditions
5. SELECT - give back what we want.

## Managing Tables

### Primary key
- Can only have one per table
- Must be unique and not null
PRIMARY KEY

### Table constraints
- Allow us to make a constraint that involves several columns
- Can seperate making constraints from

- NOT NULL
- UNIQUE
- CHECK(conditional)

    CREATE TABLE Sandwiches
    (
      id int PRIMARY KEY,
      name varchar(40) NOT NULL UNIQUE,
      description varchar(50),
      price int,
      CONSTRAINT price_needed NOT NULL price,
    );

### Foreign Key
Reference a key from another table

    CREATE TABLE Combos
    (
      id int PRIMARY KEY,
      name varchar(40) NOT NULL UNIQUE,
      price float CHECK(price > 2.0) /* we don't want our combos to get too cheap */
      sandwich_id int REFERENCES Sandwiches(id), /* one way to specify a foreign key */
      drink_id int,
      FOREIGN KEY (drink_id) REFERENCES Drinks(id) /* Another way to set a foreign key */
    )

### Orphan entries
We can't reference an entry that no longer exists with our foreign key, so the database will prevent us from making certain deletions and force us to delete our would be orphans before we delete the rows they depend on.

## Foreign Keys
CREATE TABLE Actors (
  id int PRIMARY KEY,
  name varchar(50) NOT NULL UNIQUE,
  country_id int REFERENCES Countries(id)
  FOREIGN KEY (country_id) REFERENCES Country
);

## Joins!

    /* REWRITE THIS */
    SELECT Movies.title, Movies.id, Rooms.seats 
    From Movies
    INNER JOIN Rooms
    ON Movies.id=Rooms.movie_id
    WHERE seats > 75
    ORDER BY Rooms.seats DESC;

Many to many /* REWRITE THIS */    
SELECT Actors.name, Movies.title
FROM Actors
INNER JOIN Actors_Movies
ON Actors.id=Actors_Movies.actor_id
INNER JOIN Movies
ON Actors_Movies.movie_id=Movies.id
ORDER BY title;

## Aliases
Put the alias you want next to the data, we can use the alias for the rest of the query.
### Column alias
column_name `AS` alias_we_want -> We don't even need the as, we can just put the alias after the column name. If we want spaces in the alias we can use quotes.
### table alias
We can do the same thing as column aliases

    SELECT m.title
    FROM Movies m
    INNER JOIN Movies_Genres mg
    on m.id = mg.movie_id
    INNER JOIN Genres g
    on mg.genre_id = g.id
    
Sandwiches.name `AS` Sandwich

### Outer Join
LEFT OUTER JOIN (other_table)
ON (my_id)=(other_table.id)

## Subqueries
Use one query inside of another query!
runs the inner query first, returns the result, then uses it in the outer query.
JOIN Queries can have better performance

WHERE condition IN
(other query in parens)

## Resources
[3 battle tested ways to install postgresql](https://www.codefellows.org/blog/three-battle-tested-ways-to-install-postgresql/)
[10 SQL Articles everyone must read](https://blog.jooq.org/2015/02/13/10-sql-articles-everyone-must-read/)
[SQL Like Condition](https://www.techonthenet.com/sql/like.php)