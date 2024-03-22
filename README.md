### leagueoflegends-data-analysis:


# League of Legends Data Analysis 2024

## Introduction

My raw dataset contained statistics taken from over 10,000 professional League of Legends matches in the year 2023. Each unique match includes 12 rows, 10 of which represent the individual players, while the other two represent the teams involved.

Professional leagues are divided into tiers by strength/skill, where Tier One leagues represent the most dominant teams in the world. Some of these Tier One leagues include the LCK (Korea), LPL (China), LEC (Europe), LCS (North America), and PCS (Asia-Pacific). 

Since 2011, a World Championship has taken place pitting Tier One teams against one another. This annual tournament has been dominated by the Korean and Chinese leagues (LCK/LPL), who together have won every single World Championship aside from 2011 (Europe-LEC).

To measure the dominance of those two regions, my project aims to analyze the win-rate of a certain champion (character), "Lee Sin," who is notoriously known for being one of the most difficult champions to master due to his versatile, complex abilities. In particular, my project attempts to answer the question: "How does the win-rate of Lee Sin in the LCK/LPL compare to the win-rate of Lee Sin across all Tier One teams?"

To answer this question, I queried the dataset to only include Tier One Teams and kept the following columns: "league", "gamelength", "side", "firstblood", "golddiffat15", and "gameid". Additionally, I changed the title of the column "result" to "won", for comprehendibility, created a column named "team_kdratio" representing the number of kills divided by the number of deaths, and created a column named "Lee Sin_played". This amounted to a total of 350 rows and 11 columns.

**Description of Columnns**
- *won*: True if the team won, False otherwise
- *Lee Sin_played*: True if any player on that team selected Lee Sin, False otherwise
- *league*: categorical column representing the Tier One league for that team
- *patch*: categorical columm representing the game version for that match
- *gamelength*: quantitative column, where each value is the length of that match, in seconds
- *side*: categorical column with two possible values: "Blue" or "Red". Represents the side which the team played on the map
- *team_kdratio*: quantitative column, calculated by divided the number of team kills by the number of team deaths over the match
- *firstblood*: True if that team got the first kill in the game, False otherwise
- *golddiffat15*: quantitative column, denoting the differential in gold (resources) at the 15 minute mark. A positive gold differential typically signifies that a team is "winning"
- *gameid*: nominal variable; the unique gameid for that game






## Data Cleaning and Exploratory Data Analysis
### Univariate Analysis

### Bivariate Analysis

### Interesting Aggregates

## Assignment of Missingness
### NMAR Analysis

### Missingness Dependency

## Hypothesis Testing
### Lee Sin Win Rate for LCK/LPL Leagues

## Framing a Prediction Problem
### Problem Identification

## Baseline Model

## Final Model

## Fairness Analysis
