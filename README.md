# League of Legends First Dragon Analysis

League of Legends First Dragon Analysis is a comprehensive data science project conducted at UCSD. Utilizing statistical analysis throughout the dataset, from exploratory data analysis to hypothesis testing, the project aims to create predictive models (baseline and final models), along with a concluding fairness analysis, that focuses on the investigation of the following question: 
**How does killing the first dragon affect the overall team performance, statistics, and match outcome?**

Author: Cheng-I (Alan) Lin

## Introduction
### General Information
League of Legends, otherwise commonly known as **LOL**, is a 
**champion**
**champion**

### Row/Column Information
The relevant rows included in the dataset are

Below are relevant columns that are nececcary for analytical purposes and their descriptions
- `gameid`:
- `league`:
- `side`:
- `champion`:
- `result`:
- `kills`:
- `deaths`:
- `assists`:
- `dpm`:
- `damagetakenperminute`:
- `damagemitigatedperminute`:
- `totalgold`:

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
From the entire dataset, I first cleaned data to include only the columns that were mentioned above in order to narrow down the scope of the data to focus on what is necessary for the analysis: `gameid`, `league`, `side`, `champion`, `ban1`, `ban2`, `ban3`, `ban4`, `ban5`, `result`, `kills`, `deaths`, `assists`, `dpm`, `damagetakenperminute`, `damagemitigatedperminute`, and `totalgold`.

In the original dataset, each match has 12 rows, with 10 rows representing each of the players (player rows, 5 from each of the 2 teams), and 2 rows representing the summary of the overall team performance and result (team summary rows, one for each team). I decided to keep only the team rows (2 rows excluding the 10 player rows), as they would provide the statistical details of each team and their overall team statistics that are needed for further analysis.

Next, to further condense the information present in the dataset, I created one new column, `kda`, out of the given statistics. The `kda` column represents the **Kill-Death-Assist ratio**, calculated by adding the total number of team kills and team assists and divided by the number of team deaths in a given match. This statistic would help us better determine which team "performed" the best, as a higher KDA constritutes more kills and assists on a lower number of deaths, which contributes to winning the game as a whole as the team can terminate more enemy opponents before getting eliminated.

I realized that some columns contain missing values, specifically in the columns `dpm`, `damageshare`. Therefore, 

### Univariate Analysis
For the univariate analysis, the below plot shows the distribution of team KDA ratio in the dataset:
<!-- Insert graph -->
The histogram clearly shows a extremely right skewed distribution of team KDA ratio, indicating that most teams performed relatively "normally", with a lower KDA that's to be expected. Yet, there are some teams that performed significantly better, killing and assiting a lot more within a match while dying on a lower interval, hence creating this right skewed distribution. This is also to be expected as in some rare cases there will be matches where one team is superior than the other, and this large gap in skills and experience can lead to extreme outliers in team KDA ratio.

Another univariate analysis I performed is shown below with the distribution of team damage per minute (DPM):
<!-- Insert graph -->
The histogram shows a nearly normal distribution with a slightly right skewed tail, indicating that the data is well-behaved as all team DPMs are relatively balanced for both team. This means that no teams had a significantly more damage output than their counterpart opponenets, which is fairly typical for such gaming scenarios. Note that although there are some outliers in team KDA distribution, there aren't really any in team DPM distribution. This is because although that both teams can deal similar amounts of damage to each other, ultimately one team is better at getting the kills or getting away with low health, thus producing a large gap in the KDA department even with similar damage output.

### Bivariate Analysis
For the bivariate analysis, the below chart shows how many teams won with the first dragon kill and how many teams lost despite obtaining the first dragon kill:
<!-- Insert graph -->
According to the pie chart, teams with the first dragon kill wins the match approximately 57.7% of the time. This significant win rate suggests that most teams that obtained the first dragon kill ultimately wins the game, implying that trying to secure a dragon kill first can be a viable strategy that could lead to an advantageous start and to overall success.

Another bivaraiate analysis I performed is shown below with the distributions of team KDA ratios based on their respective first dragon kill status:
<!-- Insert graph -->
According to the box plots, teams with the first dragon kill has a slightly higher KDA

### Interesting Aggregates
Below are some of the aggregated statistics of the dataset:
<!-- Insert table -->
By grouping the dataset by `firstdragon`, I calculated the average value of all relevant statistics. By comparing these categories with and without first dragon kill, we can easily visualize the difference in average team statistics with and without first dragon kill. From the table, it's evident that the team with the first dragon kill averages more wins, kills, assists, damages per minute, damage mitigated per minute, and total gold, all while averaging less deaths and damage taken per minute. This suggests that, on average, teams with the first dragon kill are performing better in all major gaming statistics.

## Assessment of Missingness
### NMAR Analysis
Columns `ban1`, `ban2`, `ban3`, `ban4`, and `ban5` are likely not missing at random (NMAR).

### Missingness Dependency


## Hypothesis Testing
Null hypothesis

## Framing a Prediction Problem
From the hypothesis testing, it's statistically significant to conclude that securing the first dragon kill can lead to a better team performance. Therefore, we can try to predict if a team would win or lose a game given whether they obtained the first dragon kill or not along with some other important team statistics.

## Baseline Model
Now, it's time to make a predictive baseline model that can answer our prediction question.

## Final Model


## Fairness Analysis
