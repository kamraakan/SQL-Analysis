# SQL Interview Questions prep:
https://data36.com/sql-interview-questions-tech-screening-data-analysts/

1) Let’s say you have two SQL tables: authors and books.
The authors dataset has 1M+ rows; here’s the first six rows:

author_name	book_name
author_1	book_1
author_1	book_2
author_2	book_3
author_2	book_4
author_2	book_5
author_3	book_6
…	…
The books dataset also has 1M+ rows and here’s the first six:

book_name	sold_copies
book_1	1000
book_2	1500
book_3	34000
book_4	29000
book_5	40000
book_6	4400
…	…
1) **Create an SQL query that shows the TOP 3 authors who sold the most books in total!**
```sql
select autho_name, sum(sold_copies) as sold_sum
from authors
join books 
on authors.book_name= books.book_name
group by author_name
order by sold_sum desc
limit 3;
```
**SQL Interview Question #2**
You work for a startup that makes an online presentation software. You have an event log that records every time a user inserted an image into a presentation. (One user can insert multiple images.) The event_log SQL table looks like this:

user_id	event_date_time
7494212	1535308430
7494212	1535308433
1475185	1535308444
6946725	1535308475
6946725	1535308476
6946725	1535308477
…	…
…and it has over one billion rows.
Note: If the event_date_time column’s format doesn’t look familiar, google “epoch timestamp”!

2) Write an SQL query to find out how many users inserted more than 1000 but less than 2000 images in their presentations!
```sql
SELECT COUNT(*) FROM
  (SELECT user_id, COUNT(event_date_time) AS image_per_user
  FROM event_log
  GROUP BY user_id) AS image_per_user
WHERE image_per_user < 2000 AND image_per_user > 1000;
```

3) calaculate partioned average temp for each zip code

```sql
select date, zipcode, max temp, avg(temp) over (partition by Zip_code) as zip_avg
from whether
```

4) The runs_2016 table contains the team_name and the number of runs scored during the 2016 season. Calculate the number of sixes and fours that were scored by each team.

```sql
select team_name, 
sum (case when runs_scored = 4 then 1
else 0 end) as fours,

case when run_scored = 6 then 1
else 0 end ) as sixes

from runs_2016

group by team_name
order by team_name;
```

### 5) The food table contains nutritional information about items on sale in a restaurant. The number of calories per 100g is given in the energy column. You want to compare the calories in the 5 items with the largest number of calories, to the average number of calories in all items on offer. Create a new column avg_calories with the average over all of the data.

Food
id int
item text
energy int
protein numeric

```sql 
select item, energy as calorie, avg(energy) over() as avg_calorie
from food
order by energy desc
limit 5;
```

6) Return a summary of the table match_dates to show the sum of goals per month, using the columns date and goals.

id  date        goal
1   2019-11-17    3
2   2019-11-18    4

```sql
select sum(goal), date_part('month', date) as month
from match_dates
group by month
order by month;
```

7) Which wines are priced under the average price of all wines in the wine_region table?

```sql
select id 
from wine_region
where price < (select avg(price) from wine_region)
order by id;
```

8) The wine table gives information about wines stocked in an online wine retailer. Find all of the countries that have an average wine price greater than $15.


```sql
select country
from wine
group by country 
having avg(price)> 15
order by country
```

9) The IPL_runs table contains the total runs scored by a player per match. Calculate the total number of runs scored by each player over all matches. Display the TOP 5 scorers.

IPL_runs
match_id    player_id   player_name         runs    
549337            304         F du Plessis    417
549323            304         F du Plessis    410
...
734004             27         Yuvraj Singh    495
734010             27         Yuvraj Singh    449

