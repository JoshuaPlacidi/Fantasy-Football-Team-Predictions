# Predicting Fantasy Football Teams Using Machine Learning
*By minimising human bias and focusing purely on statistics can a system be trained to predict high performing fantasy football teams?*

## Premise
This is a statistical study analysing the performance of various regression based machine learning algoirthms in their ability to predict the performance of fantasy football players in the English Premier League. In this study data structure and formatting, machine learning regression algorithms and optimisiation algorithms are all experimented with in order to create a final system capable of selecting a high performing fantasy football team for a given set of matches.

This project is conducted in the Fantasy Premier League enviroment, the offical fantasy football application of the Premier League, information of the rules of the game can be found [here](https://www.premierleague.com/news/1252542).

## Results overview
An overview of the final results of the study are briefly summarised here for convience. This document continues, outlining the pipeline of the system and concluding with an detailed analysis of the study and results.

The following results are from a simulation run over the (currently played) 2019/20 Premier League Season. A breakdown of the simulation results with comparisons to the average user score and dream team score can be found [here](https://github.com/JoshuaPlacidi/FPL-Predictions/blob/master/Results/1920_results.csv "1920 results in csv format"). The graph below shows prediction performance for the 2019/20 season, for gameweeks 4-29 (29 gameweeks currently played, the system requires 3 prior games to have been played before predictions can be made).

![1920 Season Results](https://raw.githubusercontent.com/JoshuaPlacidi/FPL-Predictions/master/Results/Graphs/1920_results_graph.png "1920 Season Results")

The Dream Team represents the highest possible score of an FPL legal team for each gameweek; due to the inherit unpredictable nature of sports, our generated selections will fall significantly short of this score. FPL Average shows the average score of all nearly 7.5M human players for each gameweek, the Final System shows the score of the system for each gameweek. We can see the system consistantly out performed the average FPL user score, when plotted on a cummulative frequency graph the difference is even more apparent.

![1920 Season Cumulative Sum Results](https://raw.githubusercontent.com/JoshuaPlacidi/FPL-Predictions/master/Results/Graphs/1920_results_cumsum_graph.png "1920 Season Cumulative Sum Results")

Over the gameweek range of 4-29 the predictive system acheived a total score of 1518 (58.38 mean points per GW), the average FPL user score over the same peroid was 1262 (48.54 mean points per GW). This is a large difference however it should be noted that the system did not take into account transfers and was selecting a new team from scratch each week, whereas FPL users did have to abide by transfer restrictions.

## Approach

Linear regression models were used to generate predictions for the number of fantasy points each player would score for a given gameweek. An optimal performing team was then selected from the generated predictions using linear optimisation. The selected team must abide by all [FPL restrictions](https://fantasy.premierleague.com/help/rules). The systems selections are then analysed with the real results and are compared to the average FPL user score.

## Data
Data was sourced from the official FPL API via the [vaastav repository](https://github.com/vaastav/Fantasy-Premier-League). Statistics from the past 4 seasons of the Premier League were combined into a single [file](https://github.com/JoshuaPlacidi/FPL-Predictions/blob/master/Data/Player_Data.csv) with a unique entry for each player for each game. A rolling dataset was then generated where for each entry the sum of performance metrics (goals scored, assists, goals conceeded etc) from the previous *n* weeks were summed. An optimal *n* value of 3 was found by testing model accuracy with *n* values from 1 to 9. So each entry contains information about the fixture (opponet difficult, home/away etc) and data about how well the player had performed in their previous 3 games.

## Prediction Models

Four regression aglorithms were compared in their ability to accurately model and predict fantasy football performance. FPL splits players into four position groups: goalkeepers, defenders, midfielders and forwards, each group earns points in different ways. The accuracy of each algorithm for each position was tested over the range of *n* values (*n* = number of prevouis games considered). Below is an example of the results for the defenders subset:

![Defender Model Comparison](https://raw.githubusercontent.com/JoshuaPlacidi/FPL-Predictions/master/Results/Graphs/defender_model_comparison.png "Defender Model Comparison")

After tests had been run of all positional datasets it was concluded that linear regression with an *n* value of 3 provided the most accurate model for each position. The models were then fine tuned and tested using the 2016-17, 2017-18 and 2018-19 as training data and the current 2019-20 season as test data producing the following error:

| Position | Mean Error
--- | ---
Goalkeepers | 2.342
Defenders | 2.308
Midfielders | 1.764
Forwards | 2.134
Total | 2.043



## Selecting Optimal Teams

Once predictions had been generated for each player for each game, we then need to create an algorithm capable of selecting a subset of players for a given gameweek that obey all FPL team restrictions and in which the sum of points of all players is maximised. After experimenting with different approachs, a [linear optimisation](https://en.wikipedia.org/wiki/Linear_programming) solution was implemented. The final algorithm takes a list of players and their predicted points for a given gameweek and returns the optimal FPL legal team.

// Insert Image of selection for an individual gameweek

## Results

We can now generate team selections for inidividual gameweeks and compare how well they actually performed. To test the system a simulation over the already played 2019/20 gameweeks (4-29) and measure the results.
