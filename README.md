# League of Legends Data Analysis 2024
author: Jordan Lambino

## Introduction

My raw dataset contained statistics taken from over 10,000 professional League of Legends matches in the year 2023. Each unique match includes 12 rows, 10 of which represent the individual players, while the other two represent the teams involved.

Professional leagues are divided into tiers by strength/skill, where Tier One leagues represent the most dominant teams in the world. Some of these Tier One leagues include the LCK (Korea), LPL (China), LEC (Europe), LCS (North America), and PCS (Asia-Pacific). 

Since 2011, a World Championship has taken place pitting Tier One teams against one another. This annual tournament has been dominated by the Korean and Chinese leagues (LCK/LPL), who together have won every single World Championship aside from 2011 (Europe-LEC).

To measure the dominance of those two regions, my project aims to analyze the win-rate of a certain champion (character), "Lee Sin," who is notoriously known for being one of the most difficult champions to master due to his versatile, complex abilities. In particular, my project attempts to answer the question: "How does the win-rate of Lee Sin in the LCK/LPL compare to the win-rate of Lee Sin across all Tier One teams?"

To answer this question, I queried the dataset to only include Tier One Teams and kept the following columns: "league", "gamelength", "side", "firstblood", "golddiffat15", and "gameid". Additionally, I changed the title of the column "result" to "won", for comprehendibility, created a column named "team_kdratio" representing the number of kills divided by the number of deaths, and created a column named "Lee Sin_played". This amounted to a total of 350 rows and 12 columns.

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
- *xpdiffat15*: quantitative column, signifies if one team has gained more experience points through 15 minutes
- *gameid*: nominal variable; the unique gameid for that game

---
## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
I decided to only analyze matches for teams who played Lee Sin, so that every row represented a team who played Lee Sin. This ensured that my dataset did not contain two rows for each match. I only kept relevant columns which denoted a team's overall performance for that game. In particular, due to the strength of Lee Sin in the early-mid portions of a given match, I decided to keep "golddiffat15" and calculate the "team_kdratio".

**insert dfhead**

### Univariate Analysis

<iframe
    src="assets/leesin-win-percentage-lcklpl.html" width="800" height="600" frameborder="0"
></iframe>
The pie chart above shows the win rate of Lee Sin in the LCK/LPL regions (in other words, the distribution of the "won" column).

<iframe
    src="assets/leesin-win-percentage-lcklpl.html" width="800" height="600" frameborder="0"
></iframe>
The pie chart above shows the win rate of Lee Sin in non-LCK/LPL Tier One regions (in other words, the distribution of the "won" column).

### Bivariate Analysis

<iframe
    src="assets/leesin-win-percentage-byleague.html" width="800" height="600" frameborder="0"
></iframe>
The bar chart above shows the win rate of Lee Sin by region. As evident, the LCK/LPL regions have high win-rates when playing Lee Sin, along with the VCS (Vietnam) and LCS (North America).

<iframe
    src="assets/leesin-num-games-byleague.html" width="800" height="600" frameborder="0"
></iframe>
The bar chart above shows the number of games played for Lee Sin by region. This provides new insights to my experiment, as the LCK/LPL regions seem to play Lee Sin significantly more, and still has a positive win rate.

### Interesting Aggregates
| won   |   gamelength |   team_kdratio |   firstblood |   golddiffat15 |   xpdiffat15 |
|:------|-------------:|---------------:|-------------:|---------------:|-------------:|
| False |      1900.5  |       0.451633 |     0.388571 |       -1458.06 |     -696.243 |
| True  |      1846.19 |       3.3006   |     0.622857 |        2062.15 |     1136.02  |

---

## Assessment of Missingness
### NMAR Analysis
I believe that the column 'firsttower' is Not Missing at Random (NMAR).

### Missingness Dependency
Aside from the missingness of "Lee Sin" in matches, it was difficult for me to find potential Missing at Random/Missing Completely at Random data. This was due to the fact that much of the data was Missing by Design, since only certain leagues (namely the LPL) did not include values for "golddiffat15" and "xpdiffat15". 

Instead, I decided to go back to the original, raw dataset and assess the missingness of the variable "elders".

Missingness of "elders" **depends on** "gamelength". Since an "elder dragon" requires that either team first obtains four elemental dragons, I hypothesized that "elders" may be missing for matches with shorter "gamelength".

<iframe
    src="assets/gamelength-distr-eldermissing.html" width="800" height="600" frameborder="0"
></iframe>The graph above shows the distribution of "gamelength" when "elders" is missing.

<iframe
    src="assets/gamelength-distr-eldernotmissing.html" width="800" height="600" frameborder="0"
></iframe>The graph above shows the distribution of "gamelength" when "elders" is *not* missing.

