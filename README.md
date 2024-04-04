# **League of Legends Data Analysis 2024**
created by: Jordan Lambino


## **Introduction**

My raw dataset contained statistics taken from over 10,000 professional League of Legends matches in the year 2023. Each unique match includes 12 rows, 10 of which represent the individual players, while the other two represent the teams involved.

Professional leagues are divided into tiers by strength/skill, where Tier One leagues represent the most dominant teams in the world. Some of these Tier One leagues include the LCK (Korea), LPL (China), LEC (Europe), LCS (North America), and PCS (Asia-Pacific). 

Since 2011, a World Championship has taken place pitting Tier One teams against one another. This annual tournament has been dominated by the Korean and Chinese leagues (LCK/LPL), who together have won every single World Championship aside from 2011 (Europe-LEC).

To measure the dominance of those two regions, my project aims to analyze the win-rate of a certain champion (character), "Lee Sin," who is notorious for being one of the most difficult champions to master due to his versatile, complex abilities. In particular, my project attempts to answer the question: "How does the win-rate of Lee Sin in the LCK/LPL compare to the win-rate of Lee Sin across all Tier One teams?"


**Description of Columnns**
- ***won***: True if the team won, False otherwise
- ***league***: categorical column representing the Tier One league for that team
- ***patch***: categorical columm representing the game version for that match
- ***gamelength***: quantitative column, where each value is the length of that match, in minutes
- ***side***: categorical column with two possible values: "Blue" or "Red". Represents the side which the team played on the map
- ***team_kdratio***: quantitative column, calculated by divided the number of team kills by the number of team deaths over the match
- ***firstblood***: True if that team got the first kill in the game, False otherwise
- ***golddiffat15***: quantitative column, denoting the differential in gold (resources) at the 15 minute mark. A positive gold differential typically signifies that a team is "winning"
- ***xpdiffat15***: quantitative column, signifies if one team has gained more experience points through 15 minutes
- ***gameid***: nominal variable; the unique gameid for that game



---


## **Data Cleaning and Exploratory Data Analysis**
### Data Cleaning
I decided to only analyze games for teams who played Lee Sin, so that every row represents a team who played Lee Sin in a given match. This ensured that my dataset did not contain two rows for each match. I only kept relevant columns which denoted a team's overall performance for that game. 

I queried the dataset to only include Tier One Teams and kept the following columns: "league", "gamelength", "side", "firstblood", "golddiffat15", and "gameid". Additionally, I changed the title of the column "result" to "won", for comprehendibility and created a column named "team_kdratio" representing the number of kills divided by the number of deaths. This amounted to a total of 350 rows and 10 columns.

The first 5 rows of my DataFrame are displayed below:

| won   | league   |   patch |   gamelength | side   |   team_kdratio | firstblood   |   golddiffat15 |   xpdiffat15 | gameid                |
|:------|:---------|--------:|-------------:|:-------|---------------:|:-------------|---------------:|-------------:|:----------------------|
| True  | LCK      |   13.01 |      36.1833 | Blue   |        4.25    | True         |           1287 |          538 | ESPORTSTMNT04_2661035 |
| True  | PCS      |   13.01 |      35.65   | Blue   |        2.5     | False        |           1339 |          571 | ESPORTSTMNT03_3098669 |
| True  | LPL      |   13.01 |      32.85   | Blue   |        1.63636 | False        |            nan |          nan | 9718-9718_game_1      |
| True  | CBLOL    |   13.01 |      30.4667 | Blue   |        2.2     | True         |           4450 |         1096 | ESPORTSTMNT01_3302316 |
| True  | LPL      |   13.01 |      24.25   | Red    |       12       | True         |            nan |          nan | 9726-9726_game_2      |


### Univariate Analysis

<iframe
    src="assets/leesin-win-percentage-lcklpl.html" width="800" height="500" frameborder="0"
></iframe>
The pie chart above shows the win rate of Lee Sin in the LCK/LPL regions (in other words, the distribution of the "won" column).


<iframe
    src="assets/leesin-win-percentage-notlcklpl.html" width="800" height="500" frameborder="0"
></iframe>
The pie chart above shows the win rate of Lee Sin in non-LCK/LPL Tier One regions (again, the distribution of the "won" column).


### Bivariate Analysis

<iframe
    src="assets/leesin-win-percentage-byleague.html" width="800" height="400" frameborder="0"
></iframe>
The bar chart above shows the win rate of Lee Sin by region. As evident, the LCK/LPL regions have high win-rates when playing Lee Sin, along with the VCS (Vietnam) and LCS (North America).


<iframe
    src="assets/leesin-num-games-byleague.html" width="800" height="400" frameborder="0"
></iframe>
The bar chart above shows the number of games played for Lee Sin by region. This provides new insights to my experiment, as the LCK/LPL regions seem to play Lee Sin significantly more, yet they still have a higher win-rate than all leagues, aside from the VCS.



### Interesting Aggregates

