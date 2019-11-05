## Introduction to SQL for data science
Distinct selects unique values from a column 'language' from table 'films'.
```
SELECT DISTINCT language
FROM films;
```
To count the number of rows in a table:
```
SELECT COUNT(*)
FROM people;

SELECT COUNT(DISTINCT language)
FROM films;
```
To filter results:
```
SELECT *
FROM films
WHERE budget > 10000;

SELECT title
FROM films
WHERE country = 'China'
```

Use the AND OR command to add conditions:
```
SELECT title
FROM films
WHERE release_year > 1994 AND < 2000;

SELECT title
FROM films
WHERE release_year > 1994
AND release_year < 2000;

SELECT title
FROM films
WHERE (release_year = 1994 OR release_year = 1995)
AND (certification = 'PG' OR certification = 'R');
```
Use BETWEEN to select a range:
```
SELECT title
FROM films
WHERE release_year
BETWEEN 1994 AND 2000;
```

We have the IN operator to select from a list of values:
```
SELECT name
FROM kids
WHERE age IN (2, 4, 6, 8, 10);
```
