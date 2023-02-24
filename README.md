
# Dominating the Rift: Uncovering the Most Impactful Position in League of Legends
Data analysis on the impact of different roles from League of Legends for a DSC80 project at UCSD. 
Contributors: Charisse Hao and Nicole Zhang


## Table of Contents
- [Introduction](#introduction)
    - [Dataset Statistics and Definitions](#dataset-statistics-and-definitions)
- [Cleaning and EDA (Exploratory Data Analysis)](#cleaning-and-eda)
    - [Data Cleaning](#data-cleaning)
    - [Univariate Analysis](#univariate-analysis)
    - [Bivariate Analysis](#bivariate-analysis)
    - [Interesting Aggregates](#interesting-aggregates)
- [Assessment of Missingness](#assessment-of-missingness)
    - [NMAR Analysis](#nmar-analysis)
    - [Missingness Dependency](#missingness-dependency)
- [Hypothesis Testing](#hypothesis-testing)


## Introduction
Welcome to Summoner’s Rift! [League of Legends (LoL)](https://en.wikipedia.org/wiki/League_of_Legends) is a popular online multiplayer battle arena game, with millions of active players worldwide. The game's massive player base makes it an excellent source of data for researchers and data scientists interested in exploring various aspects of the game, from player behavior to gameplay mechanics. 

League of Legends is a competitive game, so it is natural for players to want to dominate their games and achieve victory. However, what is the best way to obtain this goal? Two teams, consisting of five players each, battle to destroy the opposing team's center base known as the Nexus. Each team's players fulfill a specific role on the team - selecting to play one of the five roles: top, middle, bottom, support, or jungle. If players want to carry their team to victory, they might want to pick the best role to do so. **Thus, we decided to conduct our data analysis on which role is the most impactful in the overall duration of the game.** 

\* For our purposes, impact will be defined through the statistics of KDA, total gold, and damage to champions

The dataset we used to conduct this analysis is the [2022 League of Legends Esports Stats dataset](https://drive.google.com/file/d/1EHmptHyzY8owv0BAcNKtkQpMwfkURwRy/view?usp=sharing) provided by Oracle's Elixir, a website that provides advanced statistics and analysis tools for the game. This dataset contains a comprehensive collection of data from professional League of Legends matches, including detailed information on player and team performance, game events, and match outcomes. The dataset contains information on thousands of matches, covering various tournaments and competitions.

### Dataset Statistics and Definitions:
The dataset has a total of 149,232 rows and 123 columns. We cleaned the dataset to only use the data we need for our analysis, so we used 124,360 rows and 9 columns. Below is the definition of the columns we used from this dataset.

| **Column Name**     | **Definition**                                               |
| ------------------- | ------------------------------------------------------------ |
| `gameid`            | game ID                                                      |
| `datacompleteness`  | indicates if the data for this match was complete or partial |
| `position`          | player’s position (more detail in table below)               |
| `kills`             | total kills per player                                       |
| `deaths`            | total deaths per player                                      |
| `assists`           | total assists per player                                     |
| `damagetochampions` | total damage dealt to other champions                        |
| `totalgold`         | total gold per player                                        |
| `total cs`          | total creep score per player                                 |

\* Note: we will be using the words "role" and "position" interchangeably 

| **Positions Abbreviation** | **Meaning** |
| -------------------------- | ----------- |
| *top*                      | top         |
| *jng*                      | jungle      |
| *mid*                      | middle      |
| *bot*                      | bottom      |
| *sup*                      | support     |


## Cleaning and EDA (Exploratory Data Analysis) 

### Data Cleaning
The first step of our data cleaning process was to check for missingness in our dataset. We found that the column damagetochampions had missing values when the value for datacompleteness was `partial`. We then dropped the rows where `datacompleteness` is partial, which got rid of all missing values in our dataset.

The second step of our data cleaning process was to standardize our statistics in the dataset. We transformed all our data to z-scores for each game. We did this by creating a helper function that calculates the z-score using the z-score formula: Z = (X-µ) / σ

We then grouped by `gameid` and transformed the numerical statistics using the z-score helper function. The first five rows of our dataframe are included below:
<table border="1" class="dataframe" width='100%'> <thead>    <tr style="text-align: right;">      <th></th>      <th>gameid</th>      <th>datacompleteness</th>      <th>position</th>      <th>damagetochampions</th>      <th>totalgold</th>      <th>total cs</th>      <th>KDA</th>    </tr>  </thead>  <tbody>    <tr>      <th>0</th>      <td>ESPORTSTMNT01_2690210</td>      <td>complete</td>      <td>top</td>      <td>0.319151</td>      <td>0.470574</td>      <td>0.587278</td>      <td>-0.947893</td>    </tr>    <tr>      <th>1</th>      <td>ESPORTSTMNT01_2690210</td>      <td>complete</td>      <td>jng</td>      <td>-0.283245</td>      <td>-0.404958</td>      <td>-0.399444</td>      <td>-0.892339</td>    </tr>    <tr>      <th>2</th>      <td>ESPORTSTMNT01_2690210</td>      <td>complete</td>      <td>mid</td>      <td>0.091917</td>      <td>-0.123676</td>      <td>0.135526</td>      <td>-0.704843</td>    </tr>    <tr>      <th>3</th>      <td>ESPORTSTMNT01_2690210</td>      <td>complete</td>      <td>bot</td>      <td>-0.382416</td>      <td>0.310190</td>      <td>0.527837</td>      <td>-1.017335</td>    </tr>    <tr>      <th>4</th>      <td>ESPORTSTMNT01_2690210</td>      <td>complete</td>      <td>sup</td>      <td>-1.502484</td>      <td>-1.604184</td>      <td>-1.659597</td>      <td>-0.934004</td>    </tr>  </tbody></table>



### Univariate Analysis
For our univariate analysis, we decided to focus on investigating the distribution of each statistic’s z-scores depending on the position column. In order to generate these plots, we grouped by position before calculating the average z-score for each statistic we were investigating. These included: KDA, total gold, total creep score, and damage to champions. 

<p float = 'left'> 
    <iframe src="assets/univariate-plots/uni-fig-mid.html" width=410 height=275 frameBorder=0></iframe>
    <iframe src="assets/univariate-plots/uni-fig-bot.html" width=410 height=275 frameBorder=0></iframe> 
<p float = 'left'> 
    <iframe src="assets/univariate-plots/uni-fig-top.html" width=410 height=300 frameBorder=0></iframe>
    <iframe src="assets/univariate-plots/uni-fig-sup.html" width=410 height=300 frameBorder=0></iframe> 
<p float = 'left'> 
    <iframe src="assets/univariate-plots/uni-fig-jng.html" width=410 height=300 frameBorder=0></iframe>
</p>
</p>
</p>

In the bar graphs provided above, each graph corresponds to a different role or position, with each bar on the x-axis representing a different statistic while the y-axis indicates the average z-score. Upon taking a closer look at these graphs, the first thing we noticed was that roles such as middle and top had positive average z-scores for all statistics while top had a combination of positive and negative and roles such as support and jungle had average z-scores around or below zero. Based on this observation, the data seems to indicate middle and bottom roles have more impact in games overall than top, support, and jungle. 


### Bivariate Analysis
In this section, we wanted to continue comparing the statistics’ z-scores across all positions. We combined all positions’ average z-scores into one bar graph to make comparisons easier, as well as creating box plots to perform further analysis on the distribution of z-scores for each statistic. 

<iframe src="assets/bivariate-plot-all.html" width=800 height=435 frameBorder=0></iframe>

The graph above contains each positions’ average z-score across all statistics. Along the x-axis, it specifies which position the bars belong to, and the y-axis once again indicates the average z-score. The bins for each position are color coded based on the legend, with blue corresponding to KDA, red to total gold, green to total creep score, and purple to damage to champions. By plotting all positions side-by-side, we were able to deduce that bottom has the overall highest average z-scores for all statistics, while support has the lowest average z-score for all statistics besides KDA. 

The following box plots illustrate each position’s distribution of z-scores for KDA and damage to champions respectively. The x-axis specifies which position the box plot corresponds to and the y-axis represents the z-score. 

<iframe src="assets/bivariate-plot-kda.html" width=800 height=435 frameBorder=0></iframe>

Taking a closer look at this plot, we find that all positions seem to have relatively the same spread in z-scores, with the median ranging from around -0.1 to -0.5. The boxplot for top seems to be slightly lower compared to the others, and top and support are the only roles containing outliers. Once again, we notice the roles of middle and bottom have higher z-scores, as their box plots are higher than the other positions’.

<iframe src="assets/bivariate-plot-dc.html" width=800 height=435 frameBorder=0></iframe>

Unlike the previous boxplot, there is a lot more variance in the distribution of z-scores across each role. Top, middle, and bottom all seem to have similarly wide ranges, while jungle and support have much smaller ranges. Furthermore, jungle and support have the lowest medians at -0.55 and -1.23 respectively, and they also have the most outlying data points. On the other hand, middle and bottom continue to have the highest medians, which further implies that these two roles may be the most impactful overall. 


### Interesting Aggregates
To further explore the distribution of z-scores and the pattern we noticed of middle and top consistently having higher z-scores, we decided to plot all positions’ z-scores for KDA and total damage to champions in an overlaid histogram. For the following plots, the x-axis represents the z-score, the y-axis represents the number of players that fall within each z-score bin, and each color corresponds to a certain role based on the legend - blue is top, red is jungle, green is middle, purple is bottom, and support is orange. 

<iframe src="assets/int-agg-plots/int-fig-kda.html" width=800 height=375 frameBorder=0></iframe>

This overlaid histogram seems to indicate that all roles have a similar spread in z-scores for KDA, contrasting to the plots we analyzed previously. Each bin appears to have a similar count for each role, and this conflicting observation led us to our null hypothesis: Do all roles have the same impact overall?

<iframe src="assets/int-agg-plots/int-fig-dc.html" width=800 height=375 frameBorder=0></iframe>

Unlike the preceding overlaid histogram, this plot supports the observation made in previous sections - support generally has the lowest z-scores while middle, bottom, and top have the highest z-scores. 


## Assessment of Missingness

### NMAR Analysis
We believe that the column `ban5` in our dataset is not missing at random (NMAR). Some values are missing in this column with no apparent pattern, so we think that some teams have either forgotten to ban or did not see the need to ban a fifth champion. This would make the missing data point dependent on the actual value of the missing data point itself, which makes this NMAR. To make this column missing at random (MAR), we can collect more data on whether or not the total 5 bans for each team were complete and name the new column `bancompleteness`. With this new column, the `ban5` column will no longer be dependent on the actual value itself, but it will depend on `bancompleteness` instead. 


### Missingness Dependency
When we first began exploring this dataset, we were interested in overall data and data collected at the 15 minute mark. We noticed many of the columns corresponding to data collected at this time mark had missing values, so we decided to investigate the missingness dependency of the column `killsat15`.

The first column we tested missingness against was the column `datacompleteness`.
Null Hypothesis: Distribution of `'datacompleteness'` when `'killsat15'` is missing is the same as the distribution of `'datacompleteness'` when `'killsat15'` is not missing
Alternative Hypothesis: Distribution of `'datacompleteness'` when `'killsat15'` is missing is *not* the same as the distribution of `'datacompleteness'` when `'killsat15'` is not missing
Included below is the graph portraying the distribution of `datacompleteness` when `killsat15` is and is not missing.

<iframe src="assets/missingness-plots/miss-data-completeness.html" width=800 height=375 margin-left="auto" margin-right="auto" frameBorder=0></iframe>

We performed a permutation test to determine whether or not the missingness of `killsat15` was dependent on `datacompleteness` using total variation distance (TVD) as our test statistic. Below is the plot which illustrates the empirical distribution of the TVDs, with our observed TVD marked as a red vertical line.  

<iframe src="assets/missingness-plots/fig-empirical-dc.html" width=800 height=425 frameBorder=0></iframe>

From here, we calculated the p-value by comparing the observed TVD to the TVDs found through permutation testing, and the resulting p-value was 0. Since our p-value is less than the significance level of 0.05, we reject the null hypothesis, and therefore it seems that missingness in `killsat15` is dependent on `datacompleteness`.

The second column we tested missingness against was the column `position`. 
Null Hypothesis: Distribution of `'position'` when `'killsat15'` is missing is the same as the distribution of `'position'` when `'killsat15'` is not missing
Alternative Hypothesis: Distribution of `'position'` when `'killsat15'` is missing is *not* the same as the distribution of `'position'` when `'killsat15'` is not missing
Following the same steps as above, we plotted the distribution of `position` when `killsat15` is and is not missing. Then, we carried out a permutation test using TVD as the test statistic again.

<iframe src="assets/missingness-plots/miss-position.html" width=800 height=375 frameBorder=0>
<iframe src="assets/missingness-plots/fig-empirical-position.html" width=800 height=425 frameBorder=0></iframe>

By comparing the TVDs found through permutation testing to the observed TVD, we found the resulting p-value was 1. Since our p-value is greater than the significance level of 0.05, we fail to reject the null, and therefore it seems that the missingness in `killsat15` is *not* dependent on `position`

## Hypothesis Testing

After doing our exploratory data analysis, we chose to answer the general question: do all roles have the same impact overall? Our null hypothesis is all roles have the same impact overall. Our alternative hypothesis is all roles do not have the same impact overall.
- Test statistic: total variation distance (TVD)
- Significance level: 5% (0.05)
- Impact variables: KDA, damage to champions, total gold

We used the total variation distance for this test because we are comparing the difference between the different positions (top, jng, mid, bot, sup), which are categorical distributions. We chose the most commonly used significance level because a p-value less than 0.05 is statistically significant. It indicates strong evidence against the null hypothesis since there is less than a 5% probability that the null hypothesis is correct. We decided to not use `total cs` for our hypothesis test because total cs correlates with the total gold a player has.

We did three separate hypothesis tests for the three impact variables: KDA, damage to champions, and total gold. 

**Results for KDA:** 
- P-value: 0.0
- Conclusion: since the p-value is less than the significance level of 0.05, we reject the null hypothesis. All roles do not have the same average KDA z-score.

<iframe src="assets/hypo-test-kda.html" width=800 height=425 frameBorder=0></iframe>

**Results for Damage to Champions:**
- P-value: 0.0
- Conclusion: since the p-value is less than the significance level of 0.05, we reject the null hypothesis. All roles do not have the same average damage to champions z-score.

<iframe src="assets/hypo-test-dmg.html" width=800 height=425 frameBorder=0></iframe>

**Results for Total Gold:**
- P-value: 0.0
- Conclusion: since the p-value is less than the significance level of 0.05, we reject the null hypothesis. All roles do not have the same average total gold z-score.

<iframe src="assets/hypo-test-gold.html" width=800 height=425 frameBorder=0></iframe>

Therefore, we reject the null hypothesis—all roles do not have the same impact overall. Since we know that all roles do not have the same impact overall, we shifted our focus to only look at one variable (damage to champions). We chose to use damage because we believe that the KDA statistic may not be a good statistic to use to compare as seen under [Interesting Aggregates](#interesting-aggregates). For example, a player could be focused on by the opposing team because they were being impactful. As a result, this would lower their KDA since that player could have a lot of deaths. We also did not use total gold because the gold obtained by players is used to buy items to deal more damage to the opposing team. Hence, we selected `damagetochampions` as the variable to use. 

Looking at the box plot titled: *Distribution of Damage to Champions Z-scores per Position* under [Bivariate Analysis](#bivariate-analysis), we notice that middle and bottom seem to have the highest mean z-scores for damage to champions. We then asked a follow-up question: which role has more impact overall for damage to champions—bottom or middle? We conducted a one-tailed hypothesis test. Our new null hypothesis is bottom and middle have the same average z-score for damage to champions. Our new alternative hypothesis is bottom has a higher average z-score for damage to champions than middle. 
- Test statistic: Difference in Mean
- Significance Level: 5% (0.05)
- Impact variable: damage to champions

We used the difference in mean as our test statistic because we are comparing quantitative distributions and the mean is not the same for the two distributions. We used the same significance level as stated above. 

**Results:**
- P-value: 0.0
- Conclusion: since the p-value is less than the significance level of 0.05, we reject the null hypothesis. Bottom and middle do not have the same average z-score for damage to champions. 

<iframe src="assets/hypo-test-dtc-diff.html" width=800 height=425 frameBorder=0></iframe>
