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

IS NULL or IS NOT NULL to check empty values:
```
SELECT COUNT(*)
FROM people
WHERE birthdate IS NULL;

SELECT name
FROM people
WHERE birthdate IS NOT NULL;
```

The % wildcard will match zero, one, or many characters in text. For example, the following query matches companies like 'Data', 'DataC' 'DataCamp', 'DataMind', and so on:
```
SELECT name
FROM companies
WHERE name LIKE 'Data%';
```
The _ wildcard will match a single character. For example, the following query matches companies like 'DataCamp', 'DataComp', and so on:
```
SELECT name
FROM companies
WHERE name LIKE 'DataC_mp';
```
You can also use the NOT LIKE operator to find records that don't match the pattern you specify.

We can perform calculations with aggregate functions such as SUM, MAX, MIN, AVG.
```
SELECT AVG(budget)
FROM films;

SELECT MAX(budget)
FROM films;

SELECT SUM(budget)
FROM films;
```

Aliases make the table more readable, preventing same names:
```
SELECT MAX(budget) AS max_budget,
       MAX(duration) AS max_duration
FROM films;
```

When performing division, if all numbers are integers, the result will be an integer. If not, add a float number in the equation.
```SELECT 45 * 100.0 / 10;```

To arrange in order, it is possible to order by two columns:
```
SELECT title
FROM films
ORDER BY release_year DESC;

SELECT birthdate, name
FROM people
ORDER BY birthdate, name;
```

Use GROUP BY to group data. Commonly, GROUP BY is used with aggregate functions like COUNT() or MAX(). Note that GROUP BY always goes after the FROM clause.
```
SELECT sex, count(*)
FROM employees
GROUP BY sex;
```
