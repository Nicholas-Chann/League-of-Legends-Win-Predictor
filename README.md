# League of Legends First Turret Analysis and Win Predictor
In this project I analyze a League of Legends(LOL) 2025 dataset from Oracles Elixer. This was conducted as a final project for DSC80 at UCSD focusing on the impact of destroying first turret and how other related features affect the outcome of a game.


Author: Nicholas Chan


# Introduction

League of Legends is a popular multiplayer online battle arena game launched in October 2009 by Riot Games. It has since become one of the most popular multiplayer games and has a big esports scene. The dataset that I worked with contains data from professional esports matches that took place throughout 2025. 

This dataset contains key information about each match that was played. The dataset captures the teams and individual players performances, the outcome of the match, and timestamps of certain in-game statistics. 


In a game of League of Legends, I believe that the team who gets **first tower** gains a significant advantage over the other. Destroying first tower opens up the map and allows for more team play, better objective control, and in general allows you to pressure the opponent more. This leads to the main interest of my project, how accurately can we predict the outcome of a game know which team got first tower. Further more I will investigate how much more we can improve this model by including features available to us at the 15 minute mark of a game.


The dataset contains a wide summary of each professional esports match played with the metrics totaling to 118932 rows and 164 columns of unique data. I decided to only keep the rows that summarized the teams performance, since the goal is to predict the outcome of the game we dont really care about each individuals performance. For the sake of our analysis we are only interested in a couple of these columns. Here is an introduction to the columns that I will be using:

- `gameid`: This column represents a unique identifier for each individual match played

- `side` : The side of the map the team played on, Red or Blue

- `firstblood`: Binary column indicating if the team attained first blood

- `firstdragon`: Binary column indicating if the team attained first dragon

- `firsttower`: Binary column indicating if the team destroyed first tower

- `firstmidtower`: Binary column indicating if the team destroyed first mid tower

- `firsttothreetowers`: Binary column indicating if team was the first to destroy 3 towers

- `turretplates`: The number of turret platting achieved by the team

- `opp_turretplates`: The number of turret platting achieved by the opposing team

- `golddiffat15`: The difference in gold attained at 15 minutes

- `killsat15`: The number of team kills at 15 minutes

- `csdiffat15`: The difference in team creep score at 15 minutes

- `xpdiffat15`: The difference in team xp at 15 minutes

- `assistsat15`: The teams number of total assists at 15 minutes

- `void_grubs`: The number of void grubs achieved by the team


# Data Cleaning and Exploratory Data Analysis


As stated earlier for our problem we are only interested in the team summarizes, so my first step was to filter the dataset to contain only rows that had the `position` value of 'team'. After filtering and analyzing our new dataset I noticed that quite a few columns has missing data. These columns being, 'firstbaron', 'firstmidtower', 'firstherald', 'minionkills', 'total cs', 'turretplates', 'opp_turretplates', 'golddiffat15', 'killsat15', 'csdiffat15','xpdiffat15', and 'assistsat15'. However I noticed that the rows where these values were missing were all the same so for simplicity I decided to drop those games as I had no way to recover the lost data and there is plenty of other data to collect from. However I was able to fill in the missing values for 'total cs' I simply had to combine the minionkills column with monsterkills column for each game.  

Here is the head of my cleaned dataframe:


| gameid           | side   |   result |   firstblood |   firsttower |   firstmidtower |   firstdragon |   firstherald |   firstbaron |   teamkills |   totalgold |   teamdeaths |   minionkills |   monsterkills |   total cs |   dragons |   barons |   towers |   inhibitors |   void_grubs |   turretplates |   opp_turretplates |   golddiffat15 |   killsat15 |   csdiffat15 |   xpdiffat15 |   assistsat15 |
|:-----------------|:-------|---------:|-------------:|-------------:|----------------:|--------------:|--------------:|-------------:|------------:|------------:|-------------:|--------------:|---------------:|-----------:|----------:|---------:|---------:|-------------:|-------------:|---------------:|-------------------:|---------------:|------------:|-------------:|-------------:|--------------:|
| LOLTMNT03_179647 | Blue   |        0 |            0 |            0 |               0 |             0 |             0 |            0 |           3 |       42255 |           13 |           731 |            144 |        875 |         0 |        0 |        3 |            0 |            0 |              1 |                  8 |          -3837 |           0 |          -16 |         -469 |             0 |
| LOLTMNT03_179647 | Red    |        1 |            1 |            1 |               1 |             1 |             1 |            1 |          13 |       53936 |            3 |           753 |            169 |        922 |         2 |        1 |        9 |            2 |            6 |              8 |                  1 |           3837 |           3 |           16 |          469 |             9 |
| LOLTMNT06_96134  | Blue   |        1 |            1 |            1 |               1 |             0 |             1 |            1 |          21 |       64669 |           11 |           829 |            199 |       1028 |         3 |        1 |       11 |            3 |            6 |              8 |                  2 |           5069 |          10 |           64 |         2014 |            20 |
| LOLTMNT06_96134  | Red    |        0 |            0 |            0 |               0 |             1 |             0 |            0 |          10 |       50679 |           21 |           790 |            134 |        924 |         2 |        0 |        2 |            0 |            0 |              2 |                  8 |          -5069 |           5 |          -64 |        -2014 |             8 |
| LOLTMNT06_95160  | Blue   |        0 |            0 |            0 |               1 |             0 |             0 |            0 |          18 |       51389 |           22 |           717 |            146 |        863 |         0 |        0 |        3 |            0 |            2 |              7 |                 10 |            118 |          10 |          -43 |         1990 |            13 |


