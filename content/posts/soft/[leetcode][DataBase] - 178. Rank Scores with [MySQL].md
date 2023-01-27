[problem](https://leetcode.com/problems/rank-scores/)
```sql
select inn.Score,inn.Rank from (
    select ss.Score,@row:=@row+1 as Rank
    from 
    (select @row:=0) a,
    (select Score from Scores group by Score) as ss
    order by ss.Score desc
) as inn
right join
Scores as sss
on inn.Score = sss.Score
order by inn.Score desc
;
```

