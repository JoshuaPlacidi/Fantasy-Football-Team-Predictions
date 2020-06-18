# FPL-Predictions
*By minimising human bias and focusing on football statistics can a machine learning model be trained to consistantly predict high performing fantasy football teams?*

## Premise
This is a statistical study analysing the performance of varouis regression based machine learning algoirthms in their ability to predict the performance of fantasy football players in the English Premier League. In this study data structure and formatting, machine learning regression algorithms and optimisiation algorithms are all experimented with in order to create a final system capable of selecting a high performing fantasy football team for a given set of matches.

This project is conducted in the Fantasy Premier League enviroment, the offical fantasy football application of the Premier League, information of the rules of the game can be found [here](https://www.premierleague.com/news/1252542).

## Results overview
An overview of the final results of the study are summarised here for convience. This file continues, outlining the pipeline of the system and concluding with an indepth analysis of the study and results.

The final system was tested on the 2019/20 Premier League season (gameweeks 4-29) with positive results collected.

## Methods used

4 regression algorithms were compared in their ability to model fantasy football performance:
 - Multiple Linear Regression
 - Ridge Regression (CV)
 - Lasso Regression (CV)
 - Elastic Net Regression (CV)
*CV: Cross validation used to determine alpha values*
The models were used to generate predictions for the number of fantasy points each player would score for a given week. A team was selected from the generated predictions using linear optimisation. The selected team must abide by the restrictions imposed by the [FPL](https://fantasy.premierleague.com/help/rules). The performance of the selected team in then analysed comparing the real score to the score the model would predict, here we are concerned with maximising actual performance rather then reducing the error between predicted and actual performance (though the more accurate the model the better).

## Data
Data was sourced from the official FPL API via the [vaastav repository](https://github.com/vaastav/Fantasy-Premier-League). Statistics from the past 4 seasons of the Premier League were combined into a single csv with a unique entry for each player for each game they played. Each entry includes a range of statistics detailing  performance and giving fixture information. A rolling dataset was then constructed where for each entry the sum of certain statistics from the prevouis n weeks were summed. The optimal value of n was found by testing the accuracy of the model between with values between 1 and 9 prevouis games considered.

// Insert image

Results showed that n = 3 gave the smallest error in predictions. So given a player and a specific match we have access to the sum of their performance statistics (goals scored, assists, minutes played ect...) from their 3 prior games.

