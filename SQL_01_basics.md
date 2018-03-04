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

## Understanding the database

(MySQl)
+ `SHOW tables;`: gives you all table names in a database
+ `DESCRIBE tablename;`: shows table info


<br>

## Select clauses
`SELECT *`: Select everything.  
`SELECT DISTINCT var1`: Return the distinctive values of var1
`SELECT ROUND(var1, -3)`: Round the var1 to the nearest 1000 unit  
`SELECT var1/1000`: Immediate calculations inside select
`SELECT CONCAT(var1, '%')`: Concatenating text together  
`SELECT COALESCE(var, alternative)`: Takes the first value that is not NULL. If var is not NULL, var will be returned, otherwise the alternative. Function takes as many arguments as needed.


Aggregates:  
`SELECT SUM(var)`: returns sum  
`SELECT AVG(var)`: returns average  
`SELECT MAX(var)`: returns maximum, similarly there is `MIN`   
`SELECT COUNT(var)`: Return the number of values
`SELECT LENGTH(var)`: number of characters in a variable  

Alias-ing:  
`SELECT var1/var2 AS NewColName`   

Making new discretes:  
Making discrete values based on condition through CASE WHEN:
```SQL
SELECT var 1,
       CASE WHEN condition1 THEN 'Value1'
            WHEN condition2 THEN 'Value2'
            ELSE 'Value3'
       END AS varname
```


Limiting:  
`SELECT TOP 10 *`: selects the first   
Alternative in some SQL dialects: `SELECT * FROM tb LIMIT 10;`  

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


## Grouping/Having
Adding `GROUP BY(var)` causes other functions as SUM and COUNT to applied over the groups. It will return a row per group.  

`HAVING` will filter the groups displayed.
Difference versus WHERE: A `WHERE` clause filters rows before the aggregation, the `HAVING` clause filters after the aggregation.  



<br>

## Ordering results
Addding `ORDER BY var DESC` clause to show the result ordered in descending order.

The `var IN ('option1', 'option2')` clause returns 0-1 so it can be used for other purposes, for instance to arrange some subjects as last.  
Sample code will show nobel price winners in 1984 with Physisc and Chemistry last.

```SQL
SELECT winner, subject
FROM nobel
WHERE yr=1984
ORDER BY subject IN ('Physics','Chemistry'), subject, winner;
```






## Joins

**Additive joins**
+ `INNER JOIN` :  return rows that have a match in both tables  
+ `FULL JOIN` : return all rows combined (regardless of whether it has a match)  
+ `LEFT JOIN` : will return first table with added info from second  
+ `RIGHT JOIN` : will return second table with added info from the first
+ `CROSS JOIN` : all possible combinations of 1 and 2
+ `SELF JOIN`

General syntax:
```SQL
SELECT *
FROM table1
JOIN table2 ON table1.id = table2.id;
```

If the id column is the same in both tables it can be shorter: `JOIN table2 USING id`.
Sometimes useful to add aliases after the tables.

**Filtering joins**
There is no SQL function for anti join or semi join.  
If needed, need to do via subquery:

```SQL
SELECT *
FROM table1
WHERE var IN (SELECT var FROM table2 WHERE ____);
```


<br>

## Set Theory

Where joins will add columns to your data, with set theory you are adding rows.

+ `UNION`: every record that is in both tables
+ `UNION ALL`: every record in both + duplicates for the intersect
+ `INTERSECT`: all records that appear in both
+ `EXCEPT`: all records that only appear in one table

Syntax:

```SQL
SELECT var1, var2 FROM table1
UNION
SELECT var1, var2 FROM table2;
```



<br><hr>


## Acknowledgements

+ [SQL Zoo](http://sqlzoo.net)
