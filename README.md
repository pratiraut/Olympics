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

Total no of sports played in each Olympic games.
