[problem](https://leetcode.com/problems/nth-highest-salary/)

```sql
      # 184ms
      select min(aa.Salary) from 
      (select distinct Salary from employee order by Salary desc limit N) as aa
      where
      (select count(distinct Salary) from employee) >= N
```
or
```sql
      # 188ms       
      select aa.Salary as ass from 
      (select distinct Salary from employee order by Salary desc limit N) as aa
      where
      (select count(distinct Salary) from employee) >= N
      order by ass
      limit 1
```
