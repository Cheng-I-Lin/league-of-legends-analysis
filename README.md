# League of Legends First Dragon Analysis

League of Legends First Dragon Analysis is a comprehensive data science project conducted at UCSD. Utilizing statistical analysis throughout the dataset, from exploratory data analysis to hypothesis testing, the project aims to create predictive models (baseline and final models), along with a concluding fairness analysis, that focuses on the investigation of the following question: 
**How does killing the first dragon affect the overall team performance, statistics, and match outcome?**

Author: Cheng-I (Alan) Lin

## Introduction
### General Information
**League of Legends**, otherwise commonly known as **LOL**, is a popular multiplayer online battle arena video game developed and published by Riot Games in 2009. 

In a regular game of LOL, 10 players are divided into two teams of 5 that compete against each other in player-versus-player (PvP) combat. Each player would be able to select a **champion**, playable character with unique abilities and differing styles of play, controlling them to eliminate opposing champions and pushing through all defensive turrets (towers) to destroy the enemy main base **Nexus**, which is the main objective of the game. To help champions progress within the game in terms of strength (damage output or defensive capabilities) or other ability based upgrades, LOL also introduces the concept of 

**monsters**

Within the game of LOL, the concept of **dragons** is a special type of monster that gives the player an unique buff depending on the type of dragon slained. For instance, killing the Inferno Dragon grants the champion 4% to 16% attack damage boost and provides 150 to 330 experience points depending on the level of the dragon, which progesses as the game goes on. 



### Row/Column Information
The relevant rows included in the dataset are

Below are relevant columns that are nececcary for analytical purposes and their descriptions
- `gameid`: This column contains an unique identifier for each individual match played in the dataset, allowing to distinguish between different matches.
- `league`: This column represents the specific league tournament where the matches are held.
- `side`: This column denotes the team within the game each participating team is associated with. In an official match, the teams are separated into "blue" and "red" teams that compete against each other.
- `result`: This column indicates the outcome of whether a team has won or lost the match, with 1 indicating a win and 0 indicating a loss.
- `kills`: This column records the total number of kills -- enemy champion eliminations -- a team has throughout the match.
- `deaths`: This column records the total number of deaths -- eliminations by enemy champions or other in game mechanisms such as minions or monsters -- a team has throughout the match.
- `assists`: This column records the number of assists -- contribtuions to enemy champion eliminations, but no direct kills -- credited to a team throughout the match.
- `firstdragon`: This column indicates the outcome of whether a team has successfully obtained the first dragon kill, with 1 representing the team with the first dragon kill and 0 representing the team without the first dragon kill.
- `dpm`: This column quantifies the amount of total damage outputted by all five players in a team per minute through basic , reflecting the . <!--Need more info-->
- `damagetakenperminute`: This column quantifies the amount of total damage taken by all five players in a team per minute, reflecting the . <!--Need more info-->
- `damagemitigatedperminute`: This column quantifies the amount of total damage mitigated by all five players in a team per minute. By mitigating damage, it means that, reflecting <!--Need more info-->
- `totalgold`: This column quantifies the total amount of gold obtained by a team throughout the match. Gold is the in game currency used for various strategic <!--Need more info-->

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
From the entire dataset, I first cleaned data to include only the columns that were mentioned above in order to narrow down the scope of the data to focus on what is necessary for the analysis: `gameid`, `league`, `side`, `result`, `kills`, `deaths`, `assists`, `firstdragon`, `dpm`, `damagetakenperminute`, `damagemitigatedperminute`, and `totalgold`.

In the original dataset, each match has 12 rows, with 10 rows representing each of the players (player rows, 5 from each of the 2 teams), and 2 rows representing the summary of the overall team performance and result (team summary rows, one for each team). I decided to keep only the team rows (2 rows excluding the 10 player rows), as they would provide the statistical details of each team and their overall team statistics that are needed for further analysis.

Next, to further condense the information present in the dataset, I created one new column, `kda`, out of the given statistics. The `kda` column represents the **Kill-Death-Assist ratio**, calculated by adding the total number of team kills and team assists and divided by the number of team deaths in a given match. This statistic would help us better determine which team "performed" the best, as a higher KDA constritutes more kills and assists on a lower number of deaths, which contributes to winning the game as a whole as the team can terminate more enemy opponents before getting eliminated.

I realized that some columns contain missing values, specifically in the columns `dpm`. Therefore, 

Below is the head of the cleaned dataframe:

