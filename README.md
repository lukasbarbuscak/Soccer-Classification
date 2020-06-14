# Predicting Playing Positions of Soccer Players

## Introduction

This project examines attributes which soccer players need to have in order to play at specific positions on the pitch. Using physical, mental, and technical attributes, as well as variables such as financial information or jersey number, I will build a model that will clasify a player to a position based on the aforementioned features. The dataset used to tackle this problem is the "FIFA 20 complete player dataset" (publicly available at https://www.kaggle.com/stefanoleone992/fifa-20-complete-player-dataset). This video game uses real players and uses scouts around the globe to assess players' attributes as accurately as possible.

## Data Cleaning
The original dataset contained 104 columns, out of which some were not relevant for my analysis, and some were duplicate columns. After the initial cleaning, I further looked at the data, and found that my target variable, "positions", contained more than one position. For the purpose of this analysis, I only kept the first, "primary" position.

I then checked for missing values in the dataset. The only column containing missing values was "team jersey number". The players who did not have a number also had "value_eur" and "wage_eur" set to 0. This meant they were players without a club (free agents), and were available to sign with anyone for free. I checked whether these players could have been considered outliers:



I set the numbers of these players to 0, since there were no other values that are not imputed set to 0, and therefore it woudn't have interfered with this classification problem. 
