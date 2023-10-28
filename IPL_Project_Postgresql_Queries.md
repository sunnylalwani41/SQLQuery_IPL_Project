#### 1 Number of matches played per year of all the years in IPL.

**Query**:
```bash

select season, count(id) number_of_matches_per_year from matches group by season order by season asc;
```

#### 2 Number of matches won of all teams over all the years of IPL.

**Query**

```bash
select winner, count(id) from matches where length(winner)!=0 group by winner order by winner;
```

#### 3 For the year 2016 get the extra runs conceded per team.

**Query**

```bash
select d.bowling_team, sum(d.extra_runs) extra_runs_in_2016 from deliveries d inner join matches m on d.match_id = m.id and m.season=2016 group by d.bowling_team order by d.bowling_team;
```

#### 4 For the year 2015 get the top economical bowlers.

**Query**

```bash
select d.bowler, sum(((total_runs-(legbye_runs+bye_runs+penalty_runs))*6))/sum(CASE WHEN wide_runs!=0 THEN 0 WHEN noball_runs!=0 THEN 0 ELSE 1 END) economy_of_bowler
from deliveries d inner join matches m
on d.match_id= m.id and m.season=2015 group by bowler order by economy_of_bowler;
```

#### 5 Number of matches played by a Team in a 2016 year.

**Query**

```bash
select team, sum(c1) from (
(select team1 team, count(team1) c1 from matches where season = 2016 group by team1) union all
(select team2 team, count(team2) c1 from matches where season = 2016 group by team2)) abc group by team;
```

#### 6 Hit Number of Sixers By Batsman In The 2016 Year.

**Query**

```bash
select batsman, count(CASE WHEN batsman_runs=6 THEN 1 ELSE NULL END) from deliveries d inner join matches m on d.match_id= m.id and m.season=2016 group by batsman;
```

#### 7 Display the match analysis of the match id 1.

**Query**

```bash
select batting_team, batsman, sum(batsman_runs) from deliveries where match_id=1 group by batting_team, batsman;

select bowling_team, bowler, count(CASE WHEN dismissal_kind='' THEN NULL WHEN dismissal_kind='run out' THEN NULL ELSE 1 END) from deliveries where match_id=1 group by bowling_team, bowler;

```

#### 8 Display the team name, who won at least 3 times.

**Query**

```bash
select m1.winner, count(m1.winner) number_of_win from matches m1 inner join
(select m.season, max(m.date) date2 from matches m group by m.season)
as m2 on m1.date=m2.date2 and m1.winner!='' group by m1.winner having count(m1.winner)>=3 order by number_of_win desc;
```

#### 9 Compare the performance Between Sunriders Hyderabad vs Mumbai Indians.

```bash
(select season, 'Sunrisers Hyderabad' Team_Name, count(CASE WHEN (team1= 'Sunrisers Hyderabad' OR team2= 'Sunrisers Hyderabad') AND winner='Sunrisers Hyderabad' THEN 1 ELSE NULL END) number_of_wins, count(CASE WHEN (team1= 'Sunrisers Hyderabad' OR team2= 'Sunrisers Hyderabad') AND winner<>'Sunrisers Hyderabad' THEN 1 ELSE NULL END) number_of_loss from matches group by season order by season) UNION ALL
(select season, 'Mumbai Indians' Team_Name, count(CASE WHEN (team1= 'Mumbai Indians' OR team2= 'Mumbai Indians') AND winner='Mumbai Indians' THEN 1 ELSE NULL END) number_of_wins, count(CASE WHEN (team1= 'Mumbai Indians' OR team2= 'Mumbai Indians') AND winner<>'Mumbai Indians' THEN 1 ELSE NULL END) as number_of_loss from matches group by season order by season);
```