- Below, we observe the mean statistics of our DataFrame when grouped by "won". We see that "team_kdratio" is in fact a significant indicator of team success during a match. Interestingly, 'gamelength' tends to be shorter when Lee Sin wins, and longer when Lee Sin loses. Though the difference is not necessarily substantial, it could demonstrate that Lee Sin tends to fall off in strength as the game progresses.

| won   |   gamelength |   team_kdratio |   firstblood |   golddiffat15 |   xpdiffat15 |
|:------|-------------:|---------------:|-------------:|---------------:|-------------:|
| False |      31.675  |       0.451633 |     0.388571 |       -1458.06 |     -696.243 |
| True  |      30.7698 |       3.3006   |     0.622857 |        2062.15 |     1136.02  |


- Rather than group matches by result, instead we can look at the mean statistics when grouped by 'firstblood'. We notice that teams who play with Lee Sin often win if they get the first kill. We might also infer that getting the first kill is heavily correlated to team success at the 15 minute mark, as shown in the DataFrame below.

| firstblood   |      won |   gamelength |   team_kdratio |   golddiffat15 |   xpdiffat15 |
|:-------------|---------:|-------------:|---------------:|---------------:|-------------:|
| False        | 0.381503 |      31.5753 |        1.19968 |       -875.301 |     -221.801 |
| True         | 0.615819 |      30.8775 |        2.53727 |       1442.97  |      631.93  |

---


## **Assessment of Missingness**
### NMAR Analysis
Several columns in the dataset are Not Missing at Random (NMAR) as a result of the data generating process. For example, the column "doublekills" depends on the missing value itself, but could potentially depend on the columns "triplekills", "quadrakills", and "pentakills". Moreover, columns such as "firstdragon", "firsttower", and "firstblood" can all be classified as NMAR. For those columns, values seem to be missing simply because those teams didn't fulfill the objective. It could, however, be argued that the missingness of "firstblood" could be dependent on a column named "xpdiffat5". Since the first kill typically happens between 5-10 minutes of gameplay, obtaining additional information about the early game-state may make "firstblood" MAR.

### Missingness Dependency
Aside from the missingness of "Lee Sin" in matches, it was difficult for me to find potential MAR/MCAR data within the cleaned DataFrame. This was due to the fact that much of the missing data was Missing by Design, since only certain leagues (namely the LPL) did not include any values for "golddiffat15" and "xpdiffat15". 

Consequently, I decided to go back to the original, raw dataset and assess the missingness of the variable "elders".

Missingness of "elders" **depends on** "gamelength". Since an "elder dragon" requires that either team first obtains four elemental dragons, I hypothesized that "elders" may be missing for matches with a shorter gamelength.

<iframe
    src="assets/gamelength-distr-eldermissing.html" width="800" height="400" frameborder="0"
></iframe>The graph above shows the distribution of "gamelength" when "elders" is missing.

<iframe
    src="assets/gamelength-distr-eldernotmissing.html" width="800" height="400" frameborder="0"
></iframe>The graph above shows the distribution of "gamelength" when "elders" is *not* missing.

Using the Kolmogorov-Smirnov (KS) Statistic, the  observed value was **0.0477**.

The p-value was **0.004**.

The histogram below displays the empirical distribution of the KS Statistic, along with the obtained p-value.

<iframe
    src="assets/ks-statistic-distr-mar.html" width="800" height="400" frameborder="0"
></iframe>Using a significance level of 1% (.01), I reject the null hypothesis.



---


Missingness of "elders" **does not depend on** "firstblood". I wanted to explore this connection since teams who get the first kill may obtain the elder dragon more often. Conversely, the value for "elders" might be missing for teams who do not get the first kill, and consequently fall behind in the game.


Using the difference of means as my test-statistic, the observed value was **0.001**.

The p-value was **0.44**.

The histogram below displays the empirical distribution of the difference of means, along with the obtained p-value.

<iframe
    src="assets/diff-means-mcar.html" width="800" height="400" frameborder="0"
></iframe>
Using a significane level of 5% (.05), I fail to reject the null hypothesis.



---
 

## **Hypothesis Testing**
### Lee Sin Win Rate for LCK/LPL Leagues

**Null Hypothesis:** The win percentage for the champion "Lee Sin" in the LCK/LPL is equal to the average win percentage of Lee Sin in all professional tier one leagues.

**Alternative Hypothesis:** The win percentage for the champion "Lee Sin" in the LCK/LPL is not equal to the average win percentage of Lee Sin in all professional tier one leagues.

**Test Statistic:** Total Variation Distance (TVD)
- The TVD is an appropriate test statistic when comparing categorical distributions. In this test, I compare the distribution of "won" for the LCK/LPL, along with the overall distribution.

**Significance-Level:** 5% (0.05)

**p-value:** **0.096**, using 100,000 simulations
<iframe
    src="assets/hypothesis-test-empirical.html" width="800" height="400" frameborder="0"