## Univariate Analysis

I performed a univariate analysis on the distribution of total team gold in the dataset. 



<iframe
  src="assets/team_gold_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The histogram shows that the distribution of team gold is nearly normal with a slight right skew. This suggests that the data is well-behaved, with totalgold being distributed in a manner that is relatively balanced and serves as a good statistic for analyzing team behavior.

## Bivariate Analysis 

I performed a biariate analysis on the the statistics of first turret and result to see of the winning teams how many of them destroyed first tower.


<iframe
  src="assets/first_tower_outcome.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

According to the plot, teams that destory first tower have a win rate of about 70% while teams who do not destroy first tower win about 30% of the time. This means that the majority of teams who destroy first tower end up winning the match indicating its importance in a match.

## Interesting Aggregates

Here are some interesting aggregates within the dataset that I looked at


|   firsttower |   firstblood |   firstdragon |   firstherald |   firstbaron |   firstmidtower |
|-------------:|-------------:|--------------:|--------------:|-------------:|----------------:|
|            0 |         3897 |          4333 |          3198 |         2687 |            2495 |
|            1 |         5197 |          4755 |          5877 |         4801 |            6599 |


I first did a groupby on the cleaned data set on firsttower and then calculated the sum of all the other columns I was interested in. I wanted to see of the teams that secured first tower how many of the other objectives did the also secure first. As the data shows, the team that gets first tower has better statistics for other objectives as well.

# Assessment of Missingness

## NMAR Analysis

In the dataset, I believe that the columns `doublekills`,`triplekills`,	`quadrakills`, `pentakills` are all Not Missing At Random(NMAR). Looking at the columns, there does not seem to be a trend with the missingness. In an actual League game it is possible that no player achieves a multi-kill streak and in this case the value would be missing because it never occured, meaning that the missingness of the value would depend on itself. An additional data 
that we would need in order to conclude that the columns are actually Missing At Random (MAR) would be a binary column containing 1 if the player obtained a kill streak and 0 if they did not.

## Missingness Dependency

In this part, I am going to test if the missingness of the `firstbaron` column depends on another column like `gamelength`. In a league match baron does not spawn until the 20 minute mark so I want to determine if the missingness is becuase the match is finished before the 20 minute mark or if it was simply not taken. I conducted a test using the absolute difference in mean as my test statistic with a 5% significance level.

Null Hypothesis (H_0): The distribution of gamelength when firstbaron is missing is the same as the distribution of gamelength when firstbaron is not missing.

Alternative Hypothesis(H_1): The distribution of gamelength when firstbaron is missing is NOT the same as the distribution of gamelength when firstbaron is not missing 

<iframe
  src="assets/abs_diff_fb.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

At a 5% significance level our p value was 0.0011 meaning we reject the null that the distribution of gamelength when firstbaron is missing is the same as the distribution of gamelength when firstbaron is not missing.

For the second test I tested the missingness in firstbaron against towers, there are both categorical variables so I used TVD to test instead of the difference in absolute means

H_0: The probability that firstbaron is missing is the same for Red‑side teams and Blue‑side teams.

H_1: The probability that firstbaron is missing differs between Red‑side teams and Blue‑side teams.

<iframe
  src="assets/fb_side.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The p-value is much greater than the 0.5 significance level so we fail to reject the null hypothesis. Meaning that the missingness of firstbaron does not depend of the side column

# Hypothesis Testing



