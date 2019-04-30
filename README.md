# SQLExercises
Sql Exercises
1 - Revising the select query
Query all columns for all American cities in CITY with populations larger than 100000. The CountryCode for America is USA.
select * from city where population > 100000 and countrycode = 'USA';

2 - Revising the select query II
Query the names of all American cities in CITY with populations larger than 120000. The CountryCode for America is USA. 
select name from city where population > 120000 and countrycode = 'USA';

3 - Select all
select * from city;

4- Select by ID
Query all columns for a city in CITY with the ID 1661
select * from city where id = 1661;

5- Japanese cities attribute
Query all attributes of every Japanese city in the CITY table. The COUNTRYCODE for Japan is JPN.
select * from city where countrycode = 'JPN';

6-Query the names of all the Japanese cities in the CITY table. The COUNTRYCODE for Japan is JPN.
select name from city where countrycode = 'JPN';

7- Query a list of CITY and STATE from the STATION table.
select city,state from station;

8 - Query a list of CITY names from STATION with even ID numbers only. You may print the results in any order, but must exclude duplicates from your answer.
select distinct city from station where id%2=0;

*******
9 - Let N be the number of CITY entries in STATION, and let N' be the number of distinct CITY names in STATION; query the value of N - N' from STATION. In other words, 
find the difference between the total number of CITY entries in the table and the number of distinct CITY entries in the table.
Select count(CITY) - count(distinct CITY) from STATION;

10-Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths (i.e.: number of characters in the name). 
If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.

select top 1 city  + ' ' +  cast(len(city) as varchar) from station where len(city) = (select min(len(city)) from station) order by city;
select top 1 city  + ' ' +  cast(len(city) as varchar) from station where len(city) = (select max(len(city)) from station) order by city;

11- Query the list of CITY names starting with vowels (i.e., a, e, i, o, or u) from STATION. Your result cannot contain duplicates.
select distinct city from station where left(city,1) in ('a','e','i','o','u');

12- Query the list of CITY names ending with vowels (a, e, i, o, u) from STATION. Your result cannot contain duplicates.
select distinct city from station where right(city,1) in ('a','e','i','o','u');

13- Query the list of CITY names from STATION which have vowels (i.e., a, e, i, o, and u) as both their first and last characters. Your result cannot contain duplicates.
select distinct city from station where left(city,1) in ('a','e','i','o','u') and right(city,1) in ('a','e','i','o','u');

14- Query the list of CITY names from STATION that do not start with vowels. Your result cannot contain duplicates.
select distinct city from station where left(city,1) not in ('a','e','i','o','u');

15
16
17-
18-Query the Name of any student in STUDENTS who scored higher than 76 Marks. Order your output by the last three characters of each name. 
If two or more students both have names ending in the same last three characters 
(i.e.: Bobby, Robby, etc.), secondary sort them by ascending ID.
select name from students where marks>75 order by right(name,3), id;

19-Write a query that prints a list of employee names (i.e.: the name attribute) from the Employee table in alphabetical order.
select name from employee order by name;

20 - Write a query that prints a list of employee names (i.e.: the name attribute) for employees in Employee having a salary greater than $2000 
per month who have been employees for less than 10 months. Sort your result by ascending employee_id.
select name from employee where salary > 2000 and months<10 order by employee_id;

21-Write a query identifying the type of each record in the TRIANGLES table using its three side lengths. Output one of the following statements for each record in the table:
Equilateral: It's a triangle with sides of equal length.
Isosceles: It's a triangle with sides of equal length.
Scalene: It's a triangle with sides of differing lengths.
Not A Triangle: The given values of A, B, and C don't form a triangle.

SELECT CASE WHEN A + B > C AND A+C>B AND B+C>A THEN CASE WHEN A = B AND B = C THEN 'Equilateral' WHEN A = B OR B = C OR A = C THEN 'Isosceles' 
WHEN A != B OR B != C OR A != C THEN 'Scalene' END ELSE 'Not A Triangle' END FROM TRIANGLES;

22-The PADS
Generate the following two result sets:
Query an alphabetically ordered list of all names in OCCUPATIONS, immediately followed by the first letter of each profession as a parenthetical (i.e.: 
enclosed in parentheses). For example: AnActorName(A), ADoctorName(D), AProfessorName(P), and ASingerName(S). 
Query the number of ocurrences of each occupation in OCCUPATIONS. Sort the occurrences in ascending order, and output them in the following format: 
There are a total of [occupation_count] [occupation]s.
where [occupation_count] is the number of occurrences of an occupation in OCCUPATIONS and [occupation] is the lowercase occupation name. 
If more than one Occupation has the same [occupation_count], they should be ordered alphabetically.

