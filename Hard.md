
## [185 Department Top Three Salaries](https://leetcode.com/problems/department-top-three-salaries/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
WITH CTE AS
(
    SELECT
        d.name AS Department,
        e.name AS Employee,
        e.salary,
        DENSE_RANK() OVER
        (
            PARTITION BY e.departmentId
            ORDER BY e.salary DESC
        ) AS rn
    FROM Employee e
    JOIN Department d
        ON e.departmentId = d.id
)

SELECT
    Department,
    Employee,
    Salary
FROM CTE
WHERE rn <= 3;
```
## [601. Human Traffic of Stadium](https://leetcode.com/problems/human-traffic-of-stadium/description/)
```sql
with CTE as (select *
,id-row_number() over(order by id) as grp 
from stadium 
where people >100)

select id, visit_date, people
from CTE where grp in
( select grp from CTE 
group by grp
having count(*)>=3)
order by visit_date

```
## [262. Trips and Users](https://leetcode.com/problems/trips-and-users/description/)
```sql

SELECT
    t.request_at AS Day,
    ROUND(
        SUM(CASE
                WHEN t.status <> 'completed' THEN 1.0
                ELSE 0
            END) / COUNT(*),
        2
    ) AS [Cancellation Rate]
FROM Trips t
JOIN Users c
    ON t.client_id = c.users_id
JOIN Users d
    ON t.driver_id = d.users_id
WHERE c.banned = 'No'
  AND d.banned = 'No'
  AND t.request_at BETWEEN '2013-10-01' AND '2013-10-03'
GROUP BY t.request_at
ORDER BY t.request_at;
```
## [618_Students_Report_by_Geography](https://github.com/shawlu95/Beyond-LeetCode-SQL/tree/master/LeetCode/618_Students_Report_by_Geography)
```sql

WITH CTE AS
(
    SELECT
        name,
        continent,
        ROW_NUMBER() OVER
        (
            PARTITION BY continent
            ORDER BY name
        ) AS rn
    FROM Student
)

SELECT
    MAX(CASE WHEN continent = 'America' THEN name END) AS America,
    MAX(CASE WHEN continent = 'Asia' THEN name END) AS Asia,
    MAX(CASE WHEN continent = 'Europe' THEN name END) AS Europe
FROM CTE
GROUP BY rn
ORDER BY rn;
```
## [615_Average_Salary](https://github.com/shawlu95/Beyond-LeetCode-SQL/tree/master/LeetCode/615_Average_Salary)
```sql
SELECT
    a.Pay_Month, a.department_id,
    CASE
        WHEN a.Dept_Avg > b.Com_Avg THEN 'higher'
        WHEN a.Dept_Avg < b.Com_Avg THEN 'lower'
        ELSE 'same'
    END AS comparison
FROM
(SELECT
        e.department_id,
        DATEFROMPARTS( YEAR(s.pay_date),MONTH(s.pay_date),1) AS Pay_Month,
        AVG(s.amount * 1.0) AS Dept_Avg
    FROM Employee e
    JOIN Salary s
        ON e.employee_id = s.employee_id
    GROUP BY
        e.department_id,
        DATEFROMPARTS(YEAR(s.pay_date), MONTH(s.pay_date),1)
) AS a
JOIN
(SELECT
        DATEFROMPARTS( YEAR(pay_date), MONTH(pay_date), 1) AS Pay_Month,
        AVG(amount * 1.0) AS Com_Avg
    FROM Salary
    GROUP BY
        DATEFROMPARTS(YEAR(pay_date), MONTH(pay_date), 1)
) AS b
ON a.Pay_Month = b.Pay_Month
ORDER BY
    a.Pay_Month DESC,
    a.department_id;
```

## [601. Human Traffic of Stadium](https://leetcode.com/problems/human-traffic-of-stadium/description/)

```sql
WITH CTE AS
(
    SELECT
        id,
        visit_date,
        people,
        ROW_NUMBER() OVER (ORDER BY id) AS rn,
        id - ROW_NUMBER() OVER (ORDER BY id) AS grp
    FROM Stadium
    WHERE people >= 100
)

SELECT
    id,
    visit_date,
    people
FROM CTE
WHERE grp IN
(
    SELECT grp
    FROM CTE
    GROUP BY grp
    HAVING COUNT(*) >= 3
)
ORDER BY visit_date;
```