| gameid                | league   | side   |   result |   kills |   deaths |   assists |   kda | firstdragon   |     dpm |   damagetakenperminute | damagemitigatedperminute   |   totalgold |
|:----------------------|:---------|:-------|---------:|--------:|---------:|----------:|------:|--------------:|--------:|-----------------------:|---------------------------:|------------:|
| ESPORTSTMNT01_2690210 | LCKC     | Blue   |        0 |       9 |       19 |        19 |  1.4  | 0             | 1981.09 |                3537.2  | 2364.7285                  |       47070 |
| ESPORTSTMNT01_2690210 | LCKC     | Red    |        1 |      19 |        9 |        62 |  8.1  | 1             | 2799.02 |                3009.67 | 2872.3292                  |       52617 |
| ESPORTSTMNT01_2690219 | LCKC     | Blue   |        0 |       3 |       16 |         7 |  0.59 | 0             | 1690.98 |                2984.02 | 3109.6121                  |       57629 |
| ESPORTSTMNT01_2690219 | LCKC     | Red    |        1 |      16 |        3 |        39 | 13.75 | 1             | 2124.55 |                2745.72 | 2868.4201                  |       71004 |
| 8401-8401_game_1      | LPL      | Blue   |        1 |      13 |        6 |        35 |  6.86 | <NA>          | 1762.02 |                2263.25 | <NA>                       |       45468 |

### Univariate Analysis
For the univariate analysis, the below plot shows the distribution of team KDA ratio in the dataset:

<iframe
  src="assets/kda-distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The histogram clearly shows a extremely right skewed distribution of team KDA ratio, indicating that most teams performed relatively "normally", with a lower KDA that's to be expected. Yet, there are some teams that performed significantly better, killing and assiting a lot more within a match while dying on a lower interval, hence creating this right skewed distribution. This is also to be expected as in some rare cases there will be matches where one team is superior than the other, and this large gap in skills and experience can lead to extreme outliers in team KDA ratio.

Another univariate analysis I performed is shown below with the distribution of team damage per minute (DPM):

<iframe
  src="assets/dpm-distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The histogram shows a nearly normal distribution with a slightly right skewed tail, indicating that the data is well-behaved as all team DPMs are relatively balanced for both team. This means that no teams had a significantly more damage output than their counterpart opponenets, which is fairly typical for such gaming scenarios. Note that although there are some outliers in team KDA distribution, there aren't really any in team DPM distribution. This is because although that both teams can deal similar amounts of damage to each other, ultimately one team is better at getting the kills or getting away with low health, thus producing a large gap in the KDA department even with similar damage output.

### Bivariate Analysis
For the bivariate analysis, the below chart shows how many teams won with the first dragon kill and how many teams lost despite obtaining the first dragon kill:

<iframe
  src="assets/win-piechart.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

According to the pie chart, teams with the first dragon kill wins the match approximately **57.7%** of the time. This significant win rate suggests that most teams that obtained the first dragon kill ultimately wins the game, implying that trying to secure a dragon kill first can be a viable strategy that could lead to an advantageous start and to overall success. 

Another bivaraiate analysis I performed is shown below with the distributions of team KDA ratios based on their respective first dragon kill status:

<iframe
  src="assets/kda-boxplot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

According to the box plots, both the team with and without the first dragon kill have similar distributions when it comes to KDA, yet teams with the first dragon kill has a higher overall KDA with highers values in the first quartile, median, and third quartile: **1.53** to 1.13, **4.07** to 2.255, and **7.29** to 5.69, respectively. This implies that obtaining the first dragon kill greatly associates with better player performances in terms of eliminating enemy champions due to the various buffs and benefits it provides, which deems securing the first dragon kill a valuable tactic to execute in game.

### Interesting Aggregates
Below are some of the aggregated statistics of the dataset:

|   result |   kills |   deaths |   assists |     kda |     dpm |   damagetakenperminute |   damagemitigatedperminute |   totalgold |
|---------:|--------:|---------:|----------:|--------:|--------:|-----------------------:|---------------------------:|------------:|
| 0.422969 | 13.6692 |  15.5542 |   30.3549 | 4.03649 | 2054.97 |                2956.4  |                    2577.55 |     56223.9 |
| 0.57706  | 15.5269 |  13.7008 |   34.526  | 5.31485 | 2141.88 |                2930.35 |                    2637.62 |     57845.8 |

By grouping the dataset by `firstdragon`, I calculated the average value of all relevant statistics. By comparing these categories with and without first dragon kill, we can easily visualize the difference in average team statistics with and without first dragon kill. From the table, it's evident that the team with the first dragon kill averages more wins, kills, assists, KDA, damages per minute, damage mitigated per minute, and total gold, all while averaging less deaths and damage taken per minute. This suggests that, on average, teams with the first dragon kill are performing better in all major gaming statistics.

