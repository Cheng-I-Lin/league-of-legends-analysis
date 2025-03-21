# League of Legends First Dragon Analysis

League of Legends First Dragon Analysis is a comprehensive data science project conducted at UCSD. Utilizing statistical analysis throughout the dataset, from exploratory data analysis to hypothesis testing, the project aims to create predictive models (baseline and final models), along with a concluding fairness analysis, that focuses on the investigation of the impacts of first dragon kill on overal team performance and game statistics. 

Author: Cheng-I (Alan) Lin

## Introduction
### General Information
**League of Legends**, otherwise commonly known as **LOL** or simply **League**, is a popular multiplayer online battle arena video game developed and published by Riot Games in 2009. Through its free-to-play accessibility, interesting character designs, and unique gameplay mechanisms, League of Legends has soon gained worldwide popularity, highly regarded as one of the greatest video games ever made. Naturally, with millions of global players on a daily basis, professional League of Legends tournaments have quickly became one of the most viewed and anticipated esports in gaming history. The dataset used in this report includes professionally collected player, team, and match statistics by Oracle’s Elixir throughout League esports matches in 2022.

In a regular game of LOL, 10 players are divided into two teams of 5 that compete against each other in player-versus-player (PvP) combat. Each player would be able to select a **champion**, playable character with unique abilities and differing styles of play, controlling them to eliminate opposing champions and pushing through all defensive turrets (towers) to destroy the enemy main base **Nexus**, which is the main objective of the game. To help champions progress within the game in terms of strength (damage output or defensive capabilities) or other ability based upgrades, LOL introduces the concept of **minions** and **monsters** which are non-playable-characters (NPC) that provide experience points (XP), gold, and a variety of other benefits to the players that slained them.

Within the game of LOL, a **dragon** is a special type of monster that gives the player an unique buff depending on the type of dragon slained. For instance, killing the Inferno Dragon grants the champion 4% to 16% attack damage boost and provides 150 to 330 experience points depending on the level of the dragon, which progesses as the game goes on. Therefore, the faster one can succesffully kill a dragon, the more advantages one possesses when facing their oponents. This is regarded as the **first dragon kill**, referring to the initial kill of any type of dragons by a team in the beginning of the match. By securing the first dragon kill, there would be an immediate and noticeable effect between the difference in strengths of the two teams, usually propelling the team with the first dragon kill toward an advantageous start.

To fully understand the impact that first dragon kill has on the entire match, the analysis would be focused around the following question: 
> **How does killing the first dragon affect the overall team performance, statistics, and match outcome?** 

Specifically, by utilizing the provided dataset, I want to statistically analyze how first dragon kill affects performance metrics, such as the KDA ratio (which would be discussed later) and damage outputs, and predict how it can impact the final result of the match. Through this predictive model, I can provide valuable information on how to enhance team productivity and elevate the level of gameplay by optimizing tatical strategies involving dragon kills, potentially amplifying team performance and increasing winning probabilities, which is crucial coming from a competitive gaming perspective.

### Row/Column Information
The dataset includes an extensive array of gaming statistics, such as player and team performance metrics in the forms of match results, number of kills, and many other important gameplay features, gathered from professional League of Legends esports tournament matches in the year 2022. Overall, there are **150588** rows and **161** columns in the dataset. However, not all rows and columns are necessary for the analytical purposes of this report (the dataset would be cleaned in the **Data Cleaning** section). The following shows the relevant columns that would be used in the latter parts of this analysis and brief descriptions of what their values represent:

- `gameid` This column contains an unique identifier for each individual match played in the dataset, allowing one to distinguish between different matches.

- `league` This column represents the specific league tournament where the matches are held.

- `side` This column denotes the team within the game each participating team is associated with. In an official match, the teams are separated into "blue" and "red" teams that compete against each other.

- `result` This column indicates the outcome of whether a team has won or lost the match, with 1 indicating a win and 0 indicating a loss.

- `kills` This column records the total number of kills -- enemy champion eliminations -- a team has throughout the match.

- `deaths` This column records the total number of deaths -- eliminations by enemy champions or other in game mechanisms such as minions or monsters -- a team has throughout the match.

- `assists` This column records the number of assists -- contribtuions to enemy champion eliminations, but no direct kills -- credited to a team throughout the match.

