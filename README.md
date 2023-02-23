---
# LoL-role-impact
Data analysis on the impact of different roles from League of Legends for a DSC80 project at UCSD. 
---

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



---


## Cleaning and EDA

### Data Cleaning
The first step of our data cleaning process was to check for missingness in our dataset. We found that the column damagetochampions had missing values when the value for datacompleteness was `partial`. We then dropped the rows where `datacompleteness` is partial, which got rid of all missing values in our dataset.

The second step of our data cleaning process was to standardize our statistics in the dataset. We transformed all our data to z-scores for each game. We did this by creating a helper function that calculates the z-score using the z-score formula:

$$z(x_i) = \frac{x_i - \text{mean of } x}{\text{SD of }x}$$

We then grouped by `gameid` and transformed the numerical statistics using the z-score helper function. 



### Univariate Analysis

### Bivariate Analysis

### Interesting Aggregates

---

## Assessment of Missingness

### NMAR Analysis

### Missingness Dependency

---

## Hypothesis Testing

---