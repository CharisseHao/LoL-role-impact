
# LoL-role-impact
Data analysis on the impact of different roles from League of Legends for a DSC80 project at UCSD. 


## Introduction
Welcome to Summoner’s Rift! League of Legends (LoL) is a popular online multiplayer battle arena game, with millions of active players worldwide. The game's massive player base makes it an excellent source of data for researchers and data scientists interested in exploring various aspects of the game, from player behavior to gameplay mechanics. 

League of Legends is a competitive game, so it is natural for players to want to dominate their games and achieve victory. However, what is the best way to obtain this goal? In League of Legends, a team consists of five players, who each fulfill a specific role on the team. The five roles are top, jungle, middle, bottom, and support. If players want to carry their team to victory, they might want to pick the best role to do so. **Thus, we decided to conduct our data analysis on which role is the most impactful in the overall duration of the game.** 

The dataset we used to conduct this analysis is the League of Legends Esports Stats dataset provided by Oracle's Elixir, a website that provides advanced statistics and analysis tools for the game. This dataset contains a comprehensive collection of data from professional League of Legends matches, including detailed information on player and team performance, game events, and match outcomes. The dataset contains information on thousands of matches, covering various tournaments and competitions.

### Dataset Statistics and Definitions:
The dataset has in total of 149232 rows and 123 columns. We cleaned the dataset to only use the data we need for our analysis, so we used 124360 rows and 9 columns. Below is the definition of the columns we used from this dataset.

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


| **Positions Abbreviation** | **Meaning** |
| -------------------------- | ----------- |
| *top*                      | top         |
| *jng*                      | jungle      |
| *mid*                      | middle      |
| *bot*                      | bottom      |
| *sup*                      | support     |


## Cleaning and EDA

### Data Cleaning
The first step of our data cleaning process was to check for missingness in our dataset. We found that the column damagetochampions had missing values when the value for datacompleteness was `partial`. We then dropped the rows where `datacompleteness` is partial, which got rid of all missing values in our dataset.

The second step of our data cleaning process was to standardize our statistics in the dataset. We transformed all our data to z-scores for each game. We did this by creating a helper function that calculates the z-score using the z-score formula:

Z = \frac{X-\mu}{\sigma}

We then grouped by `gameid` and transformed the numerical statistics using the z-score helper function. 
<table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>gameid</th>      <th>datacompleteness</th>      <th>position</th>      <th>damagetochampions</th>      <th>totalgold</th>      <th>total cs</th>      <th>KDA</th>    </tr>  </thead>  <tbody>    <tr>      <th>0</th>      <td>ESPORTSTMNT01_2690210</td>      <td>complete</td>      <td>top</td>      <td>0.319151</td>      <td>0.470574</td>      <td>0.587278</td>      <td>-0.947893</td>    </tr>    <tr>      <th>1</th>      <td>ESPORTSTMNT01_2690210</td>      <td>complete</td>      <td>jng</td>      <td>-0.283245</td>      <td>-0.404958</td>      <td>-0.399444</td>      <td>-0.892339</td>    </tr>    <tr>      <th>2</th>      <td>ESPORTSTMNT01_2690210</td>      <td>complete</td>      <td>mid</td>      <td>0.091917</td>      <td>-0.123676</td>      <td>0.135526</td>      <td>-0.704843</td>    </tr>    <tr>      <th>3</th>      <td>ESPORTSTMNT01_2690210</td>      <td>complete</td>      <td>bot</td>      <td>-0.382416</td>      <td>0.310190</td>      <td>0.527837</td>      <td>-1.017335</td>    </tr>    <tr>      <th>4</th>      <td>ESPORTSTMNT01_2690210</td>      <td>complete</td>      <td>sup</td>      <td>-1.502484</td>      <td>-1.604184</td>      <td>-1.659597</td>      <td>-0.934004</td>    </tr>  </tbody></table>




### Univariate Analysis

<iframe src="assets/univariate_plot.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

### Interesting Aggregates



## Assessment of Missingness

### NMAR Analysis

### Missingness Dependency



## Hypothesis Testing