```sql
select sum(runs), player_name
from ipl_runs
group by player_name
ORDER BY sum(runs) desc
LIMIT 5;

10) The match_captain table lists the names of the captains for each IPL match between 2008 and 2016.
Which players have captained more than 50 IPL matches?

--match_captain
match_id    player_name     
335987        MS Dhoni    
335988        Yuvraj Singh            
335989        SK Warne      
335990        R Dravid 

select player_name, count(match_id)
from match_captain
group by player_name
having count(match_id) > 50
order by player_name;

11) The wine table, whose structure is shown below, contains information about a selection of wines offered by an online retailer. For each wine, return the style, type and price along with the average price for type, e.g for a red wine return the average price of all the red wines.

select style, type, price, round (avg(price) over(partition by type), 2)
from wine
where price is not null
order by price desc;

12) The structure of the employees table is shown. Complete the query to determine which employees are also managers.

employee_id
emp_id
first_name
last_name
manager_id

select first_name
from employee_id
where (employeer_id in (select manager_id from employee_id))
order by last_name;

13) A restaurant that likes to provide customers with recommendations for food to go with their wines has asked you to do some analysis on their data. They want to know which wines don't have a food pairing, and which foods don't have a wine pairing. Join the two datasets so that you can see all styles and items, regardless of whether they have a pairing.


select w.style, p.item
from wine_region w
full join pairing p
on w.id = p.id
order by w.style

14) The purple_cap column contains the id of the player who won the purple cap title.

List the names of the purple_cap players per season. Only records with a match in both tables should be retrieved.


--season
season_id    purple_cap  season_year
    1           102         2008
    2            61         2009
    
    --player
player_id    player_name
1            SC Ganguly
2            BB McCullum
3            RT Ponting


select p.player_name, s.season_year
from player p
join season s
on s.purple_cap = p.player_id
order by s.season_year;

15) Using a subquery as a common expression, return all of the columns from the match table where win_margin is greater than 20 and the match_winner is equal to 1. Limit the results to 10 rows.


with subquery as(select * from match where win_margin >20)

select match_id, win_margin 
from subquery
where match_winner = 1
order by match_id
limit 10;

16) Cast the match_id field to have a data type of 'text'. Limit the result to 10 rows.

select cast(match_id, text) , player_name
   
FROM ipl_runs
ORDER BY player_name, runs
LIMIT 10

17) Extract the names of cities that start with ('A', 'E', 'I', 'O', 'U'). Your results should not contain duplicates.

select city from country

where left(city, 1) in ('A', 'E', 'I', 'O', 'U')
order by city ;

18) How many unique nationalities exists in the artists table?

select count (distinct nationality)
from artists;

19) /****** Script for 2nd highest salary  ******/

select top 1 salary from employees
where salary not in (select max(salary) from employees)
order by salary desc; 

Select Max(Salary) as sal from Employees where Salary < (Select Max(Salary) from Employees);

20 /****** Script for 5th highest salary using CTE ******/



with subquery as (Select distinct top 5 salary from employees order by salary desc)

select top 1 salary
from subquery
order by salary;

Select distinct top 5 salary from employees order by salary desc;

WITH RESULT AS
(
    SELECT SALARY,
           DENSE_RANK() OVER (ORDER BY SALARY DESC) AS DENSERANK
    FROM EMPLOYEES
)
SELECT TOP 1 SALARY
FROM RESULT
WHERE DENSERANK = 5;

21) Delete the duplicate rows

with employee_cte as (select *, row_number() over (partition by order_id order by order_id) as rownumber from employee)

delete from employee_cte where rownumer > 1

22) Transfrom rows into columns

SQL Script to create a table
create table countries 

( country nvarchar (10),
    city navrchar (10) )
    
 GO
 
 insert into countries values( 'usa', newyork)
 insert into countries values ('usa', chicago)
 insert into countries values ('india', mumbai)
 insert into countries values ('india, delhi)
 ```
 
 ``` sql
 select country, city, 'city' + cast(row_number() over (partition by country order by country) as as varchar(10) coulmn sequence) ) 
  from countries
  
  
   Select country, city1, city2
 from
 
 (select country, city, 'city' + cast(row_number() over (partition by country order by country)  as nvarchar(10) ) as coulmn_sequence
  from countries1) Temp

  PIVOT ( 
		 max(city)
		 for coulmn_sequence in (city1, city2)
  ) PIV
  
  
23) FInd employees hired in the last 10 months

select *, datediff(month, HireDate, GETDATE()) as diff
from emp
where datediff(month, HireDate, GETDATE()) between 1 and 10
order by hiredate desc;

24) FInd employees hired in the last 2 years

select * 
from emp
where datediff(year, hiredate, getdate()) between 0 and 2
order by hire-_date

25) fetch data that contains numeric values

select * from emp where isnumeric(value) = 1

26) SQL Query to find department with highest number of employees

with subquery as (select department id, department name, row_number() over(partition by departmentid) as row_numer
                   from employees join department on dptid = d.deptid)
                   
                   select departmentname, department id
                   from subquery 
   
   where departmentid = (select max(deptid) from subquery)
   
   SELECT TOP 1 DepartmentName
FROM Employees
JOIN Departments
ON Employees.DepartmentID = Departments.DepartmentID
GROUP BY DepartmentName
ORDER BY COUNT(*) DESC
27)Write an SQL query that makes recommendations using the  pages that your friends liked. Assume you have two tables: a two-column table of users and their friends, and a two-column table of users and the pages they liked. It should not recommend pages you already like
      friends                 likes
    userid friendid        userid  pageid        user id  page id 
      101    201            101     1              201     1
      201    301            201     2               201   2
      201    101            101     2               201   2
      301    101            201     1               101    1
      401    501            401     3               201    2
                            301     4                101   1
                                                    101 1
                                                    201 4
      
      select  distinct pageid
      from likes l
      where user_id in 
      join likes l on l.userid = u.userid
      where friendid in (select userid from page
      
      
      ## https://code.dennyzhang.com/page-recommendations
select distinct page_id as recommended_page
from Likes
where user_id in (select user2_id
    from Friendship
    where user1_id=1
    union
    select user1_id
    from Friendship
    where user2_id=1)
    and page_id not in
        (select page_id
        from Likes
        where user_id=1)
      
      
      
28) select invoice_id
    from invoice
    where year(invoice_date) = '2016'
    order by inv_amtdesc
    limit 1;
    
29) percentage of male in the emp table

 select sum(case when gender = 'male' then 1 else 0 end) as male_count,
         count(*) as total_count,
		 sum(case when gender = 'male' then 1 else 0 end)/CAST( count(*) as float) as ratio
		 FROM [AK ].[dbo].[Employees];

		 Select  (cast(Count(gender) as float)  / (Select Count(*) From [AK ].[dbo].[Employees])) as ratio
FROM [AK ].[dbo].[Employees]
where gender = 'male' ;
    
30) Write a SQL query for a report that provides the following information for each person in the Person table, regardless if there is an address for each of those people:

select FirstName, LastName, City, State
from person p
left join Address a on
p.personid = a.personid

31) Write a SQL query to get the second highest salary from the Employee table.

select ifnull(max(salary), null) as secondhighestsalary
from employee
where salary not in (select max(Salary) from employee) ; 

32)Write a SQL query to rank scores. If there is a tie between two scores, both should have the same ranking. Note that after a tie, the next ranking number should be the next consecutive integer value. In other words, there should be no "holes" between ranks.

SELECT Score, DENSE_RANK() OVER (ORDER BY Score DESC) as 'rank'
FROM Scores
ORDER BY Score DESC

33) Given the Employee table, write a SQL query that finds out employees who earn more than their managers. For the above table, Joe is the only employee who earns more than his manager.

# Write your MySQL query statement below
select e.name as employee
from employee e
join  employee m
on e.managerid = m.id 
and e.salary > m.salary ;


34) Write a SQL query to find all duplicate emails in a table named Person.

select email
from person
group by email
having count(email) > 1


35)  Customers Who Never Order anything
select name as customers
from customers
where id not in (select customerID from orders);

36) Write a SQL query to find employees who have the highest salary in each of the departments. For the above tables, your SQL query should return the following rows (order of rows does not matter).

select e.name as employee, d.name as department, salary
from employee e
join department d
on e.DepartmentId = d.id
where(  e.departmentid, salary )in (select departmentid, max(salary) over(partition by departmentid) from employee)

SELECT
    Department.name AS 'Department',
    Employee.name AS 'Employee',
    Salary
FROM
    Employee
        JOIN
    Department ON Employee.DepartmentId = Department.Id
WHERE
    (Employee.DepartmentId , Salary) IN
    (   SELECT
            DepartmentId, MAX(Salary)
        FROM
            Employee
        GROUP BY DepartmentId
	)
;

37) Write a SQL query to find employees who earn the top three salaries in each of the department. For the above tables, your SQL query should return the following rows (order of rows does not matter).


select d.name as department, e.name as employee, salary
from employee e
join department d
on e.departmentid = d.id
where 3> (Select count(distinct e2.salary)
           from employee e2
           where e2.salary > e.salary
           and e.departmentid= e2.departmentid
           
           ) ;
           
 38)Please list out all classes which have more than or equal to 5 students.
 
 select  class
from courses
group by class
having count(distinct student) >= 5
```