- `firstdragon` This column indicates the outcome of whether a team has successfully obtained the first dragon kill, with 1 representing the team with the first dragon kill and 0 representing the team without the first dragon kill.

- `dpm` This column, damage per minute (DPM), quantifies the amount of total damage outputted by all five players in a team per minute through basic attacks, ability based attacks, or other offensive means, reflecting the offensive capabilities of each team. With a higher team dpm, it generally indicates a better offensive performance from the team that would result in fruitful outcomes.

- `damagetakenperminute` This column quantifies the amount of total damage taken by all five players in a team per minute through enemy champion attacks, monster attacks, or other opposing offensive attacks, reflecting the performance or defensive capabilities of each team. With fewer damage taken, the champions are more likely to survive, thus generally indicating a better performance. However, more damage taken doesn't necessary always imply a worse performance as there are champions with large sums of health designed with the sole purpose to "tank" attacks so as to protect other high damage champions. Therefore, taking damage in this sense can help increase the overall team defense. 

- `damagemitigatedperminute` This column quantifies the amount of total damage avoided by all five players in a team per minute through reducing or blocking damage with champion abilities, shields, or other defensive equipment or item based means, reflecting the defensive capabilities of each team. With a higher damage mitigated per minute, champions are more likely to survive as they can alleviate some or most of the received damage, thus generally indicating a better performance.

- `totalgold` This column quantifies the total amount of gold obtained by a team throughout the match. Gold is the in-game currency used for purchasing items in the shop that provide champions with bonus stats and abilities, which is one of the main ways a player can power up throughout the course of the match. At the beginning of each game, the champions are given starting gold based on the map being played on, and can receive more gold through several methods. One method is the automatic gold generation, where every champion passively generates gold throughout the game. Although the ouput of the gold is slow, this method is guaranteed to continually proceed during the full length of the match. Another method is by killing in-game units, such as minions, monsters, and enemy champions, which grants a varying amount of golds. Therefore, the more total gold a team possesses generally indicates a better overall performance as the team can more easily upgrade their champions' offensive, defensive, and other ability based capabilities, leading to an overall domination in terms of the degree of gap between the quantity and quality of items bought.

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
From the entire dataset, I first cleaned the data to include only the columns that were mentioned above in order to narrow down the scope of the data to focus on what is necessary for the analysis: `gameid`, `league`, `side`, `result`, `kills`, `deaths`, `assists`, `firstdragon`, `dpm`, `damagetakenperminute`, `damagemitigatedperminute`, and `totalgold`.

In the original dataset, each match has 12 rows, with 10 rows representing each of the players (player rows, 5 from each of the 2 teams), and 2 rows representing the summary of the overall team performance and result (team summary rows, one for each team). I decided to keep only the team rows (2 rows excluding the 10 player rows), as they would provide the statistical details of each team and their overall team statistics that are needed to answer the central question for further analysis.

Next, to further condense the information present in the dataset, I created one new column, `kda`, out of the given statistics. The `kda` column represents the **Kill-Death-Assist ratio**, calculated by adding the total number of team kills and team assists and divided by the number of team deaths in a given match (the number of deaths is incremented by one to avoid division by zero error if a team has zero deaths, which is unlikely but definitely possible). This statistic would help us better determine which team "performed" the best, as a higher KDA constitutes to more kills and assists on a lower number of deaths, which contributes to winning the game as a whole as the team can terminate more enemy opponents before getting eliminated. Therefore, to determine the performance of the team in terms of killing enemy champions, one can just look at the KDA ratio column to get a better representation instead of looking at the three columns of kills, assists, and deaths.

After the `kda` column is implemented, I realized that some columns contain missing values, such as in the columns `dpm` and `damagetakenperminute`, which are missing two values each in the same two rows. Remember, since I already cleaned the dataset to contain only the team rows, two missing rows correspond to one specific match as there are two teams per match (thus two team rows). This means that there's only one game where both the data on `dpm` and `damagetakenperminute` are not recorded. Since this is only one instance of such missing values, I decided to drop both rows (drop the entire match) as there are still multiple matches left to analyze, hence one dropped match would not have pose any significant impacts on the overall analysis. Note that there are other missing values in the dataset, notably in the columns `firstdragon` and `damagemitigatedperminute`, yet they are missing too many values to simply be dropped and they lack other statistics that can help me impute valid values into these missing data. Therefore, I decided to keep these missing rows for now and analyze their missingness in the latter parts of the report in the **Assessment of Missingness** section.

