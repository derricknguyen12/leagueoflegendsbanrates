## Introduction

The *League of Legends Ban Rates of Champions Analysis* is an in-depth data science project conducted at UCSD that dives into multiple factors of the game. This project includes stages such as exploratory data analysis, hypothesis testing, baseline model development, and a concluding a fairness assessment. The primary goal is to comprehend how the ban rates of different champions impact overall statistics and results in League of Legends. The secondary goal is to build a model that can accurately predict the league a match is played in using various features.

### General Introduction to League of Legends

League of Legends (LOL) is one of the most popular multiplayer online games ever developed by Riot Games, boasting millions of players worldwide and establishing itself as one of the most influential esports in the industry. Our dataset, provided by Oracle's Elixir, records match data from professional LOL esports matches throughout 2018.

This dataset captures key gameplay statistics and outcomes, offering valuable insights into player behavior, team dynamics, and match performance. It includes a variety of features such as individual player performance, team strategies, in-game statistics, and overall match dynamics.

In League of Legends, champion bans are significant and heavily influence a match's trajectory. Before the game begins, each team bans five champions, preventing the opposing team from selecting them. These bans not only impact the immediate game but also shape the overall dynamics, strategies, and potential outcomes of the match.

Our main goal is to analyze the effectiveness of banned champions and their relationship with other game statistics in the dataset. We aim to examine how champion bans affect team performance, strategic decisions, in-game metrics, and match outcomes. Using data analysis techniques, we will explore the impact of these bans and develop a predictive model to determine the league of the teams.

This predictive model has the potential to enhance strategic decision-making, optimize team compositions, and improve the overall gameplay experience. By understanding the effects of champion bans, teams can make more informed decisions, leading to better performance and success in matches.

### Column Information

Our dataset revolves around the game of League of Legends, and we are investigating the most frequently banned champions to determine if their ban rates correlate with higher win rates compared to champions that are not frequently banned. **Our main research question is: Do champions that are often banned have a higher win rate than those who are not banned?**

This analysis is relevant to our readers because, if you are visiting our website, you are likely interested in playing League of Legends and would benefit from knowing which champions are more effective to use.

Our dataset comprises 80,904 rows and 17 columns. The key columns we are using include:
- `champion`: The name of the champion that players pick and play with.
- `ban1, ban2, ban3, ban4, ban5`: The names of the champions that are banned.
- `pick1, pick2, pick3, pick4, pick5`: The names of the champions that are picked to be used in the game.
- `result`: Indicates whether the picked champion won or lost the game (0 for a loss, 1 for a win).
- `participantid`: Index of player/team.
- `datacompleteness`: Indicates the presence of missing values.
- `teamname`: The names of the teams that are competing in their individual leagues.
- `gameid`: The id or reference number of the game that is being played.
- `league`: The name of the league that the players are playing in.

This comprehensive dataset allows us to conduct a detailed analysis to answer our research question and provide valuable insights for League of Legends players.

## Data Cleaning and Exploratory Data Analysis

### Cleaned Data

To optimize our data cleaning workflow, we initially filtered the dataset to retain only the relevant columns: 'champion', 'ban1', 'ban2', 'ban3', 'ban4', 'ban5', 'pick1', 'pick2', 'pick3', 'pick4', 'pick5', 'result', 'participantid', 'teamname', 'gameid', and 'league'. Each game entry consists of 12 rows, with 10 rows representing individual player data and 2 rows summarizing team performance and outcome. We preserved all rows as they are integral for our analysis.

Subsequently, we divided the dataset into two separate dataframes: one for teams and another for players. This division was based on the 'participantid' column, where values greater than or equal to 100 were assigned to the team dataframe, while values less than 100 were assigned to the player dataframe. This distinction was made because player IDs range from 1 to 10, whereas team IDs range from 100 to 200. During this process, we removed certain columns with missing data, such as the 'champions' column from the team dataframe, as it does not make sense for a team to have selected only one champion.

#### Team Dafaframe:

| league   | gameid    |   participantid | datacompleteness   | teamname            | ban1    | ...     | ban5     | pick1     | pick2     | pick3   | pick4    | pick5      |   result |
|:---------|:----------|----------------:|:-------------------|:--------------------|:--------|:--------|:---------|:----------|:----------|:--------|:---------|:-----------|---------:|
| LPL      | 2899-3157 |             100 | complete           | Invictus Gaming     | Azir    | ...     | Vladimir | Ornn      | Ezreal    | Orianna | Kha'Zix  | Tahm Kench |        1 |
| LPL      | 2899-3157 |             200 | complete           | Royal Never Give Up | Zoe     | ...     | Alistar  | Ryze      | Jarvan IV | Braum   | Vayne    | Maokai     |        0 |
| LPL      | 2899-3158 |             100 | complete           | Royal Never Give Up | Zoe     | ...     | Lee Sin  | Jarvan IV | Ryze      | Alistar | Vladimir | Tristana   |        1 |
| LPL      | 2899-3158 |             200 | complete           | Invictus Gaming     | Camille | ...     | Jhin     | Kalista   | Azir      | Braum   | Ivern    | Maokai     |        0 |
| LPL      | 2899-3159 |             100 | complete           | Invictus Gaming     | Azir    | ...     | Vladimir | Ornn      | Kha'Zix   | Ezreal  | Taliyah  | Leona      |        0 |

#### Player Dataframe:

| league   | gameid    |   participantid | datacompleteness   | teamname        | champion   | ban1   | ban2     | ban3    | ban4   | ban5     |   result |
|:---------|:----------|----------------:|:-------------------|:----------------|:-----------|:-------|:---------|:--------|:-------|:---------|---------:|
| LPL      | 2899-3157 |               1 | complete           | Invictus Gaming | Ornn       | Azir   | Malzahar | Camille | Illaoi | Vladimir |        1 |
| LPL      | 2899-3157 |               2 | complete           | Invictus Gaming | Kha'Zix    | Azir   | Malzahar | Camille | Illaoi | Vladimir |        1 |
| LPL      | 2899-3157 |               3 | complete           | Invictus Gaming | Orianna    | Azir   | Malzahar | Camille | Illaoi | Vladimir |        1 |
| LPL      | 2899-3157 |               4 | complete           | Invictus Gaming | Ezreal     | Azir   | Malzahar | Camille | Illaoi | Vladimir |        1 |
| LPL      | 2899-3157 |               5 | complete           | Invictus Gaming | Tahm Kench | Azir   | Malzahar | Camille | Illaoi | Vladimir |        1 |

### Univariate Analysis

We executed univariate analysis on the champions winrate in this dataset.

<iframe
  src="assets/Champion_Win_Rate.html"
  width="1250"
  height="600"
  frameborder="0"
></iframe>

This bar chart displays the win rate for each character selected by players. 

We also made a bar chart for the ban rates of champions in this data set.

<iframe
  src="assets/Banned_Champion_Counts.html"
  width="1250"
  height="600"
  frameborder="0"
></iframe>

This bar chart shows the number of times each champion has been banned.

### Bivariate Analysis

We permformed bivariate analysis on the pick rate, ban rate, and win rate of the champions in the dataset to visualize.

<iframe
  src="assets/Pick_vs_Ban_vs_Win_Rate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

According to our plot, there is a postive correlation between pick rate and ban rate of the champions.

### Interesting Aggregates
Aggregate DataFrame:

| is_banned   |   result |   banrates |
|:------------|---------:|-----------:|
| False       | 0.453945 |     30.875 |
| True        | 0.500743 |    642.476 |

We first classified whether or not a champion would be considered who is frequently banned, then we chose this threshold to be 100. For example, if a champion were to be banned over 300 times in our dataset, then they would be considered as a banned champion. Then we groupby this classification to see the average win rate and banned rate between these 2 groups. Champions who are classifed as banned have a higher win rate.

## Assessment of Missingness

### NMAR Analysis

In our data we believe that columns ban1, ban2, ban3, ban4, ban5 contains some NMAR(not missing at random) because in League of Legends, a team can decide to choose if they want to ban a character or pass and not ban anyone depending on their strategy for the game.

### Missingness Dependency

In this part, we are going to test if the missingness of bans columns depends on other columns. The two other columns that we used are league and bans1-5. The significance level we choose for both permutation tests is 0.5, and the test statistic is Total Variance Distance (TVD).

First, we perform the permutation test on bans and league, and the missingness of bans on league.

`Null Hypothesis`: Distribution of league when bans is missing is the same as the distribution of league when bans is not missing.

`Alternative Hypothesis`: Distribution of league when bans is missing is NOT same as the distribution of league when bans is not missing.

Below is the observed distribution of league when bans is missing and not missing with the first 15 rows of our dataframe.