39) Write a query that shows a company's name, "status" (found in the Companies table), and the number of unique investors in that company. Order by the number of investors from most to fewest. Limit to only companies in the state of New York.

```sql
SELECT companies.name AS company_name,
       companies.status,
       COUNT(DISTINCT investments.investor_name) AS unqiue_investors
  FROM tutorial.crunchbase_companies companies
  LEFT JOIN tutorial.crunchbase_investments investments
    ON companies.permalink = investments.company_permalink
 WHERE companies.state_code = 'NY'
 GROUP BY 1,2
 ORDER BY 3 DESC
 
 
 40) Write a query that lists investors based on the number of companies in which they are invested. Include a row for companies with no investor, and order from most companies to least.
 
 SELECT CASE WHEN investments.investor_name IS NULL THEN 'No Investors'
            ELSE investments.investor_name END AS investor,
       COUNT(DISTINCT companies.permalink) AS companies_invested_in
  FROM tutorial.crunchbase_companies companies
  LEFT JOIN tutorial.crunchbase_investments investments
    ON companies.permalink = investments.company_permalink
 GROUP BY 1
 ORDER BY 2 DESC
 
 41) Write a query that selects all Warrant Arrests from the tutorial.sf_crime_incidents_2014_01 dataset, then wrap it in an outer query that only displays unresolved incidents.
 
  SELECT sub.*
      FROM (
            SELECT *
              FROM tutorial.sf_crime_incidents_2014_01
             WHERE descript = 'WARRANT ARREST'
           ) sub
     WHERE sub.resolution = 'NONE'
     
 42) Write a SQL query to find the cancellation rate of requests made by unbanned users between Oct 1, 2019 and Oct 3, 2019. 
For the above tables, your SQL query should return the following rows with the cancellation rate being rounded to two decimal places


Select request_date as day, round(sum(case when status = ‘completed’ then 0 else 1 end)/count(*),2) 
From trips t
Join users u
On t.client_id = u.user_id and  u.banned =’ no’
Where request_at between ‘10/01/2019’ and ‘10/03/2020’
Group by request_at
Order by request_at;

43)Write a SQL query to find all numbers that appear at least three times consecutively.

SELECT DISTINCT
    l1.Num AS ConsecutiveNums
FROM
    Logs l1,
    Logs l2,
    Logs l3
WHERE
    l1.Id = l2.Id - 1
    AND l2.Id = l3.Id - 1
    AND l1.Num = l2.Num
    AND l2.Num = l3.Num
    
  44) Given a Weather table, write a SQL query to find all dates' Ids with higher temperature compared to its previous (yesterday's) dates.
  
  select w1.id as id
from weather w1,
weather w2
where datediff( w1.RecordDate, w2.RecordDate) = 1 and w1.Temperature > w2.Temperature

45) Write a SQL query to delete all duplicate email entries in a table named Person, keeping only unique emails based on its smallest Id.

Delete P1
from person p1, person p2
where p1.email = P2.email and p1.id >p2.id'
```

