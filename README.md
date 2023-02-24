
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

League of Legends is a competitive game, so it is natural for players to want to dominate their games and achieve victory. However, what is the best way to obtain this goal? Two teams, consisting of five players each, battle to destroy the opposing team's center base known as the Nexus. Each team's players fulfill a specific role on the team - selecting to play one of the five roles: top, middle, bottom, support, or jungle. If players want to carry their team to victory, they might want to pick the best role to do so. **Thus, we decided to conduct our data analysis on which role is the most impactful in the overall duration of the game.** \*For our purposes, impact will be defined through the statistics of KDA, total gold, and damage to champions

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

In the bar graphs provided above, each graph corresponds to a different role or position, with each bar on the x-axis representing a different statistic while the y-axis indicates the average z-score. Upon taking a closer look at these graphs, the first thing we noticed was that roles such as middle and top had positive average z-scores for all statistics while top had a combination of positive and negative and roles such as support and jungle had average z-scores around or below zero. Based on this observation, the data seems to indicate middle and top roles have more impact in games overall than bottom, support, and jungle. 


### Bivariate Analysis
In this section, we wanted to continue comparing the statistics’ z-scores across all positions. We combined all positions’ average z-scores into one bar graph to make comparisons easier, as well as creating box plots to perform further analysis on the distribution of z-scores for each statistic. 

<iframe src="assets/bivariate-plot-all.html" width=800 height=400 frameBorder=0></iframe>

The graph above contains each positions’ average z-score across all statistics. Along the x-axis, it specifies which position the bars belong to, and the y-axis once again indicates the average z-score. The bins for each position are color coded based on the legend, with blue corresponding to KDA, red to total gold, green to total creep score, and purple to damage to champions. By plotting all positions side-by-side, we were able to deduce that bottom has the overall highest average z-scores for all statistics, while support has the lowest average z-score for all statistics besides KDA. 

The following box plots illustrate each position’s distribution of z-scores for KDA and damage to champions respectively. The x-axis specifies which position the box plot corresponds to and the y-axis represents the z-score. 

<iframe src="assets/bivariate-plot-kda.html" width=800 height=400 frameBorder=0></iframe>

Taking a closer look at this plot, we find that all positions seem to have relatively the same spread in z-scores, with the median ranging from around -0.1 to -0.5. The boxplot for top seems to be slightly lower compared to the others, and top and support are the only roles containing outliers. Once again, we notice the roles of middle and bottom have higher z-scores, as their box plots are higher than the other positions’.

<iframe src="assets/bivariate-plot-dc.html" width=800 height=400 frameBorder=0></iframe>

Unlike the previous boxplot, there is a lot more variance in the distribution of z-scores across each role. Top, middle, and bottom all seem to have similarly wide ranges, while jungle and support have much smaller ranges. Furthermore, jungle and support have the lowest medians at -0.55 and -1.23 respectively, and they also have the most outlying data points. On the other hand, middle and bottom continue to have the highest medians, which further implies that these two roles may be the most impactful overall. 


### Interesting Aggregates



## Assessment of Missingness

### NMAR Analysis
We believe that the column `ban5` in our dataset is not missing at random (NMAR). Some values are missing in this column with no apparent pattern, so we think that some teams have either forgotten to ban or did not see the need to ban a fifth champion. This would make the missing data point dependent on the actual value of the missing data point itself, which makes this NMAR. To make this column missing at random (MAR), we can collect more data on whether or not the total 5 bans for each team were complete and name the new column `bancompleteness`. With this new column, the `ban5` column will no longer be dependent on the actual value itself, but it will depend on `bancompleteness` instead. 


### Missingness Dependency



## Hypothesis Testing

