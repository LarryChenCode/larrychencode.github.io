# SQL Query: Video Games Analysis
When Was the Golden Era of Video Games?
---
<img src="../images/video-game/video-games-golden-era-cover.gif?raw=true"/>

## Introduction to the SQL Project on Video Games Analysis
Dive into the $300 billion gaming market to explore if video games are improving or if their golden age has passed. This SQL project involves analyzing scores and sales of the top 400 video games since 1977 to 2017. Identify peak years in gaming popularity and examine sales trends by joining datasets and applying set theory, filtering, grouping, and sorting techniques. Gear up for a data-driven journey through gaming history!

## Analysis
### Best_selling_games
SQL Code:
```SQL
SELECT *
FROM game_sales
ORDER BY games_sold DESC
LIMIT 10;
```

Table:
<img src="../images/video-game/best-selling-games.png?raw=true"/>


#### Games Sold by Platform & Publisher
<img src="../images/video-game/games-sold.png?raw=true"/>

Nintendo on the Wii platform has a towering lead over other combinations.

#### Games Sold by year
<img src="../images/video-game/games-sold-by-year.png?raw=true"/>

A significant peak in sales around the year 2005. This suggests a particularly successful period for the gaming industry, with interest or production sharply decreasing in subsequent years.

### Ten years with the highest average critic score
SQL Code:
```SQL
SELECT g.year, COUNT(g.name) AS num_games, ROUND(AVG(r.critic_score),2) AS avg_critic_score
FROM game_sales g
INNER JOIN reviews r
ON g.name = r.name
GROUP BY g.year
HAVING COUNT(g.name) > 4
ORDER BY avg_critic_score DESC
LIMIT 10;
```

Table:
<img src="../images/video-game/critic-score.png?raw=true"/>

### Years where critics and users broadly agreed that games released were highly rated 
SQL Code:
```SQL
SELECT u.year, u.num_games, c.avg_critic_score, u.avg_user_score, GREATEST(c.avg_critic_score, u.avg_user_score) - LEAST(c.avg_critic_score, u.avg_user_score) AS diff
FROM critics_avg_year_rating c
INNER JOIN users_avg_year_rating u
ON c.year = u.year
WHERE c.avg_critic_score > 9 OR u.avg_user_score > 9
ORDER BY diff ASC
```

Table:
<img src="../images/video-game/golden-year.png?raw=true"/>

#### Critic Score vs User Score
<img src="../images/video-game/critic-score-vs-user-score.png?raw=true"/>

The graph shows a sharp decline in this difference in the early years, reaching a more stable level in the middle years before starting to rise again. This could indicate that initially, there was a large discrepancy between critic and user opinions, which then aligned more closely over time. However, the trend towards the end suggests that the gap between critic and user opinions is widening again. **This could suggest a divergence in the expectations or satisfaction levels of critics versus users in more recent years.**

#### The project was completed on [Datacamp](https:/app.datacamp.com/learn/projects/2013) 