Below is the head (first five entries) of the cleaned dataframe that contains all the needed data values that would be further utilized in the hypothesis testing and predictive model sections of the analysis:

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

According to the box plots, both the team with and without the first dragon kill have similar distributions when it comes to KDA, yet teams with the first dragon kill has a higher overall KDA with highers values in the first quartile, median, and third quartile: **1.53** to 1.13, **4.07** to 2.255, and **7.29** to 5.69, respectively. This implies that obtaining the first dragon kill greatly associates with better team performances in terms of eliminating enemy champions due to the various buffs and benefits it provides, which deems securing the first dragon kill a valuable tactic to execute in game.

### Interesting Aggregates
Below are some of the aggregated statistics of the dataset:

|   firstdragon |   result |   kills |   deaths |   assists |     kda |     dpm |   damagetakenperminute |   damagemitigatedperminute |   totalgold |
|--------------:|---------:|--------:|---------:|----------:|--------:|--------:|-----------------------:|---------------------------:|------------:|
|             0 | 0.422969 | 13.6692 |  15.5542 |   30.3549 | 4.03649 | 2054.97 |                2956.4  |                    2577.55 |     56223.9 |
|             1 | 0.57706  | 15.5269 |  13.7008 |   34.526  | 5.31485 | 2141.88 |                2930.35 |                    2637.62 |     57845.8 |

By grouping the dataset by `firstdragon`, I calculated the average value of all relevant statistics. By comparing these categories with and without first dragon kill, we can easily visualize the difference in average team statistics with and without first dragon kill. From the table, it's evident that the team with the first dragon kill averages more wins, kills, assists, KDA, damages per minute, damage mitigated per minute, and total gold, all while averaging less deaths and damage taken per minute. This suggests that, on average, teams with the first dragon kill are performing better in all major gaming statistics.

## Assessment of Missingness
### NMAR Analysis
In the dataset, I believe that columns `ban1`, `ban2`, `ban3`, `ban4`, and `ban5` are likely **not missing at random (NMAR)**. Each of these ban columns has an unique number of missing values (2334, 2202, 2520, 2400, and 2724, respectively) and does not possess any specific trends of missingness or any missingness dependency with other columns. This is likely due to the fact that in a competitive LOL match, each player gets to decide which champion he or she would like to ban before other players begin their champion selection phase. However, a player is not required to ban a champion in the given time, thus creating the missing values in the five ban columns as some players decided not to ban any champions. Therefore, the columns are **NMAR** because they are only missing when the players chose not to ban any champion, meaning the missingness of these values depends on the actual values themselves. 

To better explain the missingness of the five ban columns, making them **missing at random (MAR)** instead of NMAR, one additional data I would obtain is `all_bans`, which is a binary data with 1 indicating all players from a team have banned a champion, and 0 indicating at least one player in the team did not ban any champion. By adding this column of data, one can easily determine if there's a missing value in one of the five ban columns just by looking at `all_bans`, which helps explain the missingness.

### Missingness Dependency
Besides the ban columns that contain missing values, the `firstdragon` column is also missing values (3784 missing rows to be exact). Therefore, I decided to test if the missingness of this column depend on other columns in the dataset, specifically the columns `league` and `side`. To test this missingness dependency, a permutation test is required. Consequently, I chose a significance level of 0.05 (5%) using **total variance distance (TVD)** as the test statistic to conduct the test.

First, let's look at the observed missingness dependency between the columns `firstdragon` and `league`.

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

Then, I ran a permutation test using the following hypotheses and plotted the empirical distribution of the TVD for the test:

Null Hypothesis
: The distribution of `league` when `firstdragon` is missing is the same as the distribution of `league` when `firstdragon` is not missing.

Alternative Hypothesis
: The distribution of `league` when `firstdragon` is missing is different from the distribution of `league` when `firstdragon` is not missing.

<iframe
  src="assets/missing-league.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

