# SQL Basics

Basic syntax of SQL queries:

```SQL
SELECT var1, var2, var3
FROM table
WHERE var1 = value1;
```
<br>

## Select clauses
`SELECT *`: Select everything.  
`SELECT COUNT(*)`: Return the number of values  
`SELECT DISTINCT var1`: Return the distinctive values  
`SELECT ROUND(var1, -3`: Round the var1 to the nearest 1000 unit  
`SELECT var1/1000`: You can do immediate calculations inside SELECT  


<br>






## Where clauses

`WHERE var1 = value1`  Comparison versus one value with =, <, <=, >, >= , <>  
`WHERE var1 BETWEEN value1 and value2`  

`WHERE var1 LIKE 'A%'`  All starting with A  
`WHERE LEFT(var1, 1) = LEFT(var2, 1)` All where the first letter of var1 and var2 are identical

`WHERE var1 IN ('value1', 'value2') Will show all values that are either value1 or value 2  



WHERE stategements
WHERE var AND, OR, XOR (exclusive OR)
WHERE var BETWEEN value1 AND value2;


`WHERE length(var1) > 5`: Value length larger than 5.


All details of Literature Nobel price winners in the eighties:
```SQL
SELECT *
FROM nobel
WHERE subject = 'Literature' 
AND yr BETWEEN 1980 AND 1989;
```



All details of the following subjects in 19970
```SQL
SELECT * FROM nobel
WHERE yr = 1970
AND subject IN ('Cookery', 'Chemistry', 'Literature');
```



Search for all Nobel price winners called John.
```SQL
SELECT winner
FROM nobel
WHERE winner LIKE 'John %';
```



Select all info from winners in 1980 excluding Chemistry or Medicine
```SQL
SELECT *
FROM nobel
WHERE yr = 1980 AND subject NOT IN ('Chemistry', 'Medicine');
```


Find the details about Eugene O'Neill.
If you need an apostrophe inside a search term, use two single quotes within the quoted string.
```SQL
SELECT *
FROM nobel
WHERE winner = 'Eugene O''Neill';
```


## Changing the appearance

Add an **ORDER BY var DESC** clause to show the result ordered in descending order.

The <var IN ('option1', 'option2')> clause returns 0-1 so it can be used for other purposes, for instance to arrange some subjects as last.
Sample code will show nobel price winners in 1984 with Physisc and Chemistry last.
```SQL
SELECT winner, subject
FROM nobel
WHERE yr=1984
ORDER BY subject IN ('Physics','Chemistry'), subject, winner;
```



