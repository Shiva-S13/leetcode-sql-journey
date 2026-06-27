## [585. Investments in 2016](https://leetcode.com/problems/investments-in-2016/)
```sql
SELECT ROUND(SUM(i1.tiv_2016), 2) AS tiv_2016
FROM Insurance i1
WHERE i1.tiv_2015 IN (
    SELECT tiv_2015
    FROM Insurance
    GROUP BY tiv_2015
    HAVING COUNT(*) > 1
)
AND (i1.lat, i1.lon) IN (
    SELECT lat, lon
    FROM Insurance
    GROUP BY lat, lon
    HAVING COUNT(*) = 1
);
```

## [602. Friend Requests II: Who Has the Most Friends](https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends/description/)
```sql
WITH CTE AS (
    SELECT requester_id AS id
    FROM RequestAccepted

    UNION ALL

    SELECT accepter_id AS id
    FROM RequestAccepted
)

SELECT TOP 1
    id,
    COUNT(id) AS num
FROM CTE
GROUP BY id
ORDER BY num DESC;
```
## [1321. Restaurant Growth](https://leetcode.com/problems/restaurant-growth/description/)
```sql
with Daily_amount as (SELECT
        visited_on,
        SUM(amount) AS amount
    FROM Customer
    GROUP BY visited_on)

    select visited_on, 
    sum(amount) over (order by visited_on rows between 6 preceding and current row) as amount,
    round(Avg(amount*1.0) over (order by visited_on rows between 6 preceding and current row),2) as average_amount
    from Daily_amount
    order by Visited_on asc
    offset 6 rows
```

## [1341. Movie Rating](https://leetcode.com/problems/movie-rating/description/)
```sql
SELECT *
FROM (
    SELECT TOP 1
        u.name AS results
    FROM MovieRating r
    JOIN Users u
        ON u.user_id = r.user_id
    GROUP BY u.name
    ORDER BY COUNT(*) DESC, u.name ASC
) a

UNION ALL

SELECT *
FROM (
    SELECT TOP 1
        m.title AS results
    FROM MovieRating r
    JOIN Movies m
        ON r.movie_id = m.movie_id
    WHERE r.created_at >= '2020-02-01'
      AND r.created_at < '2020-03-01'
    GROUP BY m.title
    ORDER BY AVG(r.rating * 1.0) DESC, m.title ASC
) b;
```

## [1484. Group Sold Products By The Date](https://leetcode.com/problems/group-sold-products-by-the-date/description/)
```sql
select 
sell_date, 
count(distinct product) as num_sold,
string_agg(product, ',') as Products 
from 
(select distinct sell_date, product from Activities) a
group by sell_date
order by sell_date;
```
## [1327. List the Products Ordered in a Period](https://leetcode.com/problems/list-the-products-ordered-in-a-period/description/)
```sql
select p.product_name,
       sum(o.unit) as unit
from Products p 
join Orders o
    on p.product_id=o.product_id
where o.order_date>='2020-02-01' 
   and o.order_date<'2020-03-01'
group by p.product_name
having sum(o.unit)>=100;
```
## [626. Exchange Seats](https://leetcode.com/problems/exchange-seats/description/)
```sql
SELECT id, 
CASE
    WHEN id % 2 = 0 
        THEN LAG(student) OVER(ORDER BY id)
    ELSE COALESCE(LEAD(student) OVER(ORDER BY id), student)
END AS student
FROM Seat;
```
## [1907. Count Salary Categories](https://leetcode.com/problems/count-salary-categories/description/)
```sql
with Categories as(
    select 'Low Salary' as Category
    union all
    select 'Average Salary' 
    union all
    select 'High Salary' ),

 Salary_Counts as (select
case
 when income <20000 then 'Low Salary'
 when income <= 50000 then 'Average Salary'
 else 'High Salary'
 end as Category,
 count(*) as accounts_count
from Accounts
group by case
 when income <20000 then 'Low Salary'
 when income <= 50000 then 'Average Salary'
 else 'High Salary'
 end)

select C.Category, coalesce(s.accounts_count,0) as accounts_count
from Categories c left join Salary_Counts s
on c.Category=s.Category
```
## [1204. Last Person to Fit in the Bus](https://leetcode.com/problems/last-person-to-fit-in-the-bus/description/)
```sql
with CTE as (select person_name, turn, sum(weight) over (order by turn) as Total_weight
from Queue)
Select Top 1 person_name
from CTE
where Total_weight<=1000
order by turn desc;
```
## [1517. Find Users With Valid E-Mails](https://leetcode.com/problems/find-users-with-valid-e-mails/description/)
```sql
SELECT *
FROM Users
WHERE mail REGEXP '^[A-Za-z][A-Za-z0-9_.-]*@leetcode\\.com$';
```

