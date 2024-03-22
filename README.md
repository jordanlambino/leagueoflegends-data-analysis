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
### Data Cleaning
I decided to only analyze matches for teams who played Lee Sin, so that every row represented a team who played Lee Sin. This ensured that my dataset did not contain two rows for each match. I only kept relevant columns which denoted a team's overall performance for that game. In particular, due to the strength of Lee Sin in the early-mid portions of a given match, I decided to keep "golddiffat15" and calculate the "team_kdratio".

**insert dfhead**

### Univariate Analysis
**insert pie chart 1**
The pie chart above shows the win rate of Lee Sin in the LCK/LPL regions (in other words, the distribution of the "won" column).

**insert pie chart 2**
The pie chart above shows the win rate of Lee Sin in non-LCK/LPL Tier One regions (in other words, the distribution of the "won" column).

### Bivariate Analysis
**insert bar chart 1**
The bar chart above shows the win rate of Lee Sin by region. As evident, the LCK/LPL regions have high win-rates when playing Lee Sin, along with the VCS (Vietnam) and LCS (North America).

**insert bar chart 2**
The bar chart above shows the number of games played for Lee Sin by region. This provides new insights to my experiment, as the LCK/LPL regions seem to play Lee Sin significantly more, and still has a positive win rate.
### Interesting Aggregates

## Assignment of Missingness
### NMAR Analysis
**insert analysis**

### Missingness Dependency
Aside from the missingness of "Lee Sin" in matches, it was difficult for me to find potential Missing at Random/Missing Completely at Random data. This was due to the fact that much of the data was Missing by Design, since only certain leagues (namely the LPL) did not include values for "golddiffat15" and "xpdiffat15". 

Instead, I decided to go back to the original, raw dataset and assess the missingness of the variable "elders".

Missingness of "elders" **depends on** "gamelength". Since an "elder dragon" requires that either team first obtains four elemental dragons, I hypothesized that "elders" may be missing for matches with shorter "gamelength".

**insert histogram1**
The graph below shows the distribution of "gamelength" when "elders" is missing.

**insert histogram2**
The graph below shows the distribution of "gamelength" when "elders" is *not* missing.

Using the Kolmogorov-Smirnov (KS) Statistic, the  observed value was **0.0477**.

The p-value was 0.004.

The histogram below displays the empirical distribution of the KS Statistic, along with the obtained p-value.
**histogram**
Using a significance level of 1% (.01), I reject the null hypothesis.

*break*
Missingness of "elders" **does not depend on** "firstblood". I wanted to explore this connection since teams who get the first kill may obtain the elder dragon more often. Conversely, the value for "elders" might be missing for teams who do not get the first kill, and consequently fall behind in the game.

**insert histogram3**
The graph below shows the distribution of "firstblood" when "elders" is missing.

**insert histogram4**
The graph below shows the distribution of "firstblood" when "elders" is not missing.

Using the difference of means as my test-statistic, the observed value was **


## Hypothesis Testing
### Lee Sin Win Rate for LCK/LPL Leagues

## Framing a Prediction Problem
### Problem Identification

## Baseline Model

## Final Model

## Fairness Analysis
