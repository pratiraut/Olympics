# Olympics Analysis in SQL

## Introduction
I came across this dataset Kaggle website which has a historical dataset on the modern Olympic Games, including all the Games from Athens 1896 to Rio 2016. Using to find out the given problem statement.

### SQL Concept Applied
- Joins.
- Aggregate Function.
- Windows Function.
- Common Table Expression(CTE).

## Problem Statement
- Total no of sports played in each Olympic games.
- Total no of nations who participated in each Olympic game?
- Which nation has participated in all of the Olympic games?
- Which Sports were played only once in the Olympics?


### Exploring a database by identifying the tables and the foreign keys that link them.

```sql
SELECT *
FROM olympics_history;
```

```sql
SELECT *
FROM olympics_history_noc_region;
```

How many Olympic games have been held?

```sql
SELECT COUNT(DISTINCT games)
FROM olympics_history;
```

All the Olympic games are held in which city.

```sql
SELECT DISTINCT year, season, city
FROM olympics_history 
ORDER BY year;
```

### Entity Relationship Diagram (ERD)

An Entity Relationship Diagram (ERD) is a snapshot of data structures. An Entity Relationship Diagram shows entities (tables) in a database and relationships between tables within that database. In other words, How olympics_history is joined to olympics_history_noc_region.

1. Total no of sports played in each Olympic games.

```sql
 WITH t1 AS
      	(SELECT DISTINCT games, sport
      	FROM olympics_history),
        t2 AS
      	(SELECT games, COUNT(1) AS no_of_sports
      	FROM t1
      	GROUP BY games)
  SELECT * FROM t2
  ORDER BY no_of_sports DESC;
```

2. Total no of nations who participated in each Olympic game?

```sql
WITH all_countries AS
       (SELECT games, nr.region
        FROM olympics_history oh
        JOIN olympics_history_noc_regions nr 
		 ON nr.noc = oh.noc
        GROUP BY games, nr.region)
    SELECT games, COUNT(1) AS total_countries
    FROM all_countries
    GROUP BY games
    ORDER BY games;
```

3. Which nation has participated in all of the Olympic games?

```sql
with tot_games as
              (select count(distinct games) as total_games
              from olympics_history),
          countries as
              (select games, nr.region as country
              from olympics_history oh
              join olympics_history_noc_regions nr ON nr.noc=oh.noc
              group by games, nr.region),
          countries_participated as
              (select country, count(1) as total_participated_games
              from countries
              group by country)
      select cp.*
      from countries_participated cp
      join tot_games tg on tg.total_games = cp.total_participated_games
      order by 1;
```

4. Which Sports were played only once in the Olympics?

```sql
WITH t1 AS
          	(SELECT DISTINCT games, sport
          	FROM olympics_history),
          t2 AS
          	(SELECT sport, COUNT(1) AS no_of_games
          	FROM t1
          	GROUP BY sport)
      SELECT t2.*, t1.games
      FROM t2
      JOIN t1 
	  ON t1.sport = t2.sport
      WHERE t2.no_of_games = 1
      ORDER BY t1.sport;
```
