# SQL Basics

Basic syntax of SQL queries:

```SQL
SELECT var1, var2, var3
FROM table
WHERE var1 = value1;
```

Basic nested SQL syntax:
```SQL
SELECT var1, var2, var3
FROM table
WHERE var1 < (SELECT var1 FROM table WHERE var2 = value2);
```


<br>

## Select clauses
`SELECT *`: Select everything.  
`SELECT DISTINCT var1`: Return the distinctive values  
`SELECT ROUND(var1, -3)`: Round the var1 to the nearest 1000 unit  
`SELECT var1/1000`: You can do immediate calculations inside SELECT  
`SELECT CONCAT(var1, '%')`: Concatenating text together  

Aggregates:
`SELECT SUM(var)`: returns sum  
`SELECT AVG(var)`: returns average  
`SELECT MAX(var)`: returns maximum, similarly there is `MIN`   
`SELECT COUNT(var)`: Return the number of values    

Renaming
`SELECT var1/var2 AS var3`  

<br>

## Where clauses

`WHERE var1 = value1`:  Comparison versus one value with =, <, <=, >, >= , <>  
`WHERE var1 BETWEEN value1 and value2`  
`WHERE var1 LIKE 'A%'`:  All starting with A  
`WHERE var1 IN ('value1', 'value2')`: Will show all values that are either value1 or value 2 
`WHERE var1 NOT IN ('value1', 'value2')`: Will show all values that are neither value1 nor value 2  
`WHERE LEFT(var1, 1) = LEFT(var2, 1)`: All where the first letter of var1 and var2 are identical  

Conditions:
AND, OR, XOR (exclusive or)

<br>


## Grouping
Adding `GROUP BY(var)` causes other functions as SUM and COUNT to applied over the groups. It will return a row per group.
A `HAVING` will filter the groups displayed. WHERE clause filters rows before the aggregation, the HAVING clause filters after the aggregation. 



<br>

## Ordering results
Addding **ORDER BY var DESC** clause to show the result ordered in descending order.

The <var IN ('option1', 'option2')> clause returns 0-1 so it can be used for other purposes, for instance to arrange some subjects as last.
Sample code will show nobel price winners in 1984 with Physisc and Chemistry last.
```SQL
SELECT winner, subject
FROM nobel
WHERE yr=1984
ORDER BY subject IN ('Physics','Chemistry'), subject, winner;
```


## Other functions
`LENGTH(var)`: number of characters in a variable





## Examples:
All details of Literature Nobel price winners in the eighties:
```SQL
SELECT *
FROM nobel
WHERE subject = 'Literature' 
AND yr BETWEEN 1980 AND 1989;
```

Search for all Nobel price winners called John.
```SQL
SELECT winner
FROM nobel
WHERE winner LIKE 'John %';
```

Find the details about Eugene O'Neill.
If you need an apostrophe inside a search term, use two single quotes within the quoted string.
```SQL
SELECT *
FROM nobel
WHERE winner = 'Eugene O''Neill';
```

Select countries with a bigger population than the largest one in Europe
```SQL
SELECT name
FROM world
WHERE population >= ALL(SELECT population FROM world
                        WHERE continent = 'Europe' AND population>0);
```


Find the largest country (by area) in each continent, show the continent, the name and the area: 
```SQL
SELECT continent, name, area 
FROM world AS x
WHERE area >= ALL (SELECT area FROM world AS y
                   WHERE y.continent=x.continent AND area>0);
```

Select the code that shows the countries belonging to regions with all populations over 50000 
```SQL
SELECT name, region, population 
FROM world x 
WHERE 50000 < ALL (SELECT population FROM world y 
                   WHERE x.region=y.region AND y.population>0);
```

Give the number of countries per continent with a population greater than 10000000
```SQL
SELECT continent, COUNT(name)
FROM world
WHERE population >= 10000000
GROUP BY continent;
```
List the continents that have a total population of at least 100 million. 
```SQL
SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000;
```


## Acknowledgements

+ [SQL Zoo](http://sqlzoo.net)
