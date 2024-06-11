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

Subsequently, we divided the dataset into two separate dataframes: one for teams and another for players. This division was based on the 'participantid' column as player IDs range from 1 to 10, whereas team IDs take the value 100 or 200. This distinction allows us to analyze the data in the scope of the team and the player separately. During this process, we removed certain columns with missing data, such as the 'champions' column from the team dataframe, as by design, a *team* cannot select a champion.

#### Team Dafaframe:

| league   | gameid    |   participantid | datacompleteness   | teamname            | ban1    | ...     | ban5     | pick1     | ...    | pick5      |   result |
|:---------|:----------|----------------:|:-------------------|:--------------------|:--------|:--------|:---------|:----------|:-------|:-----------|---------:|
| LPL      | 2899-3157 |             100 | complete           | Invictus Gaming     | Azir    | ...     | Vladimir | Ornn      | ...    | Tahm Kench |        1 |
| LPL      | 2899-3157 |             200 | complete           | Royal Never Give Up | Zoe     | ...     | Alistar  | Ryze      | ...    | Maokai     |        0 |
| LPL      | 2899-3158 |             100 | complete           | Royal Never Give Up | Zoe     | ...     | Lee Sin  | Jarvan IV | ...    | Tristana   |        1 |
| LPL      | 2899-3158 |             200 | complete           | Invictus Gaming     | Camille | ...     | Jhin     | Kalista   | ...    | Maokai     |        0 |
| LPL      | 2899-3159 |             100 | complete           | Invictus Gaming     | Azir    | ...     | Vladimir | Ornn      | ...    | Leona      |        0 |

#### Player Dataframe:

| league   | gameid    |   participantid | datacompleteness   | teamname        | champion   |   result |
|:---------|:----------|----------------:|:-------------------|:----------------|:-----------|---------:|
| LPL      | 2899-3157 |               1 | complete           | Invictus Gaming | Ornn       |        1 |
| LPL      | 2899-3157 |               2 | complete           | Invictus Gaming | Kha'Zix    |        1 |
| LPL      | 2899-3157 |               3 | complete           | Invictus Gaming | Orianna    |        1 |
| LPL      | 2899-3157 |               4 | complete           | Invictus Gaming | Ezreal     |        1 |
| LPL      | 2899-3157 |               5 | complete           | Invictus Gaming | Tahm Kench |        1 |

### Univariate Analysis

We executed univariate analysis on every champions' winrate in this dataset using the Player Dataframe.

<iframe
  src="assets/Champion_Win_Rate.html"
  width="1250"
  height="600"
  frameborder="0"
></iframe>

This bar chart displays the win rate as a ratio for each character selected by players. As we can see, most champions hover around 0.50 winrate meaning that they equally win and lose games. There are some notable exceptions however, such as Lux with a 0.83 winrate and Garen with a $0.00$ winrate. Note that winrate hides the number of games that a Champion has played in. For example, Garen was only seen in one game, of which was a loss.


We also made a bar chart for the amount of games where champions were banned in this data set using the Team Dataframe. 

<iframe
  src="assets/Banned_Champion_Counts.html"
  width="1250"
  height="600"
  frameborder="0"
></iframe>

This bar chart shows the number of times each champion has been banned. Camillie, Taliyah, and Zoe are the most frequently banned champions, with each over 2000 games in which they were banned.

### Bivariate Analysis

We permformed bivariate analysis on the pick counts, ban counts, and win rate of the champions in the dataset to visualize in the form a scatter plot.

<iframe
  src="assets/Pick_vs_Ban_vs_Win_Rate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Note that we log-scaled both axes to better visualize the data. Had we not, the majority of the data would be in the bottom left corner with some outliers to the top right. According to our plot, there is a postive correlation between pick rate and ban rate of the champions. Champions with high pick and ban rate also have an average winrate of about 50%, indicated by the homegeneous color of the data points. On the contrary, unpopular champions tend to have much higher variance in winrate, with a more diverse color palette. Another example of regression towards the mean.

### Interesting Aggregates
Aggregate DataFrame:

| is_banned   |   result |   banrates |
|:------------|---------:|-----------:|
| False       | 0.453945 |     30.875 |
| True        | 0.500743 |    642.476 |

