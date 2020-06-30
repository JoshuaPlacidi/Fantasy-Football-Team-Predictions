# Predicting Fantasy Football Teams Using Machine Learning
*By minimising human bias and focusing purely on statistics can a system be trained to predict high performing fantasy football teams?*

## Premise
This is a statistical study analysing the performance of varouis regression based machine learning algoirthms in their ability to predict the performance of fantasy football players in the English Premier League. In this study data structure and formatting, machine learning regression algorithms and optimisiation algorithms are all experimented with in order to create a final system capable of selecting a high performing fantasy football team for a given set of matches.

This project is conducted in the Fantasy Premier League enviroment, the offical fantasy football application of the Premier League, information of the rules of the game can be found [here](https://www.premierleague.com/news/1252542).

## Results overview
An overview of the final results of the study are summarised here for convience. This file continues, outlining the pipeline of the system and concluding with an indepth analysis of the study and results.

The graph below shows prediction performance for the 2019/20 season, for gameweeks 4-29 (gameweeks currently played).

![1920 Season Results](https://raw.githubusercontent.com/JoshuaPlacidi/FPL-Predictions/master/Results/Graphs/1920_results_graph.png "1920 Season Results")


The Dream Team represents the highest possible score of an FPL legal team for each gameweek; due to the unpredictable nature of sports, predictions will fall significantly short of this score. FPL User Average shows the average score of all nearly 7.5M human players for each gameweek, the Final System shows the score of the system for each gameweek. We can see the system consistantly out performed the average FPL user score, when plotted on a cummulative frequency graph the difference is even more apparent.

![1920 Season Cummulative Sum Results](https://raw.githubusercontent.com/JoshuaPlacidi/FPL-Predictions/master/Results/Graphs/1920_cumsum_results_graph.png "1920 Season Cummulative Sum Results")


## Approach

Linear regression models were used to generate predictions for the number of fantasy points each player would score for a given week. An optimal performing team was then selected from the generated predictions using linear optimisation. The selected team must abide by all [FPL restrictions](https://fantasy.premierleague.com/help/rules). The performance of the selected team was then analysed comparing the real score to the score the system predicted, here we are concerned with maximising actual performance rather then reducing the error between predicted and actual performance (though the more accurate the model the better).

## Data
Data was sourced from the official FPL API via the [vaastav repository](https://github.com/vaastav/Fantasy-Premier-League). Statistics from the past 4 seasons of the Premier League were combined into a single csv with a unique entry for each player for each game they played. A rolling dataset was then constructed where for each entry the sum of statistics from the prevouis *n* weeks were summed. The optimal value of n was found by testing the accuracy of the models with *n* values from 1 to 9.

// Insert Image

Results showed that *n = 3* gave the smallest error in predictions. So given a player and a game, performance statistics (goals scored, assists, minutes played ect...) from the players 3 prior games are used to make predictions for the given game.

## Prediction Models

Four regression aglorithms were compared in their ability to accurately model fantasy football performance. FPL players are split into four position groups: goalkeepers, defenders, midfielders and forwards. To generate accurate predictions individual models were created for each position. Each algorithm was trained and tested on positional rolling datasets measuring the accuracy using k-fold cross validation (k=10) and the following results were generated:

// Insert Image

The results showed that linear regression dominates for each positional subset. Linear regression positional models were fine tuned to optimise performance for each position and then finialised. The accuracy of the final positional models was test again using k-fold cross validtion (k=10) with model accuracy being recorded:

// Insert Image

The results show that all models achieved an average prediction error of less than 2 points.

## Selecting Optimal Teams

Once predictions had been generated for each player for each game, we then need to create an algorithm capable of selecting a subset of players for a given gameweek that obey all FPL team restrictions and in which the sum of points of all players is maximised. After experimenting with different approachs, a [linear optimisation](https://en.wikipedia.org/wiki/Linear_programming) solution was implemented. The final algorithm takes a list of players and their predicted points for a given gameweek and returns the optimal FPL legal team.

// Insert Image of selection for an individual gameweek

## Results

We can now generate team selections for inidividual gameweeks and compare how well they actually performed. To test the system a simulation over the already played 2019/20 gameweeks (4-29) and measure the results.