46) A country is big if it has an area of bigger than 3 million square km or a population of more than 25 million.

Write a SQL solution to output big countries' name, population and area.

``` sql

select name, population, area
from world
where area > 3000000 or population > 25000000;

47) Please write a SQL query to output movies with an odd numbered ID and a description that is not 'boring'. Order the result by rating.


select *
from cinema
where description != 'boring' and mod(id,2) != 0
order by rating desc;

```

48)Mary is a teacher in a middle school and she has a table seat storing students' names and their corresponding seat ids.
The column id is continuous increment.
 Mary wants to change seats for the adjacent students.
 
Can you write a SQL query to output the result for Mary?
```sql
SELECT (CASE WHEN MOD(id,2)=1 AND id!=(SELECT COUNT(*) FROM seat) THEN id+1
WHEN MOD(id,2)=0 THEN id-1
ELSE id END)id, student
FROM seat
ORDER BY 1
         
49) Given a table salary, such as the one below, that has m=male and f=female values. Swap all f and m values (i.e., change all f values to m and vice versa) with a single update statement and no intermediate temp table.

Note that you must write a single update statement, DO NOT write any select statement for this problem.

# Write your MySQL query statement below
update salary
set sex = case when sex ='m' then 'f' else 'm'  end;

50)
Write an SQL query to reformat the table such that there is a department id column and a revenue column for each month.

The query result format is in the following example:
select *
from (select id, revenue, month, ('month' + '_revenue') as m
from department) as temp


PIVOT (
    SUM(revenue) 
    for m in (
                    [Jan_Revenue],
                    [Feb_Revenue],
                    [Mar_Revenue],
                    [Apr_Revenue],
                    [May_Revenue],
                    [Jun_Revenue],
                    [Jul_Revenue],
                    [Aug_Revenue],
                    [Sep_Revenue],
                    [Oct_Revenue],
                    [Nov_Revenue],
                    [Dec_Revenue]
                 ) as piv
                 
51) Write a query that displays the average number of monthly incidents for each category. Hint: use tutorial.sf_crime_incidents_cleandate to make your life a little easier.

SELECT sub.category,
       AVG(sub.incidents) AS avg_incidents_per_month
  FROM (
        SELECT EXTRACT('month' FROM cleaned_date) AS month,
               category,
               COUNT(1) AS incidents
          FROM tutorial.sf_crime_incidents_cleandate
         GROUP BY 1,2
       ) sub
 GROUP BY 1
 
 52) The following query returns all of the entries from the earliest date in the dataset
 
 SELECT *
  FROM tutorial.sf_crime_incidents_2014_01
 WHERE Date = (SELECT MIN(date)
                 FROM tutorial.sf_crime_incidents_2014_01
              )
              
  53)Write a query that displays all rows from the three categories with the fewest incidents reported.
  
  select incidents.*, sub.total 
  from tutorial.sf_crime_incidents_2014_01 incidents
join (
select category, count(incidnt_num) as total
  FROM tutorial.sf_crime_incidents_2014_01 
  group by category 
  order by total
  limit 3) sub

on incidents.category = sub.category;

54) Union ALL

SELECT investor_name,
       COUNT(*) AS investments
  FROM (
        SELECT *
          FROM tutorial.crunchbase_investments_part1
         
         UNION ALL
        
         SELECT *
           FROM tutorial.crunchbase_investments_part2
       ) sub
 GROUP BY 1
 ORDER BY 2 DESC
 
 
55) Write a query modification of the above example query that shows the duration of each ride as a percentage of the total time accrued by riders from each start_terminal

SELECT start_terminal,
       duration_seconds, start_time,
       SUM(duration_seconds) OVER
         (PARTITION BY start_terminal)
         AS running_total
  FROM tutorial.dc_bikeshare_q1_2012
 WHERE start_time < '2012-01-08'
 
56) Write a query that shows a running total of the duration of bike rides (similar to the last example), but grouped by end_terminal, and with ride duration sorted in descending order.

SELECT end_terminal,
       duration_seconds,
       SUM(duration_seconds) OVER
         (PARTITION BY end_terminal ORDER BY duration_seconds DESC)
         AS running_total
  FROM tutorial.dc_bikeshare_q1_2012
 WHERE start_time < '2012-01-08'
 
57) Write a query that shows the 5 longest rides from each starting terminal, ordered by terminal, and longest to shortest rides within each terminal. Limit to rides that occurred before Jan. 8, 2012.

SELECT *
  FROM (
        SELECT start_terminal,
               start_time,
               duration_seconds AS trip_time,
               RANK() OVER (PARTITION BY start_terminal ORDER BY duration_seconds DESC) AS rank
          FROM tutorial.dc_bikeshare_q1_2012
         WHERE start_time < '2012-01-08'
               ) sub
 WHERE sub.rank <= 5
 
 with sub as (select start_terminal, row_number() over(partition by start_terminal ORDER by duration_seconds desc) as rank
 FROM tutorial.dc_bikeshare_q1_2012
 WHERE start_time < '2012-01-08')
 
 select * from sub
 where rank <= 5;
 
 58)It can often be useful to compare rows to preceding or following rows,
 
 SELECT *
  FROM (
    SELECT start_terminal,
           duration_seconds,
           duration_seconds -LAG(duration_seconds, 1) OVER
             (PARTITION BY start_terminal ORDER BY duration_seconds)
             AS difference
      FROM tutorial.dc_bikeshare_q1_2012
     WHERE start_time < '2012-01-08'
     ORDER BY start_terminal, duration_seconds
       ) sub
 WHERE sub.difference IS NOT NULL
 
 59) Write a query to calculate the average daily price change in Apple stock, grouped by year.
 
 select avg(close- open), year
 from tutorial.aapl_historical_stock_price
 group by year
 order by year;
 
 60)Write a query that calculates the lowest and highest prices that Apple stock achieved each month.
 
 select year, month, max(high) as max, min(low) as low
     FROM tutorial.aapl_historical_stock_price
      Group by year, month
      order by year, month;
      
  61) Write a query that includes a column that is flagged "yes" when a player is from California, and sort the results with those players first.
  
  select player_name, state, 
        (case when state = 'CA' then 'yes' 
              else 'no' END) as california_player
from benn.college_football_players
order by california_player desc ;


62)Write a query that includes players' names and a column that classifies them into four categories based on height. Keep in mind that the answer we provide is only one of many possible answers, since you could divide players' heights in many ways.

SELECT player_name,
       height,
       CASE WHEN height > 74 THEN 'over 74'
            WHEN height > 72 AND height <= 74 THEN '73-74'
            WHEN height > 70 AND height <= 72 THEN '71-72'
            ELSE 'under 70' END AS height_group
  FROM benn.college_football_players
  ```
  
  
