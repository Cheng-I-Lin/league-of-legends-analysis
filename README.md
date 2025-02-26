# League of Legends Strongest Champion Analysis

League of Legends Strongest Champion Analysis is a comprehensive data science project conducted at UCSD. Utilizing statistical analysis throughout the dataset, from exploratory data analysis to hypothesis testing, the project aims to create predictive models (baseline and final models), along with a concluding fairness analysis, that focuses on the investigation of the following questions: 
1. Which of the League of Legends Champion/Character is the "strongest" in competitive tournament matches? 
2. What are its overall impacts on match statistics and outcomes?

Author: Cheng-I (Alan) Lin

## Introduction
### General Information

League of Legends, otherwise commonly known as LOL, is a 
**champion**

### Row/Column Information
The relevant rows included in the dataset are

Below are relevant columns that are nececcary for analytical purposes and their descriptions
- `gameid`:
- `league`:
- `side`:
- `champion`:
- `ban1`:
- `ban2`:
- `ban3`:
- `ban4`:
- `ban5`:
- `result`:
- `kills`:
- `deaths`:
- `assists`:
- `dpm`:
- `damageshare`:
- `damagetakenperminute`:
- `damagemitigatedperminute`:
- `totalgold`:

## Data Cleaning and Exploratory Data Analysis

From the entire dataset, I first cleaned data to include only the columns that were mentioned above in order to narrow down the scope of the data to focus on what is necessary for the analysis: `gameid`, `league`, `side`, `champion`, `ban1`, `ban2`, `ban3`, `ban4`, `ban5`, `result`, `kills`, `deaths`, `assists`, `dpm`, `damageshare`, `damagetakenperminute`, `damagemitigatedperminute`, and `totalgold`.
In the original dataset, each match has 12 rows, with 10 rows representing each of the players (player rows, 5 from each of the 2 teams), and 2 rows representing the summary of the overall team performance and result (team summary rows, one for each team). I decided to keep only the player rows (10 rows excluding the 2 team rows), as they would provide the statistical details of each player and their chosen champion that are needed for further analysis.

Next, to further condense the information present in the dataset, I created two new columns, and , out of the given statistics. The `kda` column represents the **Kill-Death-Assist ratio**, calculated by adding the number of kills and assists and divided by the number of deaths in a given match. This statistic would help us better determine which player/champion "performed" the best, as a higher KDA constritutes more kills and assists on a lower number of deaths, which contributes to winning the game as a whole as one can terminate more enemy opponents before he gets eliminated.

The `allbans` column represents all the banned champions in the match by the two teams by combining the values of `ban1`, `ban2`, `ban3`, `ban4`, and `ban5` into a list. This would help us easily understand all the banned champions by just looking at one column, instead of checking for each of the five columns.

I realized that some columns contain missing values, specifically in the columns . Therefore, 

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem
From the hypothesis testing, it's statistically significant to conclude that is the best champion in competitive LOL matches. Therefore, we can try to predict if a team would win or lose a game given the champions the players chose.

## Baseline Model

## Final Model

## Fairness Analysis