To classify whether or not a champion would be considered as someone who is frequently banned, we arbitrarily decided that if a champion reaches threshold of 100 bans, they would be considered frequently banned. For example, if a champion were to be banned over 300 times in our dataset, then they would be considered as a banned champion. Then we grouped by this classification to see the average win rate and banned rate between these 2 groups. From the table, we see that champions who are classifed as banned have a higher win rate than those who aren't as frequently banned.

## Assessment of Missingness

### NMAR Analysis

In our data we believe that columns `ban1`, `ban2`, `ban3`, `ban4`, `ban5` contains some NMAR(not missing at random) because in League of Legends, a team can decide to choose if they want to ban a character or pass and not ban anyone depending on their strategy for the game.

### Missingness Dependency
#### Ban Missingness on League

In this part, we are going to test if the missingness of bans columns depends on other columns. The two other columns that we used are `league` and `bans1-5`. The significance level we choose for both permutation tests is 0.05, and the test statistic is Total Variance Distance (TVD).

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

<iframe
  src="assets/Permuted_Distribution_of_TVDs_of_Bans_Missingness_and_League.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since the p-value is less than the 0.05 significance level, we reject the null hypothesis. Thus, the missingness of banned champions depends on the league column.

#### Ban Missingness on Result
The second permutation test that we are performing is on bans and result.

`Null Hypothesis`: Distribution of result when bans is missing is the same as the distribution of result when bans is not missing.

`Alternative Hypothesis`: Distribution of result when bans is missing is NOT same as the distribution of result when bans is not missing.

After we performed permutation tests, we found that the observed statistic for this permutation test is: 0.04082295106574599, and the p-value is 0.346. The plot below shows the empirical distribution of the TVD for the test.

<iframe
  src="assets/Permuted_Distribution_of_TVDs_of_Missingness_of_Bans_and_Wins.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since our p-value .309 > 0.05, we fail to reject null hypothesis. We cannot claim that there is a relationship between missingness and result. Thus, the missingness of bans does not depend on the result column.

## Hypothesis Testing

We saw in the interesting aggregates section that the winrate of frequently banned champions is a bit higher than that of less-frequently banned champions. To see if this difference is significant, we will conduct a one-sided hypothesis test.

`Null`: The winrate for banned and non-banned characters is the same.

`Alternative`: The winrate for banned characters is higher than the winrate for non-banned characters.

`Test Statistic`: Difference of means between characters with high ban rates and low ban rates.

`Significance Level`: 5%

<iframe
  src="assets/Permuted_Distribution_of_Differences_of_Mean_Wins_and_NonBan_rate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Based on the hypothesis test performed, our p-value of 0.0123 is less than the significance level of 0.05. Therefore, we reject the null hypothesis that banned and non-banned champions have the same win rate. This result suggests that there is a statistically significant difference in win rates, with banned champions, on average, having a higher win rate than non-banned champions. This finding implies that the selection of banned champions could be strategically impactful, potentially influencing the overall success of teams in League of Legends matches.

## Framing a Prediction Problem

`Prediction Problem`: Are we able to predict the league based on game statistics? - Classification Problem

`Type of Prediction Problem`: Multiclass Classification

We are attempting to predict the league a game belongs to based on various game statistics. This is a multiclass classification problem because the target variable, League, can take on more than two distinct categories.

`Response Variable`: League
Reason for Selection: The primary objective is to determine the league classification from the given game statistics. Therefore, the League variable is the target variable we aim to predict.

`Metric`: Accuracy
Reason for Selection:
Ease of Interpretation: Accuracy is a straightforward metric that represents the proportion of correct predictions out of the total predictions. This simplicity makes it easy to interpret and communicate the model's performance.

`Justification of Features`

At the time of prediction, we only know the following information for each league: teamkills, teamdeaths, ckpm, gamelength, team kpm. These are all the statistics collected during the game. We will train our model based on the above features.

## Baseline Model

This is the first 5 rows of the dataframe that we are using in this section:

| league   |   teamkills |   teamdeaths |   ckpm |   gamelength |   team kpm |
|:---------|------------:|-------------:|-------:|-------------:|-----------:|
| LPL      |          12 |            4 | 0.6038 |         1590 |     0.4528 |
| LPL      |           4 |           12 | 0.6038 |         1590 |     0.1509 |
| LPL      |          13 |            3 | 0.5707 |         1682 |     0.4637 |
| LPL      |           3 |           13 | 0.5707 |         1682 |     0.107  |
| LPL      |          11 |           21 | 0.8939 |         2148 |     0.3073 |

