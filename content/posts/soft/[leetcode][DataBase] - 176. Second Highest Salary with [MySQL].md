[problem](https://leetcode.com/problems/second-highest-salary/)
```sql
select ifnull((select distinct Salary from Employee order by Salary desc limit 1,1), null) as SecondHighestSalary;
```

or other
```sql
select max(Salary) as SecondHighestSalary from employee where Salary < (select max(Salary) from employee);
```
