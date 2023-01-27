[题目](https://leetcode.com/problems/combine-two-tables/)
```sql
select FirstName, LastName, City, State 
    from Person left join Address
    using (PersonId);
```

or
```sql
select FirstName, LastName, City, State 
    from Person left join Address
    on Person.PersonId=Address.PersonId;
```
