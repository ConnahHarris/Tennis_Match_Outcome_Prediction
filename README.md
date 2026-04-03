# Historical Tennis Match Outcome Prediction Project

<p align="center">
  <img src="Figures/Robots_Playing_Tennis.png" width="400">
</p>

## Project Overview

This project develops a time-aware machine learning pipeline to predict ATP tennis match outcomes using historical match data. 

The focus of the project is not only on predictive performance, but on building a realistic and robust modelling framework. This includes handling messy real-world data, engineering sequential player features, and ensuring that all features and evaluations respect the chronological nature of the data.

## Data

The data is sourced from the publicly available [Jeff Sackmann ATP database](https://github.com/JeffSackmann), which contains detailed match-level data for professional tennis matches. 

The dataset used in this project consisted of approximately 100,000 matches and includes a range of inconsistencies and missing values. As a result, a significant portion of the work involved cleaning, validating and restructuring the data before it could be used for analysis and modelling.

## Feature Engineering

The raw dataset was first cleaned by identifying and removing or correcting anomalous entries. Many of these anomalies were due to missing data that could not be reliably recovered from external sources such as Wikipedia or official ATP records. In other cases, clearly erroneous values were identified (e.g. a player listed with a height of 3 cm), which were corrected or excluded.

Following data cleaning, all matches were ordered chronologically using the tournament start date and round ("Q1", "Q2", "Q3", "R128", "R64", "R32", "R16", "QF", "SF", "F"). This chronological ordering is essential to ensure that all engineered features are based only on information available prior to each match, preventing data leakage.

This chronological structure also forms the foundation for all subsequent feature engineering, ensuring that player statistics such as Elo ratings, recent form, and head-to-head records are computed in a time-consistent manner.



### ELO Calculations
The first feature engineered was player's ELO ratings, a tool is most commonly used in chess to rank players based on their relative ability. Every player without any previous match history has their ELO set to 1500.

The expected probability of Player 1 winning a match ($P_1$) is calculated as:

$P_1 = \frac{1}{1 + 10^{(E_2 - E_1)/400}}$

Where $E_1$ and $E_2$ represent the pre-match ELO ratings of Player 1 and Player 2, respectively.

Following the match, Player's ELO ratings are updated using:

$E_1' = E_1 + K \cdot (S_1 - P_1)$

Where $S_1 \in \{0,1\}$ indicates if Player 1 won the match, and $K$ is a scaling factor set to 32.  

A surface ELO was calculated using the same methodology, but only calculating this metric for each player on a specific surface. 

The data was plotted to look for the prediction power of these newly engineered features, shown in figure 1. 


<table align="center">
  <tr>
    <td><img src="Figures/elo_difference.png" width="500"></td>
  </tr>
  <tr>
    <td><img src="Figures/surface_elo_difference.png" width="500"></td>
  </tr>
</table>

<p align="center">
  <em>
    Figure 1: Distribution of tennis match outcomes based on Elo rating differences. 
    The plots show how frequently the higher-rated player (by Elo) wins, considering 
    both overall Elo differences and surface-specific Elo differences.
  </em>
</p>