63)Write a query that selects all columns from benn.college_football_players and adds an additional column that displays the player's name if that player is a junior or senior.
```sql

SELECT *,
       CASE WHEN year IN ('JR', 'SR') THEN player_name ELSE NULL END AS upperclass_player_name
  FROM benn.college_football_players
  
64) Write a query that counts the number of 300lb+ players for each of the following regions: West Coast (CA, OR, WA), Texas, and Other (Everywhere else).

select  case when state IN ('CA', 'OR', 'WA') then 'westcost'
              when state = 'TX' then 'Texas'
                else 'others' end as designation, count (*)
  from  benn.college_football_players
  where weight >= 300
  group by designation;
  
  select  count(case when state IN ('CA', 'OR', 'WA') then 1 else null end) as westcoast,
        count(case when state = 'TX' then 1 else null end) as texas,
        count( case when state NOT IN (  'CA', 'OR', 'WA', 'TX') then 1 else NULL end) as others
  from  benn.college_football_players
  where weight >= 300;
  
 65) Write a query that calculates the combined weight of all underclass players (FR/SO) in California as well as the combined weight of all upperclass players (JR/SR) in California.
  
SELECT sum(( case when year in ('FR' ,'SO') then weight  else 0 end)) as combined_underclassweight,
         sum(( case when year in('JR', 'SR') then weight else 0 end))as combined_upperclassweight
from benn.college_football_players
where state = 'CA'
;

66) Write a query that displays the number of players in each state, with FR, SO, JR, and SR players in separate columns and another column for the total number of players. Order results such that states with the most players come first. 
SELECT state,
       COUNT(CASE WHEN year = 'FR' THEN 1 ELSE NULL END) AS fr_count,
       COUNT(CASE WHEN year = 'SO' THEN 1 ELSE NULL END) AS so_count,
       COUNT(CASE WHEN year = 'JR' THEN 1 ELSE NULL END) AS jr_count,
       COUNT(CASE WHEN year = 'SR' THEN 1 ELSE NULL END) AS sr_count,
       COUNT(*) AS total_players
  FROM benn.college_football_players
 GROUP BY state
 ORDER BY total_players DESC 
 
 67) Write a query that shows the number of players at schools with names that start with A through M, and the number at schools with names starting with N - Z.   
 select count(case when school_name < 'n' then player_name else null end)as a_m,
             COUNT(case when school_name >= 'n'  then player_name else null end) as n_m
             
             from benn.college_football_players;

```
             
 68) Count the number of unique companies (don't double-count companies) and unique acquired companies by state. Do not include results for which there is no state data, and order by the number of acquired companies from highest to lowest.
 ```sql
                                           
  SELECT state_code, count(distinct(companies.permalink)) AS companies_permalink,
        count(distinct(acquisitions.company_permalink)) AS acquisitions_permalink
       
  FROM tutorial.crunchbase_companies companies
  Left JOIN tutorial.crunchbase_acquisitions acquisitions
    ON companies.permalink = acquisitions.company_permalink
    
    where state_code is not NULL
    group by state_code
    
    order by acquisitions_permalink desc;

  69) Write a query that appends the two crunchbase_investments datasets above (including duplicate values). Filter the first dataset to only companies with names that start with the letter "T", and filter the second to companies with names starting with "M" (both not case-sensitive). Only include the company_permalink, company_name, and investor_name columns.
                                           
     SELECT company_permalink,
       company_name,
       investor_name
  FROM tutorial.crunchbase_investments_part1
 WHERE company_name ILIKE 'T%'
 
 UNION ALL

SELECT company_permalink,
       company_name,
       investor_name
  FROM tutorial.crunchbase_investments_part2
 WHERE company_name ILIKE 'M%'
             
                                           
70) Write a query that returns all rows for which Ludacris was a member of the group.
                                           
select *
    from tutorial.billboard_top_100_year_end
    where ''group'' Like  '%Ludacris%' ; 
                 
                                           
71) Write a query that returns all rows for which the first artist listed in the group has a name that begins with "DJ".
                                           
    select *
    
    from tutorial.billboard_top_100_year_end
    where "group" like 'DJ%'
                                           
72) Write a query that returns all rows for songs that were on the charts in 2013 and do not contain the letter "a".                                           
       SELECT *
      FROM tutorial.billboard_top_100_year_end
     WHERE song_name NOT ILIKE '%a%'
       AND year = 2013
                                           
                                           
73) Trim Functions
    SELECT location,
       TRIM(both '()' FROM location)
  FROM tutorial.sf_crime_incidents_2014_01
                                           
                                           
                                           
 74)Write a query that separates the `location` field into separate fields for latitude and longitude. You can compare your results against the actual `lat` and `lon` fields in the table.
    select trim (leading '(' from left(location, position(',' in location)-1)) as lat , trim (trailing ')' from right(location, position(',' in location)+1)) as long
 from tutorial.sf_crime_incidents_2014_01;
 ```

 website analysis

```sql
 CREATE TABLE Lpayment
    ("date" timestamp,"memberid" varchar(11),"amount" int)
;

    
INSERT INTO Lpayment
    ("date","memberid","amount")
VALUES
    ('2019-01-03','A',33.9),
    ('2019-01-04','C',19.3),
    ('2019-01-07','A',29.1),
    ('2019-01-05','B',32.2),
    ('2019-01-07','C',35.5)
;

CREATE TABLE Luser
    ("date" timestamp,"memberid" varchar(11))
;
    
INSERT INTO Luser
    ("date","memberid")
VALUES
    ('2019-01-01','A'),
    ('2019-01-01','A'),
    ('2019-01-04','A'),
    ('2019-01-04','C'),
    ('2019-01-03','A'),
    ('2019-01-03','B'),
    ('2019-01-05','B'),
    ('2019-01-06','B'),
    ('2019-01-06','B'),
    ('2019-01-04','C'),
    ('2019-01-05','C'),
    ('2019-01-05','D'),
    ('2019-01-07','A'),
    ('2019-01-07','C')
;

CREATE TABLE Ldate
    ("date" timestamp)
;
    
INSERT INTO Ldate
    ("date")
VALUES
    ('2019-01-01'),
    ('2019-01-02'),
    ('2019-01-03'),
    ('2019-01-04'),
    ('2019-01-05'),
    ('2019-01-06'),
    ('2019-01-07'),
    ('2019-01-08')
;

```
### 75)Total Revenue & Payers: Create a table to display total revenue & number of buyers on each day
 
 ```sql
 select date, count(memberid) as number_of_users, sum(amount) as total_revenue
from Lpayment;
```
### 76) Top payers: Create a table to display the top one payer on each day
```sql
WITH sub AS
(
   SELECT *,
         ROW_NUMBER() OVER (PARTITION BY date ORDER BY amount DESC) AS row_num
   FROM Lpayment
)
SELECT date, memberid
FROM sub
WHERE row_num = 1
ORDER BY date,member_id;
```
 ### 77) A distribution of # days active within a week: Create a table to show how many members are active for 1 day, 2days, 3days,…7days during 01/01-07/01.
```sql
 WITH sub AS
(
    SELECT DISTINCT date, memberid
    FROM Luser
    WHERE date BETWEEN '2019-01-01' AND '2019-01-07'
    GROUP BY memberid, date
) ,
sub1 as(
    SELECT memberid, COUNT(memberid) as days_active
    FROM sub
    GROUP by memberid)
    
    SELECT days_active, COUNT(days_active) as count
    FROM  sub1
    GROUP BY days_active
    ORDER BY 1,2;    

             
###76) SQL script for nth highest salary
/****** Script for SelectTopNRows command from SSMS  ******/
/****** Analysis for nth highest salary  ******/
SELECT TOP (1000) [ID]
      ,[FirstName]
      ,[LastName]
      ,[Gender]
      ,[Salary]
  FROM [AK ].[dbo].[Employees];
  
  select FirstName, salary, dense_rank() over(order by salary desc) as rank1
  from Employees;

  with cte as (select FirstName, salary, dense_rank() over(order by salary desc) as rank1
  from Employees)

  select *
  from cte
  where rank1 = 4;

  select id, salary, firstName from Employees e
  where  3 = (select count(distinct salary) from employees m  where e.salary  < m.salary);

   select salary as sal from Employees e
  where  3 = (select count(distinct salary) from employees m  where e.salary  < m.salary);


select max(salary) from employees where salary < (select max(salary) from Employees);

select Top 1 salary
from (select Distinct top 4 salary 
		from employees
		order by salary desc) e
order by salary;

select Distinct top 5 salary
		from employees
		order by salary desc;

select Distinct top 5 salary, firstname
		from employees
		order by salary desc;

  ### 77) Add the role_desc from the role table to the player_match table. The player_match and role tables have a matching column.

  -player_match
match_id    player_id   role_id  
  335987    1            1    

  --role
role_id        role_desc       
  1            Captain   

  select match_id,
         player_id, 
        role_id, 
        role_desc
    from player_match p
    inner join role r
      on p.role_id = r.role_id
    order by match_id

###78) Combine the team and umpire tables so that every team is matched to every umpire.

--team
team_id        team_name
1            Kolkata Knight Riders
2            Royal Challengers
3            Chennai Super Kings
--umpire
umpire_id    umpire_name     umpire_country
1            Asad Rauf           6
2            MR Benson           10
3            Aleem Dar           6

selct team_name, umpire_name
    from team
  /* cross joining the table */
    cross join umpire
    order by umpire
    limit 10;
  
### 79) The wine table gives information about wines stocked in an online wine retailer. Find all of the countries that have an average wine price greater than $15.

select country
    from wine
  group by country
  having avg(price) > 15 
  order by country;

/*formatted query 
SELECT f.species_name,
        AVG(f.height) AS average_height, AVG(f.diameter) AS average_diameter
   FROM flora AS f
  WHERE f.species_name = 'Banksia'
     OR f.species_name = 'Sheoak'
     OR f.species_name = 'Wattle'
  GROUP BY f.species_name, f.observation_date) */




###80) The orange_cap column contains the id of the player who won the orange cap title.

List the season year and the name of the Orange Cap holder.

-season
season_id    man_of_the_series   orange_cap  purple_cap season_year
1        32                      100             102         2008
2        53                      18              61          2009
3         133                     133             131         2010

--player
player_id    player_name     country_id
1            SC Ganguly          1
2            BB McCullum         4
3            RT Ponting          5

select season_year, player_name
    from season 
  Left  Join player
      on season.orange_cap = player.player_id
  order by season_year;

### 81)The structure of the employees table is shown. Complete the query to determine which employees are also managers.

select  first_name
    from employees e
  (where employee id in (select manager_id from employees m));

### 82) The cricketers table lists details of a few players.

Which country has players with a combined weight that is greater than 300 kg?

--cricketers
name            weight  age     height  country_name
Jimmy Neesham        80   29        187  New Zealand
Virat Kohli         72   30        175  India
Rishabh Pant        62   21        170  India

select country_name, 
       sum(weight)
    from crickters 
  group by country_name
  having sum(weight) > 300;

### 83) The IPL_runs table contains the total runs scored by a player per match. Calculate the total number of runs scored by each player over all matches. Display the TOP 5 scorers.

--IPL_runs
match_id    player_id   player_name         runs    
549337            304         F du Plessis    417
549323            304         F du Plessis    410
...
734004             27         Yuvraj Singh    495
734010             27         Yuvraj Singh    449

SELECT IPL_runs.player_id,
	   sum(runs)
FROM IPL_runs
GROUP BY player_id
ORDER BY sum(runs) desc
LIMIT 5;

### 84) Extract the names of cities that start with ('A', 'E', 'I', 'O', 'U'). Your results should not contain duplicates.

select name
    from city 
  where left(name, 1) in ('A', 'E', 'I', 'O', 'U')
  order by name;

### 85) Include a subquery in the WHERE clause to identify rows where the win_margin is greater than the average win_margin, from the match dataset.

select match_id, win_margin
    from match
  where 
    win_margin > (select avg(win_margin)from match)
  order by win_margin
  limit 10;

### 86) The sentence "Movies, TV, Series" is delimited using a comma. Split the sentence and return the 2nd part.

select split_part("Movies, TV, Series" , ',', 2) as new_string;




 






