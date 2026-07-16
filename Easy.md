
## [1251. Average Selling Price](https://leetcode.com/problems/average-selling-price/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
select p.product_id, cast(sum(p.price*u.units)*1.0/sum(u.units) as decimal(6,2)) as Average_revenue
from Prices P left join 
UnitsSold u
on p.product_id=u.product_id and u.purchase_date between p.start_date and p.end_date
group by p.product_id
```
## [1075. Project Employees](https://leetcode.com/problems/project-employees-i/description/?envType=study-plan-v2&envId=top-sql-50)
```sql

select p.project_id, cast(avg(e.experience_years*1.0) as decimal(6,2))as Average_experience
from Project P join Employee e 
on p.employee_id=e.employee_id
group by p.project_id
```
## [1978. Employees Whose Manager Left the Company](https://leetcode.com/problems/employees-whose-manager-left-the-company/?envType=study-plan-v2&envId=top-sql-50)
```sql

SELECT e.employee_id
FROM Employees e
LEFT JOIN Employees m
    ON e.manager_id = m.employee_id
WHERE e.salary < 30000
  AND e.manager_id IS NOT NULL
  AND m.employee_id IS NULL
ORDER BY e.employee_id;

SELECT employee_id
FROM Employees
WHERE salary < 30000
  AND manager_id IS NOT NULL
    AND manager_id NOT IN (
        SELECT employee_id
        FROM Employees) 
ORDER BY employee_id;
```
## [626. Exchange Seats](https://leetcode.com/problems/exchange-seats/description/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT id, 
CASE
    WHEN id % 2 = 0 
        THEN LAG(student) OVER(ORDER BY id)
    ELSE COALESCE(LEAD(student) OVER(ORDER BY id), student)
END AS student
FROM Seat;
```























