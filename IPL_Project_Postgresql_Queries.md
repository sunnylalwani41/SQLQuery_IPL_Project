```bash
select batsman, count(CASE WHEN batsman_runs=6 THEN 1 ELSE NULL END) from deliveries d inner join matches m on d.match_id= m.id and m.season=2016 group by batsman;
```
