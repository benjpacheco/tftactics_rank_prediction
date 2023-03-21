## Table of Contents

1. [Setup](#setup)
2. [Objective](#objective)
3. [Extraction](#extract)
4. [Exploratory Data Analysis](#eda)
5. [Modelling](#model)


## Setup <a name="setup"></a>

In order to access the analysis in the notebooks please follow the below steps:

1. clone this repository with `git clone https://github.com/benjpacheco/tftactics_rank_prediction.git`

2. cd into the directory folder 

3. create and activate your conda or virtualenv for this project

4. run: `pip install -r requirements.txt` in your shell

## Objective <a name="objective"></a>

The goal of this project is to predict ranking placements for TFT players. TFT is a spin-off video game based off the League of Legends world and characters created by Riot. TFT was created with inspiration of Dota 2's auto chess.

In TFT you have 8 participants per match that fight against each other by forming many combinations of different traits and items. TFT is a game about probability (luck and expected values) and economy management. You get gold each round to either roll down for a new set of units to buy or purchase experience to further your exp bar and progress to the next level. Each level allows you to set down another unit and increases the chance that certain rarities/units appear. At the max level (9) you can place up to 9 units and with certain augments/items you are allowed to place extra as well.

Players fight each other by placing a set number of units on a hexagonal board as they wish. These units fight each other automatically when a round starts. The player who defeats their opponent’s whole squad is declared the winner of a round, and when the round is over, all units on both sides are reset to their previous position to get ready for the next round. When you lose a round, you lose points represented by the health of your tactician, not your units. The more opposing pieces alive after a round ends, the greater the damage you’ll take. Players who reach zero health points are knocked out of the match regardless of which round they’re in. The last player with their tactician standing is declared the winner of an autobattler match.

TFT runs off seasons or "sets" in which every 6 months a new set appears with new mechanics and synergies along with a mid-set half way. This set is based off set 7.5 which is the mid-set of set 7. I gathered the data utilizing Riot API end points and some python code :). If you would like to see my web scraping and initial wrangling i've provided that notebook aswell in the directory.

For this analysis we will only focus on the highest tier of ranked matches, the challenger league.


## Extraction <a name="extract"></a>

You can access the jupyter notebook here: [Extraction](Ben_Pacheco_TFT_Analysis_Gathering_FINAL.ipynb)

I gathered all the data using Riot API endpoints, and only 2 years of data is availible (from 2020-2022).

## Exploratory Data Analysis <a name="eda"></a>

You can access the jupyter notebook here: [EDA](Ben_Pacheco_TFT_Analysis_EDA_FINAL.ipynb)

To explain some of the columns: 
* `combination`: column of dictionaries that show synergistic traits each participant has in the current match. Key: traits, Value: # of combined traits.
* `champion`: shows which champions each of the players set, also dictionary format. Key: champion name, Value: items, tier (tier enhance the units stats, it holds a value of 1-3)
* `match_id`: unique ID for ranked match
* `game_version`: patch version for set 7.5
* `game_length`: total game duration in seconds
* `level`: participant level (1-9 sometimes 10 if you have the special augment)
* `last_round`: last round of current participant (round before death)
* `placement`: participant ranking in the game after it finishes (1-8)
* `time_eliminated`: time spent playing per participant in seconds before being eliminated (or wins if first)
* `gold_left`: amount of gold left when a participants gets eliminated (or wins if first)

I have several ideas/questions and a goal in mind with the dataset:
* My goal is to be able to leverage a multiclass classification model in order to predict the ranking (1-8) of a participant at the end of a match
* What correlations exist between the features?
* Engineer more features if possible
* What are the common compositions when a player has 2 units? 4 units? 6 units? 8 or more units?
* How much gold left does a tactician have when reaching certain levels?
* How accuratetly and efficiently can the model predict the placement of a player?
* What is the most impactful composition that determines predicting rank?
* What are the most impactful champions along with their item set ups that determines predicting rank?

## Modelling <a name="model"></a>

You can access the jupyter notebook here: [Modelling](Ben_Pacheco_TFT_Analysis_Modeling_FINAL.ipynb)

or the modeling portion I will begin with some basic models and not do any feature engineering or hyperparameter tuning. I will be using the following models:

* Logistic Regression
* Random Forest
* XGBoost (extreme gradient boosting trees)

Some questions that I wanted to answer using machine learning:

* How accuratetly and efficiently can the model predict the placement of a player?
* What is the most impactful composition that determines predicting rank?
* What are the most impactful champions along with their item set ups that determines predicting rank?

Evaluating the models:

* Accuracy, f1-score, Mean Absolute Error
* AUCROC for multiclass classification

After some evaluation of the basic models I will fine tune the models with hyperparameters then select the best performer:

* Feature Engineering
* GridsearchCV
* Optuna (TPE)

And finally re-evaluate the best forming model:

* Accuracy, f1-score, Mean Absolute Error
* AUCROC for multiclass classification