## Assessment of Missingness
### NMAR Analysis
In the dataset, I believe that columns `ban1`, `ban2`, `ban3`, `ban4`, and `ban5` are likely not missing at random (NMAR). Each of these ban columns has an unique number of missing values (2334, 2202, 2520, 2400, and 2724, respectively) and does not possess any specific trends of missingness or any missingness dependency with other columns. This is likely due to the fact that in a competitive LOL match, each player gets to decide which champion he or she would like to ban before other players begin their champion selection phase. However, a player is not required to ban a champion in the given time, thus creating the missing values in the five ban columns as some players decided not to ban any champions. Therefore, the columns are NMAR because they are only missing when the players chose not to ban any champion, meaning the missingness of these values depends on the actual values themselves. To better explain the missingness of the five ban columns, making them missing at random (MAR) instead of NMAR, one additional data I would obtain is `all_bans`, which is a binary data with 1 indicating all players from a team have banned a champion, and 0 indicating at least one player in the team did not ban any champion. By adding this column of data, one can easily determine if there's a missing value in one of the five ban columns just by looking at `all_bans`, which helps explain the missingness.

### Missingness Dependency
Besides the ban columns that contain missing values, the `damagemitigatedperminute` column is also missing values ( to be exact). Therefore, I decided to test if the missingness of this column depend on other columns in the dataset, specifically the columns `league` and `side`. To test this missingness dependency, a permutation test is required. Consequently, I chose a significance level of 0.05 (5%) using **total variance distance** (TVD) as the test statistic to conduct the test.

First, let's look at the missingness dependency between the columns `damagemitigatedperminute` and `league`.

| league          |   firstdragon_missing = False |   firstdragon_missing = True |
|:----------------|------------------------------:|-----------------------------:|
| ASCI            |                    0          |                   0.0391121  |
| CBLOL           |                    0.0228041  |                   0          |
| CBLOLA          |                    0.0202703  |                   0          |
| CDF             |                    0.00713213 |                   0          |
| CT              |                    0.00243994 |                   0          |
| DCup            |                    0          |                   0.0406977  |
| DDH             |                    0.0196134  |                   0          |
| EBL             |                    0.0155781  |                   0          |
| EBLPA           |                    0.00178303 |                   0          |
| EL              |                    0.0126689  |                   0          |
| ESLOL           |                    0.0225225  |                   0          |
| EUM             |                    0.0250563  |                   0          |
| GL              |                    0.0163288  |                   0          |
| GLL             |                    0.0146396  |                   0          |
| GLLPA           |                    0.00441066 |                   0          |
| HC              |                    0.0152027  |                   0          |
| HM              |                    0.0143581  |                   0          |
| IC              |                    0.00703829 |                   0          |
| LAS             |                    0.0213026  |                   0          |
| LCK             |                    0.0438251  |                   0          |
| LCKC            |                    0.0370683  |                   0          |
| LCL             |                    0.0015015  |                   0          |
| LCO             |                    0.0198949  |                   0          |
| LCS             |                    0.0287162  |                   0          |
| LCSA            |                    0.0506757  |                   0          |
| LDL             |                    0          |                   0.497357   |
| LEC             |                    0.0228041  |                   0          |
| LFL             |                    0.0231794  |                   0          |
| LFL2            |                    0.0226164  |                   0          |
| LHE             |                    0.0228979  |                   0          |
| LJL             |                    0.0201764  |                   0          |
| LJLA            |                    0.00356607 |                   0          |
| LLA             |                    0.0175488  |                   0          |
| LMF             |                    0.03003    |                   0          |
| LPL             |                    0          |                   0.415433   |
| LPLOL           |                    0.0199887  |                   0          |
| LVP SL          |                    0.0229917  |                   0          |
| MSI             |                    0.00750751 |                   0          |
| NEXO            |                    0.0182057  |                   0          |
| NLC             |                    0.0228979  |                   0          |
| NLC Aurora Open |                    0.0131381  |                   0          |
| PCS             |                    0.0256194  |                   0          |
| PGC             |                    0.0529279  |                   0          |
| PGN             |                    0.0140766  |                   0          |
| PRM             |                    0.022241   |                   0          |
| PRMP            |                    0.0126689  |                   0          |
| SL (LATAM)      |                    0.0154842  |                   0          |
| TAL             |                    0.019238   |                   0          |
| TCL             |                    0.0209272  |                   0          |
| UL              |                    0.0228979  |                   0          |
| UPL             |                    0.0386637  |                   0          |
| USP             |                    0.00319069 |                   0          |
| VCS             |                    0.0304992  |                   0          |
| VL              |                    0.0159535  |                   0          |
| WLDs            |                    0.013232   |                   0.00739958 |

