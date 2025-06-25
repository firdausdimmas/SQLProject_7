# When Was the Golden Era of Video Games?

### Project Overview

![video_game](https://github.com/user-attachments/assets/bcd08591-a363-41bb-af80-4dc0ba318727)

This project analyzes data on video game critic scores, user ratings, and sales for the top 400 video games released since 1977. The goal of this project is to explore whether games are improving over time or if the golden age of video games has already passed. By examining trends in review scores from both critics and users, as well as analyzing global sales data, this project aims to uncover the most celebrated eras in gaming history and provide insights into the business dynamics behind successful games.

### Data Sources

The database contains four tables. Each table has been limited to 400 rows for this project, but we can find the complete dataset with over 13,000 games on Kaggle.

<h4><code>game_sales</code> table</h4>

<table>
  <thead>
    <tr>
      <th>Column</th>
      <th>Definition</th>
      <th>Data Type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>name</code></td>
      <td>Name of the video game</td>
      <td><code>varchar</code></td>
    </tr>
    <tr>
      <td><code>platform</code></td>
      <td>Gaming platform</td>
      <td><code>varchar</code></td>
    </tr>
    <tr>
      <td><code>publisher</code></td>
      <td>Game publisher</td>
      <td><code>varchar</code></td>
    </tr>
    <tr>
      <td><code>developer</code></td>
      <td>Game developer</td>
      <td><code>varchar</code></td>
    </tr>
    <tr>
      <td><code>games_sold</code></td>
      <td>Number of copies sold (millions)</td>
      <td><code>float</code></td>
    </tr>
    <tr>
      <td><code>year</code></td>
      <td>Release year</td>
      <td><code>int</code></td>
    </tr>
  </tbody>
</table>

<h4><code>reviews</code> table</h4>

<table>
  <thead>
    <tr>
      <th>Column</th>
      <th>Definition</th>
      <th>Data Type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>name</code></td>
      <td>Name of the video game</td>
      <td><code>varchar</code></td>
    </tr>
    <tr>
      <td><code>critic_score</code></td>
      <td>Critic score according to Metacritic</td>
      <td><code>float</code></td>
    </tr>
    <tr>
      <td><code>user_score</code></td>
      <td>User score according to Metacritic</td>
      <td><code>float</code></td>
    </tr>
  </tbody>
</table>

<h4><code>users_avg_year_rating</code> table</h4>

<table>
  <thead>
    <tr>
      <th>Column</th>
      <th>Definition</th>
      <th>Data Type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>year</code></td>
      <td>Release year of the games reviewed</td>
      <td><code>int</code></td>
    </tr>
    <tr>
      <td><code>num_games</code></td>
      <td>Number of games released that year</td>
      <td><code>int</code></td>
    </tr>
    <tr>
      <td><code>avg_user_score</code></td>
      <td>Average score of all the games ratings for the year</td>
      <td><code>float</code></td>
    </tr>
  </tbody>
</table>

<h4><code>critics_avg_year_rating</code> table</h4>

<table>
  <thead>
    <tr>
      <th>Column</th>
      <th>Definition</th>
      <th>Data Type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>year</code></td>
      <td>Release year of the games reviewed</td>
      <td><code>int</code></td>
    </tr>
    <tr>
      <td><code>num_games</code></td>
      <td>Number of games released that year</td>
      <td><code>int</code></td>
    </tr>
    <tr>
      <td><code>avg_critic_score</code></td>
      <td>Average score of all the games ratings for the year</td>
      <td><code>float</code></td>
    </tr>
  </tbody>
</table>

### Exploratory Data Analysis (EDA)

EDA involved exploring the database to answer key questions, such as:
- What are the best-selling video games of all time?
- Which years had the highest average critic scores, considering years with at least four game releases?
- In which years did critics and users strongly agree that games were exceptionally good (average critic score over 9 or average user score over 9)?

### Data Analysis

Including some interesting code/features worked with

```sql
-- best_selling_games
SELECT *
FROM game_sales
ORDER BY games_sold DESC
LIMIT 10;
```

```sql
-- critics_top_ten_years
SELECT gs.year AS year,
       COUNT(gs.name) AS num_games,
       ROUND(AVG(r.critic_score),2) AS avg_critic_score
FROM game_sales AS gs
INNER JOIN reviews AS r
ON gs.name = r.name
GROUP BY gs.year
HAVING COUNT(gs.name) > 4
ORDER BY avg_critic_score DESC
LIMIT 10;
```
```sql
-- golden_years
SELECT ur.year,
       ur.num_games,
       cr.avg_critic_score,
       ur.avg_user_score,
       ur.avg_user_score - cr.avg_critic_score AS diff
FROM users_avg_year_rating AS ur
INNER JOIN critics_avg_year_rating AS cr
ON ur.year = cr.year
WHERE avg_critic_score > 9 OR avg_user_score > 9
ORDER BY year ASC;
```

### Results/Findings

The analysis results are summarized as follows:
- The top ten best-selling games include Wii Sports for Wii, Super Mario Bros, and Counter Strike: Global Offensive, among others. Most of these games were released between the late 2000s and 2010s.
- The years with the highest average critic scores are 1998, 2002, and 2004, with 1998 topping the list with an average critic score of 9.32.
- Years like 1997, 1998, 2004, and 2008 are identified as golden years where either critics or users had average scores above 9.

### Recommendations

Based on the analysis, we recommend the following actions:
- Remasters, remakes, or spiritual successors of the critically acclaimed games could attract both nostalgic players and new audiences.
- Future game development strategies should focus on incorporating common traits of top-selling games, such as accessibility, strong multiplayer features, or innovative gameplay mechanics to maximize commercial success.
- Publishers can leverage nostalgia-driven marketing while ensuring modern gameplay standards, graphics, and online features are met.
- Analyze which genres and platforms were dominant during the highest-rated and best-selling periods.
