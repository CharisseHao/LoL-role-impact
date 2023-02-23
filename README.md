
# Role Impact Analysis (League of Legends)
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
Welcome to Summoner’s Rift! League of Legends (LoL) is a popular online multiplayer battle arena game, with millions of active players worldwide. The game's massive player base makes it an excellent source of data for researchers and data scientists interested in exploring various aspects of the game, from player behavior to gameplay mechanics. 

League of Legends is a competitive game, so it is natural for players to want to dominate their games and achieve victory. However, what is the best way to obtain this goal? Two teams, consisting of five players each, battle to destroy the opposing team's center base known as the Nexus. Each teams' players fulfill a specific role on the team - selecting to play one of the five roles: jungle, middle, bottom, or support. If players want to carry their team to victory, they might want to pick the best role to do so. **Thus, we decided to conduct our data analysis on which role is the most impactful in the overall duration of the game.** 

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

<br />

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
For our univariate analysis, we decided to focus on investigating the distribution of each statistict’s z-scores depending on the position column. In order to generate these plots, we grouped by position before calculating the z-score for each statistic we were investigating. These included: KDA, total gold, total creep score, and damage to champions. 

<p float = 'left'> 
    <iframe src="assets/univariate-plots/uni-fig-mid.html" width=410 height=325 frameBorder=0></iframe>
    <iframe src="assets/univariate-plots/uni-fig-bot.html" width=410 height=325 frameBorder=0></iframe> 
</p>
<p float = 'left'> 
    <iframe src="assets/univariate-plots/uni-fig-top.html" width=410 height=325 frameBorder=0></iframe>
    <iframe src="assets/univariate-plots/uni-fig-sup.html" width=410 height=325 frameBorder=0></iframe> 
</p>
<p float = 'left'> 
    <iframe src="assets/univariate-plots/uni-fig-jng.html" width=410 height=325 frameBorder=0></iframe>
</p>


### Bivariate Analysis

<iframe src="assets/bivariate-plot-all.html" width=800 height=400 frameBorder=0></iframe>
<iframe src="assets/bivariate-plot-kda.html" width=800 height=400 frameBorder=0></iframe>
<iframe src="assets/bivariate-plot-dc.html" width=800 height=400 frameBorder=0></iframe>


### Interesting Aggregates



## Assessment of Missingness

### NMAR Analysis



### Missingness Dependency



## Hypothesis Testing