| gameid    | league   |   participantid | datacompleteness   | teamname        | ban1   | ban2   | ban3   | ban4   | ban5   |   result |   nonban_rate | is_missing   |
|:----------|:---------|----------------:|:-------------------|:----------------|:-------|:-------|:-------|:-------|:-------|---------:|--------------:|:-------------|
| 2885-3123 | DCup     |             100 | partial            | JD Gaming       | True   | True   | True   | True   | True   |        0 |             1 | True         |
| 2885-3123 | DCup     |             200 | partial            | Invictus Gaming | True   | True   | True   | True   | True   |        1 |             1 | True         |
| 2885-3124 | DCup     |             100 | partial            | Invictus Gaming | False  | False  | False  | False  | False  |        1 |             0 | False        |
| 2885-3124 | DCup     |             200 | partial            | JD Gaming       | False  | False  | False  | False  | False  |        0 |             0 | False        |
| 2886-3125 | DCup     |             100 | partial            | Bilibili Gaming | False  | False  | False  | False  | False  |        0 |             0 | False        |
| 2886-3125 | DCup     |             200 | partial            | Snake Esports   | False  | False  | False  | False  | False  |        1 |             0 | False        |
| 2886-3126 | DCup     |             100 | partial            | Snake Esports   | False  | False  | False  | False  | False  |        1 |             0 | False        |
| 2886-3126 | DCup     |             200 | partial            | Bilibili Gaming | False  | False  | False  | False  | False  |        0 |             0 | False        |
| 2887-3127 | DCup     |             100 | partial            | Rogue Warriors  | False  | False  | False  | False  | False  |        0 |             0 | False        |
| 2887-3127 | DCup     |             200 | partial            | EDward Gaming   | False  | False  | False  | False  | False  |        1 |             0 | False        |
| 2887-3128 | DCup     |             100 | partial            | EDward Gaming   | False  | False  | False  | False  | False  |        1 |             0 | False        |
| 2887-3128 | DCup     |             200 | partial            | Rogue Warriors  | False  | False  | False  | False  | False  |        0 |             0 | False        |
| 2888-3129 | DCup     |             100 | partial            | Suning          | False  | False  | False  | False  | False  |        0 |             0 | False        |
| 2888-3129 | DCup     |             200 | partial            | Team WE         | False  | False  | False  | False  | False  |        1 |             0 | False        |
| 2888-3130 | DCup     |             100 | partial            | Team WE         | True   | True   | True   | True   | True   |        1 |             1 | True         |

After we performed permutation tests, we found that the observed statistic for this permutation test is: 0.33373048634043834, and the p-value is 0. The plot below shows the empirical distribution of the TVD for the test.

# Add 1st empirical graph here

Since the p-value is less than the 0.05 significance level, we reject the null hypothesis. Thus, the missingness of banned champions depends on the league column.

The second permutation test that we are performing is on bans and result, and the missingness of bans does not depend on result.

`Null Hypothesis`: Distribution of result when bans is missing is the same as the distribution of result when bans is not missing.

`Alternative Hypothesis`: Distribution of result when bans is missing is NOT same as the distribution of result when bans is not missing.

After we performed permutation tests, we found that the observed statistic for this permutation test is: 0.04082295106574599, and the p-value is 0.346. The plot below shows the empirical distribution of the TVD for the test.

# Add 2nd empirical graph here

Since our p-value .309 > 0.05, we fail to reject null hypothesis. We cannot claim that there is a relationship between missingness and result. Thus, the missingness of bans does not depend on the result column.

## Hypothesis Testing

In our hypothesis test, we aim to evaluate whether there is a significant difference in the distribution of banned vs non-banned champions and the final game outcome, specifically if teams with more banned champions tend to have a higher win rate. This investigation is crucial for understanding the relationship between the banned champions in a League of Legends match and the subsequent gameplay dynamics, particularly concerning the champions used by winning teams.

`Null`: The winrate for banned and non-banned characters is the same.

`Alternative`: The winrate for banned characters is higher than the winrate for non-banned characters.

`Test Statistic`: Difference of means between characters with high ban rates and low ban rates.

`Significance Level`: 5%

Based on the hypothesis test performed, our p-value of 0.0123 is less than the significance level of 0.05. Therefore, we reject the null hypothesis that banned and non-banned champions have the same win rate. This result suggests that there is a statistically significant difference in win rates, with banned champions, on average, having a higher win rate than non-banned champions. This finding implies that the selection of banned champions could be strategically impactful, potentially influencing the overall success of teams in League of Legends matches.