select name+'('+left(occupation,1)+')' from occupations order by name;
select 'There are a total of ' + ltrim(str(count(occupation))) + ' ' + lower(occupation) + 's.' from occupations group by occupation order by count(occupation), occupation;

23- OCCUPATIONS
Pivot the Occupation column in OCCUPATIONS so that each Name is sorted alphabetically and displayed underneath its corresponding Occupation. The output column headers should be Doctor, Professor, Singer, and Actor, respectively.
Note: Print NULL when there are no more names corresponding to an occupation.

select d.name as doctor, p.name as professor, s.name as singer, a.name as actor from 
(select name, Row_number() over(order by name) as num from occupations where occupation = 'Doctor') d full join
(select name, Row_number() over(order by name) as num from occupations where occupation = 'Professor') p on d.num = p.num full join
(select name, Row_number() over(order by name) as num from occupations where occupation = 'Singer') s on s.num = p.num or s.num = d.num full join 
(select name, Row_number() over(order by name) as num from occupations where occupation = 'Actor') a on p.num = a.num or d.num = a.num or a.num = s.num;

-- first solution
/*SELECT d.Name as doctor, p.Name as professor, s.Name as singer, a.Name as actor FROM 
(SELECT Name, Row_number() over(order by name) as num FROM Occupations WHERE Occupation = 'Doctor') d full join 
(SELECT Name, Row_number() over(order by name) as num FROM Occupations WHERE Occupation = 'Professor') p on d.num = p.num full join 
(SELECT Name, Row_number() over(order by name) as num FROM Occupations WHERE Occupation = 'Singer') s on d.num = s.num or p.num = s.num full join 
(SELECT Name, Row_number() over(order by name) as num FROM Occupations WHERE Occupation = 'Actor') a on d.num = a.num or p.num = a.num or s.num = a.num*/

-- Second Solution
select
    Doctor
    ,Professor
    ,Singer
    ,Actor
from (
select
    NameOrder
    ,max(case Occupation when 'Doctor' then Name end) as Doctor
    ,max(case Occupation when 'Professor' then Name end) as Professor
    ,max(case Occupation when 'Singer' then Name end) as Singer
    ,max(case Occupation when 'Actor' then Name end) as Actor
from (
    select
        Occupation
        ,Name
        ,row_number() over(partition by Occupation order by Name ASC) as NameOrder
    from Occupations
    ) as NameLists
    group by NameOrder
) as Names


24- Binary Tree Nodes
You are given a table, BST, containing two columns: N and P, where N represents the value of a node in Binary Tree, and P is the parent of N.
Write a query to find the node type of Binary Tree ordered by the value of the node. Output one of the following for each node:
Root: If node is root node.
Leaf: If node is leaf node.
Inner: If node is neither root nor leaf node.

SELECT N , CASE WHEN P IS NULL THEN 'Root' ELSE CASE WHEN EXISTS (SELECT P FROM BST B WHERE A.N=B.P) THEN 'Inner' ELSE 'Leaf' END END FROM BST A ORDER BY N

25- New Companies
 select  
        C.company_code,
        min(founder) as founder,
        count(distinct lead_manager_Code),
        count(distinct senior_manager_Code),
        count(distinct Manager_Code),
        count(distinct Employee_Code)
        
    from Company C
        join Employee E on E.company_Code = C.company_Code

    group by C.company_code
    
    order by C.company_code 	
	
26 - Revising Aggregation The count function
select count(id) from city where population>100000;

27 - Revising Aggregation The sum function
select sum(population) from city where district='California';

28 - Revising Aggregation - Average
select avg(population) from city where district = 'California';

29 - Average Population
select round(avg(population),0) from city;

30 - Japan population
select sum(population) from city where countrycode = 'JPN';

31 - Population density difference
select max(population) - min(population) from city;

32 - The blunder
select cast(ceiling(Avg(cast(Salary as float))- avg(cast(replace(cast(salary as VarChar(10)),'0','') as float))) as int) from employees;