As a result, the p-value of the permutation test came out to be **0.0** with the observed test statitic being **0.992600422832981**. Since the p-value is lower than the 0.05 significance level, we **reject** the null hypothesis. This suggests that the missingness of `firstdragon` depends on the corresponding values of `league`. Therefore, one can assume that some leagues did not consistently record the data of first dragon kill status, or did not wish to announce those data publicly, leading to this missingness dependency with the specific leagues the League matches are being held at.

Next, let's look at the observed missingness dependency between the columns `firstdragon` and `side`.

| side   |   side_missing = False |   side_missing = True |
|:-------|-----------------------:|----------------------:|
| Blue   |                    0.5 |                   0.5 |
| Red    |                    0.5 |                   0.5 |

Again, I ran a permutation test using the following hypotheses and plotted the empirical distribution of the TVD for the test:

Null Hypothesis
: The distribution of `side` when `damagemitigatedperminute` is missing is the same as the distribution of `side` when `damagemitigatedperminute` is not missing.

Alternative Hypothesis
: The distribution of `side` when `damagemitigatedperminute` is missing is different from the distribution of `side` when `damagemitigatedperminute` is not missing.

<iframe
  src="assets/missing-side.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

As a result, the p-value of the permutation test came out to be **1.0** with the observed test statitic being **0.0**. Since the p-value is significantly larger than the 0.05 significance level, we **failed to reject** the null hypothesis. This suggests that the missingness of `firstdragon` does not depend on the corresponding values of `side`. Therefore, one can assume that the side the team is affliated with has nothing to do with the missing values of first dragon kill status.

## Hypothesis Testing
To understand the relationship between first dragon kill and overall team performance, I conducted a permutation test that aims to determine if there's a statistically significant difference between the distribution of KDA ratios for the teams that secured the first dragon kill and the teams that did not. By doing so, one can better understand how securing the first dragon kill could lead to better gameplays in terms of obtaining more kills and assists while dying a fewer number of times, which is what the KDA ratio depicts. Below is the hypotheses that are being tested, along with the resulting histogram containing the distribution of the test statistics:

Null Hypothesis
: The distribution of KDA ratios for the teams that got the first dragon kill is the same as the teams that did not get the first dragon

Alternative Hypothesis
: The distribution of KDA ratios for the teams that got the first dragon kill is **NOT** the same as the teams that did not get the first dragon

Test Statistic
: Absolute mean difference between the teams' KDA ratios with and without first dragon kill (shows how the two distributions differ from one another)

Significance Level
: 5% (0.05)

<iframe
  src="assets/hypothesis.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Based on the permutation test, which performed 500 iterations of permutations on the dataset, the resulting p-value is **0.0**, thus we **reject** the null hypothesis as the p-value is lower than the significance level of **0.05**. Since there's a statistically significant result, this suggests that the distribution between KDA ratios for teams that secured the first dragon kill and teams that did not is different. Consequently, it demontrates that first dragon kills may have a significant positive impact on the teams' performances in terms of an increased KDA ratio. Therefore, a viable strategy team could incorporate into their game plan is to secure the dragon kills first in order to potentially increase the chances of winning.

## Framing a Prediction Problem
From the hypothesis testing, it's statistically significant to conclude that securing the first dragon kill can lead to a better team performance. This increase in team production is crucial in deciding the outcome of the match. Therefore, we can try to predict the following problem:
> **Would a team win or lose a game given whether they obtained the first dragon kill or not along with some other important team and match statistics?**

Since this is a **binary classification problem** as the model is trying to classify winning and losing matches, the response variable (the variable that the model will try to predict) will be `result`. With the `result` column already in the binary format of zeros and ones, there's no need to one-hot encode the values. Below is the head of the dataframe with all the information necessary for the training, analysis, and testing phases of the predictive model, which is just the same dataset as the one shown in the **Data Cleaning** section of the report excluding some irrelevant categorical variables:

|    |   result |   kills |   deaths |   assists |   kda | firstdragon   |     dpm |   damagetakenperminute | damagemitigatedperminute   |   totalgold |
|---:|---------:|--------:|---------:|----------:|------:|--------------:|--------:|-----------------------:|---------------------------:|------------:|
|  0 |        0 |       9 |       19 |        19 |  1.4  | 0             | 1981.09 |                3537.2  | 2364.7285                  |       47070 |
|  1 |        1 |      19 |        9 |        62 |  8.1  | 1             | 2799.02 |                3009.67 | 2872.3292                  |       52617 |
|  2 |        0 |       3 |       16 |         7 |  0.59 | 0             | 1690.98 |                2984.02 | 3109.6121                  |       57629 |
|  3 |        1 |      16 |        3 |        39 | 13.75 | 1             | 2124.55 |                2745.72 | 2868.4201                  |       71004 |
|  4 |        1 |      13 |        6 |        35 |  6.86 | <NA>          | 1762.02 |                2263.25 | <NA>                       |       45468 |