Using the Kolmogorov-Smirnov (KS) Statistic, the  observed value was **0.0477**.

The p-value was **0.004**.

The histogram below displays the empirical distribution of the KS Statistic, along with the obtained p-value.

<iframe
    src="assets/ks-statistic-distr-mar.html" width="800" height="600" frameborder="0"
></iframe>Using a significance level of 1% (.01), I reject the null hypothesis.

---
Missingness of "elders" **does not depend on** "firstblood". I wanted to explore this connection since teams who get the first kill may obtain the elder dragon more often. Conversely, the value for "elders" might be missing for teams who do not get the first kill, and consequently fall behind in the game.


Using the difference of means as my test-statistic, the observed value was **0.001**.

The p-value was 0.44.

The histogram below displays the empirical distribution of the difference of means, along with the obtained p-value.

<iframe
    src="assets/diff-means-mcar.html" width="800" height="600" frameborder="0"
></iframe>Using a significane level of 5% (.05), I fail to reject the null hypothesis.

---
## Hypothesis Testing
### Lee Sin Win Rate for LCK/LPL Leagues

**Null Hypothesis:** The win percentage for the champion "Lee Sin" in the LCK/LPL is less than or equal to the average win percentage of Lee Sin in all professional tier one leagues.

**Alternative Hypothesis:** The win percentage for the champion "Lee Sin" in the LCK/LPL is greater than the average win percentage of Lee Sin in all professional tier one leagues.

**Test Statistic:** Total Variation Distance (TVD)
- The TVD is an appropriate test statistic when comparing categorical distributions. In this test, I compare the distribution of "won" for the LCK/LPL, along with the overall distribution.

**Significance-Level:** 5% (0.05)

**p-value:** 0.096, using 100,000 simulations
<iframe
    src="assets/hypothesis-test-empirical.html" width="800" height="600" frameborder="0"
></iframe>

**Conclusion:** Fail to Reject the Null Hypothesis.
- There is a lack of evidence demonstrating that the win-rate of Lee Sin in LCK/LPL is substantially greater than that of all Tier One teams. In other words, the distribution of the "won" column for LCK/LPL teams playing Lee Sin comes from the same distribution of "won" for all games including Lee Sin. However, had the other regions played more games with Lee Sin, it is possible that their win-rate would decrease, and thus affect the overall distribution.

---
## Framing a Prediction Problem
**Problem Identification:** Predict whether or not a team will obtain the first baron in the match.

**Problem Type:** Binary Classification
- First baron can be True (1) or False (0).

**Response Variable:** "firstbaron"
- I chose this variable since it is a clear indication of whether or not the team obtained the first baron. **metric**. 

***Notes:*** Since the first baron appears at 20 minutes, I selected features which are almost certain before the 20 minute mark. 

---

## Baseline Model
For my baseline model, I included three features: 'side', 'firstblood', and 'firstdragon'. I chose 'side' since the spawn-location of the baron is slightly more favorable for the blue team. I chose 'firstblood' and 'firstdragon' since each are important in allowing a team to gain an early lead, and later on get the baron. All of these three variables are nominal.

Initially, I used a DecisionTreeClassifier and then changed it to a RandomForestClassifier. I began with a max_depth of 25 as my only hyperparameter.

In terms of accuracy, I would say that my baseline model performed poorly. When testing accuracy, my model sat around 60% across trials. This was somewhat expected since I did not include that many features, and used arbitrary hyperparameters.

---

## Final Model

For my final model, I added multiple features and fwhich allowed for 

I fine-tuned "max_depth", "n_estimators", and "criterion" in order to improve my model performance and avoid overfitting. I used GridSearchCV with a grid size of 60 to search for optimal hyperparameters. After searching, I obtained values of 8, 100, and 'gini'.

For my final model, I used a RandomForestClassifier to predict which team would obtain the first baron. Initially, I had used a DecisionForestClassifier, but after testing the accuracy of both models, it appeared that the RandomForestClassifer was performing better in terms of generalization. Conversely, the DecisionForestClassifier was performing well when predicting the training data. I believe that the RandomForestClassifier is an optimal choice as the randomness of League of Legends can greatly sway one team's chances of getting the first baron.

My improved accuracy for the final model was approximately 72.1%. I believe adding more features played a huge role in this, as information such as "golddiffat15" and "xpdiffat15" are significant indicators regarding the performance of a team during a match. Moreover, variables such as "firstmidtower" typically denote if a team gains early control over the map, which might lead to the first baron as the game goes on.

---

## Fairness Analysis
Group X: LCK Teams
Group Y: LCS Teams

Null Hypothesis: The model is fair. The precision of the model is the same for LCK and LCS teams.

Alternative Hypothesis: The model is not fair. The precision of the model is greater for LCK teams than it is for LCS teams.

Conclusion: ...