Our baseline model is a decision tree with 6 features: league, teamkills, teamdeaths, ckpm, gamelength, and team kpm, and 2 hyperparameters: max_depth=50, criterion='entropy'. All 6 of our features were quantitative and we did not perform any encodings since we didn't have any categorical features. We chose max_depth = 50 because setting a maximum depth for the decision tree helps control overfitting. A deeper tree can model the training data more precisely, but it might also capture noise, leading to overfitting. A max_depth of 50 is relatively deep, allowing the tree to capture complex patterns in the data.

After fitting the model, our accuracy score on the training data is 0.4434. This means that our model is able to correctly predict 44.34% of data. The accuracy is what it is due to our data being unbalanced. Our model still has large improvement space, and we will improve it through adding more features, and tuning hyperparameters in the next section.

## Final Model

In our final model, we introduced two additional features: teamname and dragons. We included these features because in League of Legends (LOL), the team name can be significant for predicting the league, as each league has a specific set of teams, making their names potentially helpful. Additionally, the number of dragons killed during a game is a crucial factor in determining the game's outcome. We aim to assess whether higher leagues tend to kill more or fewer dragons and how this affects league prediction.

Our final model employs a Random Forest Classifier, consistent with our baseline model. The two newly added features include one categorical feature (team name) and one quantitative feature (dragons), for which we used OneHotEncoder to perform the necessary encodings. For hyperparameter tuning, we focused on three parameters: max depth, max features, and the number of estimators for the random forest classifier.

We chose to tune max_depth because it controls the maximum depth of the tree. Limiting the tree's depth can help reduce overfitting, especially when the training data contains many features or noise. By exploring different values for max_depth, we aim to find the optimal depth that balances model complexity and performance, ensuring the model generalizes well to unseen data.

We decided to tune max_features because it controls the number of features considered when looking for the best split at each node. This can help reduce overfitting and improve the model's performance by introducing more randomness into the model-building process. By experimenting with strategies like 'sqrt' or 'log2', we aim to determine the optimal number of features for the model to consider, enhancing accuracy and robustness.

We opted to tune n_estimators because it determines the number of trees in the forest. Increasing the number of trees generally improves model performance by reducing variance and helping the model generalize better. However, more trees also result in longer training times and potentially diminishing returns beyond a certain point. By trying different values for n_estimators, we aim to strike a balance between model performance and computational efficiency.

After conducting a GridSearchCV, the best hyperparameters we identified were:

- 'randomforestclassifier__max_depth': None
- 'randomforestclassifier__max_features': 'log2'
- 'randomforestclassifier__n_estimators': 300

With these hyperparameters, our model achieved an accuracy score of 0.9212, correctly predicting 92.12% of the data. This substantial improvement in evaluation metrics suggests that our model adjustments have effectively enhanced its predictive power.

## Fairness Analysis

In this section, we aim to evaluate the fairness of our model across different groups. Specifically, we seek to determine: `Does my model perform worse for teams that have killed 3 or fewer dragons during a game compared to teams that have killed more than 3 dragons during a game?` To address this question, we conducted a permutation test and analyzed the resulting difference in accuracy between the two groups.

The group X represents the players who have dragon kills less than or equal to 3, and group Y represents those who have dragon kills greater than 3. Our evaluation metric is accuracy, and the significance level is 0.05.

The followings are our hypothesis:

`Null hypothesis`: Our model is fair. Its accuracy for teams who have less than or equal to 3 dragon kills is same as the accuracy for teams who have greater than 3 dragon kills.

`Alternative hypothesis`: Our model is unfair. Its accuracy for teams who have less than or equal to 3 dragon kills is NOT same as the accuracy for teams who have greater than 3 dragon kills.

`Test statistic`: difference in accuracy between teams who have dragon kills less than, equal to, or greater than 3.

After conducting the permutation test, we obtained a p-value of 0.498, which is greater than the 0.05 significance level. As a result, we fail to reject the null hypothesis. This indicates that our model predicts teams from both groups with statistically similar accuracy levels. Therefore, our model appears to be fair, showing no discernible bias towards one group over the other.