To build a predictive model, one would need to first train the model on some sets of training data, fitting the model to those data so that it can distinguish and learn the differences between each value. Once it's able to learn the correlation and predict values within the training dataset, the model would then be tested on a separate array of values of testing data. Since the model was already trained and fitted to a similar sample of data, it should also provide similarly accurate predictions for the testing data. However, just because a model can accurately predict training data, it doesn't necessarily mean that it can generalize and work well on similar, unseen samples from the same population. This is because models can **overfit** where they are too complicated with high variance of predictions, while models can also **underfit** where they are too basic to capture the relationship between the features and the response variable. Therefore, choosing the right proportion of training and testing data is extremely important as to avoid these problems.

To avoid underfitting, I need to use a large dataset with multiple features for training and testing the model, which is already given as the current dataset contain a variety of information on League matches, so as to decrease bias. To avoid overfitting and high variance, I decided to use 75% training data and 25% testing data, which are the default parameters for the function `train_test_split` that's used to split the data. Finally, to evaluate the model, I will only use the accuracy score without the F1-score. This is because the dataset is very balanced as a match can only either be a win (1) or a loss (0), so the distribution of wins and losses is the same as there are no more wins than losses or more losses than wins. That being said, I would still provide the F1-score along with the **confusion matrix**, a table that shows the value of **recall** (proportion of actually winning matches that are correctly classified) and **precision** (proportion of predicted winning matches that are correctly classified), to provide more details on the model's ability to identify all relevant instances and distinguish the false positives (note that only **accuracy** would be used as the main model evaluation metric).

## Baseline Model
Now, it's time to make a predictive baseline model that can answer our prediction question. For the baseline model, I utilized a Random Forest Classifier, which contains the following features: 
1. `kills`
2. `assists`
3. `firstdragon`
4. `dpm`

In a general PvP video game of any kind, usually the players with the better offensive skills would win more frequently. Therefore, this model utlizes the features that are directly correlated to attacking outputs to predict the outcome of the match (`firstdragon` correlates to better performance in terms of KDA ratio as described in the **Hypothesis Testing** section). Among all these feature, all of them are quantitative except `firstdragon`, which is a nominal categorical variable already in binary form (0 and 1) that does not require further encodings. For the rest of the quantitative features (`kills`, `assists`, and `dpm`), I performed a StandardScaler Transformer before fitting them to the model. This is because every match has a different length of game time, thus naturally, the games that were played longer would make sense to have a higher quantity of numerical variables like `kills` and `assists`. Therefore, by standardizing the values, we can treat all features across different lengths of time under the same metrics and scales.

Using the hyperparameters of `max_depth = 2` and `n_estimators = 100`, the fitted model scored an accuracy of approximately **0.8371**, meaning that the model is able to accurately predict the correct outcome (win/lose) of the match **83.71%** of the time. Furthermore, the F1-score of the model turns out to be approximately *0.8406* with a precision of *0.8049* and a recall of *0.8795*, calculated from the confusion matrix shown below. 

|                  |   Predicted Losing |   Predicted Winning |
|:-----------------|-------------------:|--------------------:|
| Acutally Losing  |               2557 |                 653 |
| Actually Winning |                369 |                2695 |

Although the outcome for accuracy is not terrible, there are definitely rooms for improvement for the model. Therefore, in the next section, I will be adding more features as well as finding the best hyperparameters in order to improve the predictive model.

## Final Model
For the final model, I added four more quantitative features: 
1. `kda`
2. `damagetakenperminute`
3. `damagemitigatedperminute`
4. `totalgold`

By adding `kda` as a new feature, I also removed the features of `kills` and `assists` which were used in the previous baseline model. This is because I believe that the KDA ratio can better explain the performance of teams as it includes the average enemy champion elimination rate per death by accounting for the total number of deaths of the team. Since KDA is calculated from both kills and assists, there's no need to incorporate them into the model as they are both correlated with KDA. In other words, the KDA ratio of the team is a more encompassing and comprehensive feature that can replace `kills` and `assists` while also bringing in the factors of `deaths`.