33 - Top Earners
We define an employee's total earnings to be their monthly salary*months worked, and the maximum total earnings to be the maximum total earnings for any employee 
in the Employee table. Write a query to find the maximum total earnings for all employees as well as the total number of employees who have maximum total earnings.
Then print these values as space-separated integers.	
	
with t as (
SELECT months * salary as total
FROM Employee )
SELECT top 1 total, count(*) as cnt 
          FROM t
          GROUP BY total
          order by total desc;

34 - Weather Observation Station 2
SELECT
 CAST(SUM(LAT_N) AS DECIMAL(7,2)),
 CAST(SUM(LONG_W) AS DECIMAL(7,2))
FROM STATION

35 - Weather Observation Station 13
select cast(sum(lat_n) as decimal(10,4)) from station where lat_n > 38.7880 and lat_n < 137.2345;

36 - Weather Observation Station 14
select cast(max(lat_n) as decimal(10,4)) from station where lat_n < 137.2345;

37 - Weather Observation Station 15
select cast (long_w as decimal(10,4)) from station where lat_n = (select max(lat_n) from station where lat_n < 137.2345);

38 - Weather Observation Station 16
select cast(min(lat_n) as decimal(10,4)) from station where lat_n > 38.7780;

39 - Weather Observation Station 17
SELECT CAST(LONG_W AS NUMERIC(20,4) ) FROM STATION WHERE LAT_N = (SELECT MIN(LAT_N) FROM STATION WHERE LAT_N > 38.7780);

40-Asian Population
select sum(c.population) from city c join country co on c.countrycode = co.code where continent = 'Asia';
	
41 - African cities
select c.name from city c join country co on c.countrycode = co.code where continent = 'Africa';
	
42 - Average population of each continent
select continent, cast(floor(avg(c.population)) as int) from city c join country co on c.countrycode = co.code group by continent; 

43 -  Draw Triangle
DECLARE @i INT = 20
WHILE (@i > 0) 
BEGIN
   PRINT REPLICATE('* ', @i) 
   SET @i = @i - 1
END

44 - Draw Triangl2 2
declare @row int = 1
while (@row < 21)
begin
    print replicate('* ', @row)
    set @row = @row + 1
end	
	
43 - Print Prime Numbers
Write a query to print all prime numbers less than or equal to 1000. Print your result on a single line, and use the ampersand (&) character as your separator (instead of a space).
For example, the output for all prime numbers < 10 would be: 2&3&5&7
	
SET QUOTED_IDENTIFIER ON;
WITH Nums AS (
SELECT N
FROM (VALUES
(0),(1),(2),(3),(4),(5),(6),(7),(8),(9)
) x(N)
),
AllNums(N) AS (
SELECT N1.N + (N2.N * 10) + (N3.N * 100)
FROM Nums N1
CROSS JOIN Nums N2
CROSS JOIN Nums N3
),
Results AS (
SELECT a.N
FROM AllNums a
WHERE a.N BETWEEN 2 AND 1000
  AND NOT EXISTS(SELECT * FROM AllNums b WHERE b.N > 1 AND b.N < a.N AND a.N % b.N = 0)
)
SELECT STUFF((
SELECT '&' + CAST(r.N AS VARCHAR(10)) AS "text()"
FROM Results r
ORDER BY r.N
FOR XML PATH(''),TYPE).value('./text()[1]','VARCHAR(8000)'),1,1,'');

? - Weather Observation Station 19
select round(sqrt(power(max(long_w) - min(long_w), 2) + power(max(lat_n) - min(lat_n),2)), 4) from station;

? - Weather Observation Station 18
SELECT round(ABS(MIN(LAT_N)-MAX(LAT_N))+ABS(MIN(LONG_W)-MAX(LONG_W)),4) FROM STATION;

? - Weather Station 20 - Find the Median
select cast((
 (select max(lat_n) from (select top 50 percent lat_n from station order by lat_n asc) as bh) +
 (select min(lat_n) from (select top 50 percent lat_n from station order by lat_n desc) as th)
) / 2 as numeric(10,4))

? - The report
SELECT 
    CASE WHEN g.Grade >= 8 THEN s.Name ELSE NULL END 'Name',
    g.Grade,
    s.Marks
FROM 
    Students s
    INNER JOIN Grades g
        ON s.Marks BETWEEN g.Min_Mark AND g.Max_Mark
ORDER BY 
    g.Grade DESC, CASE WHEN g.Grade >= 8 THEN s.Name ELSE CAST(s.Marks as VARCHAR(100)) END
