# Predicting Fantasy Football Teams Using Machine Learning
*By minimising human bias and focusing purely on statistics can a system be made to predict high performing fantasy football teams?*

## Premise
This is a statistical study analysing using machine learning in an attempt to develope a system which can use statistics to select high performing fantasy football teams. Regression-based machine learning algorithms are compared in their ability to predict the performance of football players in the English Premier League. An optimisation algorithm is then used to select a final team from these predictions. The final system is capable of consistently selecting high performing fantasy football teams.

This project is conducted in the Fantasy Premier League environment, the official application of the Premier League, information of the rules of the game can be found [here](https://fantasy.premierleague.com/help/rules).

## Results overview
The final results of the study are briefly summarised here for convenience. This document continues, outlining and analysing the pipeline of the system.

The following results are from a simulation run over the currently played matches of 2019/20 Premier League Season. A breakdown of the simulation results with comparisons to the average user score and dream team score can be found [here](https://github.com/JoshuaPlacidi/FPL-Predictions/blob/master/Results/1920_results.csv "1920 results in csv format"). The graph below shows prediction performance for the 2019/20 season, for gameweeks 4-29 (29 gameweeks currently played, the system requires 3 prior games to have been played before predictions can be made).

![1920 Season Results](https://raw.githubusercontent.com/JoshuaPlacidi/FPL-Predictions/master/Results/Graphs/1920_results_graph.png "1920 Season Results")

The dream team represents the highest possible score of an FPL legal team for each gameweek; due to the inherit unpredictable nature of sports, our generated selections will fall significantly short of this score. FPL Average shows the average score of all over 7.5M human players for each gameweek, the Final System shows the score of the system's selected team for each gameweek. We can see the Final System consistently out performed the average FPL user score, when plotted on a cumulative frequency graph the difference is even more apparent.

![1920 Season Cumulative Sum Results](https://raw.githubusercontent.com/JoshuaPlacidi/FPL-Predictions/master/Results/Graphs/1920_results_cumsum_graph.png "1920 Season Cumulative Sum Results")

Over the gameweek range of 4-29 the Final System achieved a total score of 1518 (58.38 mean points per GW), the FPL Average score over the same period was 1262 (48.54 mean points per GW). This is a large difference however it should be noted that the system did not take into account transfers and was selecting a new team from scratch each week, whereas FPL users did have to abide by transfer restrictions.

## Approach

Linear regression models were used to generate predictions for the number of fantasy points each player would score for a given gameweek. An optimal performing team was then selected from the generated predictions using linear optimisation. The selected team must abide by all [FPL restrictions](https://fantasy.premierleague.com/help/rules). The systems selections are then analysed with the real results and are compared to the average FPL user score.

## Data
Data was sourced from the official FPL API via the [vaastav repository](https://github.com/vaastav/Fantasy-Premier-League). Statistics from the past 4 seasons of the Premier League were combined into a single [file](https://github.com/JoshuaPlacidi/FPL-Predictions/blob/master/Data/Player_Data.csv) with a unique entry for each player for each game. A rolling dataset was then generated where for each entry the sum of performance metrics (goals scored, assists, goals conceded etc) from the previous *n* weeks were summed. An optimal *n* value of 3 was found by testing model accuracy with *n* values from 1 to 9. So each entry contains information about the fixture (opponent difficult, home/away etc) and data about how well the player had performed in their previous 3 games.

## Prediction Models

Four regression algorithms were compared in their ability to accurately model and predict fantasy football performance. FPL splits players into four position groups: goalkeepers, defenders, midfielders and forwards, each group earns points in different ways. The accuracy of each algorithm for each position was tested over the range of *n* values (*n* = number of previous games considered). Below is an example of the results for the defenders subset:

![Defender Model Comparison](https://raw.githubusercontent.com/JoshuaPlacidi/FPL-Predictions/master/Results/Graphs/defender_model_comparison.png "Defender Model Comparison")

After tests had been run of all positional datasets it was concluded that linear regression with an *n* value of 3 provided the most accurate model for each position. The models were then fine tuned and tested using the 2016-17, 2017-18 and 2018-19 seasons as training data and the current 2019-20 season as test data producing the following error:

| Position | Mean Error
--- | ---
Goalkeepers | 2.342
Defenders | 2.308
Midfielders | 1.764
Forwards | 2.134
All | 2.043

We can see that predictions for midfielders are the most accurate, likely due to it being the most populous positions. Goalkeepers have the least data available resulting in high predictions error. The mean prediction error over all players is 2.043.

## Selecting Optimal Teams

Once predictions have been generated for each player in a gameweek, we then need to create an algorithm capable of selecting a subset of players (a team) that obey all FPL [restrictions](https://fantasy.premierleague.com/help/rules) and in which the sum of predicted points of all players is maximised. After experimenting with different approaches, a [linear optimisation](https://en.wikipedia.org/wiki/Linear_programming) solution was implemented. The final algorithm takes a list of players and their predicted points and returns the optimal scoring FPL legal team. For example when given data for the 29th gameweek of the 2019/20 season, the following team is outputted:

![Gameweek 29 Selected Team](https://github.com/JoshuaPlacidi/FPL-Predictions/blob/master/Results/gw_29_selected_team.PNG?raw=true "Gameweek 29 Selected Team")

The predicted and the real scores of individual players can be easily compared, along with team-wide statistics. A range of gameweek predictions can also be generated, the image below shows the output from gameweeks 25-29 of the 2019/20 Premier League season:

![Gameweek 25-29 Selected Teams](https://github.com/JoshuaPlacidi/FPL-Predictions/blob/master/Results/gw_25_to_29_selected_teams.PNG?raw=true "Gameweek 25-29 Selected Teams")

We can see the predicted and real score for each gameweek and the total combined scores with the errors. Over significant ranges the average (mean) error stays around 13 points which, with a mean real score of 58.38, gives a percentage error of around 22%.

## Summary

The aim of this study was to create a system that could predict high performing fantasy football teams. Using FPL data, linear regression and linear optimisation a final system has been produced which satisfies this aim. Further work can be conducted to alter the team selection algorithm so that it takes as input an already selected team and selects a new optimal team while taking into account transfer restrictions and fines. With this improvement the system would be able to effectively manage its own team and make selections week by week, like human players, that slowly let it evolve its team over a range of gameweeks.

## References

This study was originally conducted as part of my undergraduate dissertation, below I have included links to the most impactful and relevant work that influenced the project:

* *Interactive Tools for Fantasy Football Analytics and Predictions using Machine Learning* Neena Parikh 2014: https://dspace.mit.edu/handle/1721.1/100687
*  *Time Series Modelling for Dream Team in Fantasy Premier League* Akhil Gupta 2017: https://arxiv.org/abs/1909.12938
* *Competing with Humans at Fantasy Football: Team Formation at Large Partially-Observable Domains* Tim Matthews, Sarvapali D. Ramchurn, Georgios Chalkiadakis 2013: http://www.intelligence.tuc.gr/~gehalk/Papers/fantasyFootball2012cr.pdf