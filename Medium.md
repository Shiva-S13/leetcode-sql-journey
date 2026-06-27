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