**Null Hypothesis**: The distribution of `league` when `damagemitigatedperminute` is missing is the same as the distribution of `league` when `damagemitigatedperminute` is not missing.

**Alternative Hypothesis**: The distribution of `league` when `damagemitigatedperminute` is missing is different from the distribution of `league` when `damagemitigatedperminute` is not missing.

<iframe
  src="assets/missing-league.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Next, let's look at the missingness dependency between the columns `damagemitigatedperminute` and `side`.

| side   |   side_missing = False |   side_missing = True |
|:-------|-----------------------:|----------------------:|
| Blue   |                    0.5 |                   0.5 |
| Red    |                    0.5 |                   0.5 |

**Null Hypothesis**: The distribution of `side` when `damagemitigatedperminute` is missing is the same as the distribution of `side` when `damagemitigatedperminute` is not missing.

**Alternative Hypothesis**: The distribution of `side` when `damagemitigatedperminute` is missing is different from the distribution of `side` when `damagemitigatedperminute` is not missing.

<iframe
  src="assets/missing-side.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Hypothesis Testing
To understand the relationship between first dragon kill and overall team performance, I conducted a hypothesis test that aims to determine if there's a statistically significant difference between the distribution of KDA ratios for the teams that secured the first dragon kill and the teams that did not. By doing so, one can better understand how securing the first dragon kill could lead to enhanced gameplays in terms of obtaining more kills/assists while dying a fewer number of times, which is what the KDA ratio depicts.

**Null Hypothesis**: The distribution of KDA ratios for the teams that got the first dragon kill is the same as the teams that did not get the first dragon

**Alternative Hypothesis**: The distribution of KDA ratios for the teams that got the first dragon kill is **NOT** the same as the teams that did not get the first dragon

**Test Statistic**: Absolute mean difference between the teams' KDA ratios with and without first dragon kill

**Significance Level**: 5% (0.05)

<!-- Graph -->
<iframe
  src="assets/hypothesis.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Based on the hypothesis test, which performed 500 iterations of permutation tests on the dataset, the resulting p-value is **0.0**, thus the null hypothesis is **rejected** as the p-value is lower than the significance level of **0.05**. Since there's a statistically significant result, this suggests that the distribution between KDA ratios for teams that secured the first dragon kill and teams that did not is different. Consequently, this shows 

## Framing a Prediction Problem
From the hypothesis testing, it's statistically significant to conclude that securing the first dragon kill can lead to a better team performance. Therefore, we can try to predict if a team would win or lose a game given whether they obtained the first dragon kill or not along with some other important team statistics.

## Baseline Model
Now, it's time to make a predictive baseline model that can answer our prediction question. For the baseline model, I utilized a Random Forest Classifier, which contains the following features: `kills`, `assists`, `firstdragon`, and `dpm`. In a general PvP video game of any kind, usually the player with the better offensive skills would win more frequently. Therefore, this model utlizes the features that are directly correlated to attacking outputs to predict the outcome of the match. Among all these feature, all of them are quantitative except `firstdragon`, which is a nominal categorical variable already in binary form (0 and 1) that does not require further encodings. For the rest of the quantitative features (`kills`, `assists`, and `dpm`), I performed a 

## Final Model
For the final model, I added four more features: `kda`, `damagetakenperminute`, `damagemitigatedperminute`, and `totalgold`. In a typical LOL game, it is intuitively assumed that the team who dealt the most damage or avoided the most damage would have an advantage as they would be able to eliminate more enemies while staying alive for a longer period of time. Therefore, the features `damagetakenperminute` and `damagemitigatedperminute` would provide the model with the extra information needed to determine which team has a winning advantage. Moreover, gold is crucial for the players to upgrade the champions' offense and defense. With enhanced offensive and defensive skills, one can more easily defeat enemy champions and capture opposing Nexus to win the game.

## Fairness Analysis
Even though the model mentioned above may seem to be accurate, but accuracy does not imply fairness. For a model to be fair, it needs to treat all groups of values in the same way. Therefore, this fairness analysis is conducted to answer the following question: **Does the model perform worse for teams with a KDA ratio less than or equal to 5 than it does for teams with a KDA ratio greater than 5?**

**Null Hypothesis**: The model is fair, as its accuracy for teams with a KDA ratio less than or equal to 5 is same as its accuracy for teams with a KDA ratio greater than 100

**Alternative Hypothesis**: The model is unfair, as its accuracy for teams with a KDA ratio less than or equal to 5 is **NOT** the same as the accuracy for teams with a KDA ratio greater than 5

**Test Statistic**: Difference in accuracy between teams with a KDA ratio less than or greater than 5

**Significance Level**: 5% (0.05)