></iframe>

**Conclusion:** Fail to Reject the Null Hypothesis.
- There is a lack of evidence demonstrating that the win percentage of Lee Sin in LCK/LPL is substantially different than that of all Tier One teams. In other words, the distribution of the "won" column for LCK/LPL teams playing Lee Sin comes from the same distribution of "won" for all games including Lee Sin. However, had the other regions played more games with Lee Sin, it is possible that their win-rate would decrease, and thus affect the overall distribution.

---


## **Framing a Prediction Problem**
**Problem Identification:** Predict whether or not a team will obtain the first baron in the match. Obtaining the baron is basically a temporary power boost for a team, and it is one of the most important side objectives in the game.

**Problem Type:** Binary Classification
- First baron can be True (1) or False (0).

**Response Variable:** "firstbaron"
- I chose this variable since it is a clear indication of whether or not the team obtained the first baron.

**Metric:** Accuracy
- I used the accuracy metric to evaluate my model performance. Since I analyzed a classification problem, I thought that accuracy was the most intuitive and interpretable metric for readers.

***Notes on Assumptions:*** Since the first baron appears at 20 minutes, I selected features which are almost certain before the 20 minute mark. As stated later in the Final Model section, I used features such as 'firsttower' and 'firstdragon', which are not restricted to occur before 20 minutes. However, it is extremely rare for professional teams to obtain the first baron before 'firsttower' or 'firstdragon' occur, simply due to the nature of the game.



---


## **Baseline Model**
For my baseline model, I included three features: 'side', 'firstblood', and 'firstdragon'. I chose 'side' since the spawn-location of the baron is slightly more favorable for the blue team. I chose 'firstblood' and 'firstdragon' since each are important in allowing a team to gain an early lead, and later on get the baron. All of these three variables are nominal.

Initially, I used a DecisionTreeClassifier and then changed it to a RandomForestClassifier. I began with a max_depth of 25 as my only hyperparameter.

In terms of accuracy, I would say that my baseline model performed poorly. When testing accuracy, my model sat around **60%** across trials. This was somewhat expected since I did not include that many features, and used arbitrary hyperparameters.



---


## **Final Model**

When creating my final model, I first searched for other features which might help predict 'firstbaron' more accurately. In particular, I looked for variables which signified team success in the mid-game (10-20 minutes), since these factors could heavily influence who gets the first baron. After testing different features, I decided to include the following:
- ***side*** (nominal)
- ***firstblood*** (nominal)
- ***golddiffat15*** (quantitative)
- ***xpdiffat15*** (quantitative)
- ***firstdragon*** (nominal)
- ***firsttower*** (nominal)
- ***firstmidtower*** (nominal)
- ***turretplates*** (quantitative)
- ***csdiffat15*** (quantitative)
- ***xpdiffat10*** (quantitative)
- ***killsat15*** (quantitative)
- ***deathsat15*** (quantitative)

I fine-tuned the hyperparameters "max_depth", "n_estimators", and "criterion" in order to improve my model performance and avoid overfitting. I used GridSearchCV with a grid size of 60 to search for optimal hyperparameters. After searching, I obtained values of 8, 100, and 'gini', respectively. I saw a slight increase in accuracy (around 1-2 percentage points) after tweaking these hyperparameters.

For my final model, I used a RandomForestClassifier to predict which team would obtain the first baron. Initially, I had used a DecisionTreeClassifier, but after testing the accuracy of both models, it appeared that the RandomForestClassifer was performing better in terms of generalization. Conversely, the DecisionTreeClassifier was performing extremely well when predicting the training data, but not the test data. This sign of overfitting influenced me to switch Classifier models. Ultimately, I believe that using the RandomForestClassifier was an optimal choice since the randomness and unpredictability of League of Legends can greatly sway one team's chances of getting the first baron.

My improved accuracy for the final model was approximately **72.1%**. I believe adding more features played a huge role in this, as information such as "golddiffat15" and "xpdiffat15" are significant indicators regarding the performance of a team during a match. Most importantly, I believe that my final model improved in terms of generalization compared to my baseline model. Initially, my model was overfit to the training set and performed poorly when generalizing to unseen data. The combination of features and hyperparameters in my final model are a large improvement for this issue, as denoted by the significant increase in accuracy.



---


## **Fairness Analysis**

**Group 1:** Red-Side Teams
**Group 2:** Blue-Side Teams

**Null Hypothesis:** The classifier's accuracy is equal for teams playing on the red side and teams playing on the blue side.

**Alternative Hypothesis:** The classifer's accuracy is higher for teams on the red side than it is for teams playing on the blue side.

**Test Statistic:** Difference in Accuracy (Red - Blue)

**Significance Level:** 5% (0.05)

**p-value:** 0.074

**Conclusion:** Fail to reject the null hypothesis.
- Using our permutation test, there is a lack of evidence suggesting that the classifier is more accurate for red teams than it is for blue teams.