In a typical LOL game, it is intuitively assumed that the team who dealt the most damage or avoided the most damage would have an advantage as they would be able to eliminate more enemies while staying alive for a longer period of time. Therefore, the features `damagetakenperminute` and `damagemitigatedperminute` would provide the model with the extra information needed to determine which team has a winning advantage. Moreover, gold is extremely crucial for the players to upgrade the champions' offense and defense, as one can more easily defeat enemy champions and capture opposing Nexus to win the game with enhanced buffs, abilities, and items. Thus, `totalgold` would be able to provide the model with an evaluation metric on how much each team has progressed in terms of strength and item quality. With the new features, I also performed StandardScaler Transformer on them to standardize the values of these quantitative variables.

Futhermore, to more accurately predict the results of the match, I need to specify the best hyperparameters for the model, which are `max_depth` and the `n_estimators` for the random forest classifier. Using `GridSearchCV`, a technique for finding the optimal parameter values, I tested a range of `max_depth` from 2 to 200 with 20 steps each and a range of `n_estimators` from 2 to 100 with 2 steps each. With this algorithm, I found that the best hyperparameters are `max_depth = 22` and `n_estimators = 62`, which would then be implemented in the new final model.

Therefore, using the hyperparamters mentioned above, the fitted model scored an accuracy of approximately **0.9529**, meaning that the model is able to accurately predict the correct outcome (win/lose) of the match **95.29%** of the time. Furthermore, the F1-score of the model turns out to be approximately *0.9521* with a precision of *0.9470* and a recall of *0.9572*, calculated from the confusion matrix shown below.

|                  |   Predicted Losing |   Predicted Winning |
|:-----------------|-------------------:|--------------------:|
| Acutally Losing  |               3046 |                 164 |
| Actually Winning |                131 |                2933 |

As you can clearly see, there have been huge improvements in the predictive power of the model as the accuracy of the final model has drastically increased from that of the baseline model. This suggests that the adjustments made in this section were a success, as they effectively improved the accuracy score.

## Fairness Analysis
Even though the model mentioned above may seem to be accurate, but accuracy does not imply fairness. For a model to be fair, it needs to treat all groups of values in the same way. Therefore, this fairness analysis is conducted to answer the following question: 
> **Does the model perform worse for teams with a KDA ratio less than or equal to 5 than it does for teams with a KDA ratio greater than 5?** 

To answer this question, I performed a permutation test on the two different groups, with *Group X*  representing the teams with a KDA ratio less than or equal to 5 and *Group Y*  representing the teams with a KDA ratio greater than 5. Using accuracy as the evaluation metric, below is the hypotheses that are being tested, along with the resulting histogram containing the distribution of the test statistics:

Null Hypothesis
: The model is fair, as its accuracy for teams with a KDA ratio less than or equal to 5 is same as its accuracy for teams with a KDA ratio greater than 5

Alternative Hypothesis
: The model is unfair, as its accuracy for teams with a KDA ratio less than or equal to 5 is **NOT** the same as the accuracy for teams with a KDA ratio greater than 5

Test Statistic
: Difference in accuracy between teams with a KDA ratio less than or greater than 5

Significance Level
: 5% (0.05)

<iframe
  src="assets/fairness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Based on the permutation test, the resulting p-value is **0.0**, thus we **reject** the null hypothesis as the p-value is lower than the significance level of **0.05**. This suggests that the model I fitted and used predicted the match outcome of both groups with different accuracies, showing some bias toward *Group Y*  as the observed test statistic of accuracy of roughly **0.067** is significantly higher than the rest of the permutated data. This means that the model predicted better for teams with a KDA ratio greater than 5 (*Group Y* ) as the accuracy score is statistically higher than that of teams with a KDA ratio less than or equal to 5 (*Group X* ). Therefore, even though the observed accuracies for both groups are extremely high (*Group X*  has an accuracy of about 0.929 while *Group Y*  has an accuracy of about 0.996) and that the difference in accuracy is only about **7%**, the model appears to be unfair based on this sepecific criteria of KDA ratios.

