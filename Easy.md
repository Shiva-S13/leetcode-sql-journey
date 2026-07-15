
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
