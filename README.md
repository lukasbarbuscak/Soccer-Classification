# Predicting Playing Positions of Soccer Players

## Introduction

This project examines attributes which soccer players need to have in order to play at specific positions on the pitch. Using physical, mental, and technical attributes, as well as variables such as financial information or jersey number, I will build a model that will clasify a player to a position based on the aforementioned features. The dataset used to tackle this problem is the "FIFA 20 complete player dataset" (publicly available at https://www.kaggle.com/stefanoleone992/fifa-20-complete-player-dataset). This video game uses real players and uses scouts around the globe to assess players' attributes as accurately as possible.

## Data Cleaning

The original dataset contained 104 columns, out of which some were not relevant for my analysis, and some were duplicate columns. After the initial cleaning, I further looked at the data, and found that my target variable, "positions", contained more than one position. For the purpose of this analysis, I only kept the first, "primary" position.

I then checked for missing values in the dataset. The only column containing missing values was "team jersey number". The players who did not have a number also had "value_eur" and "wage_eur" set to 0. This meant they were players without a club (free agents), and were available to sign with anyone for free. I checked whether these players could have been considered outliers:

![outliers](https://github.com/lukasbarbuscak/Soccer-Classification/blob/master/images/outliers.PNG)

Both boxplots show the free agents were not outliers. I set the numbers of these players to 0, since there were no other values that are not imputed set to 0, and therefore it woudn't have interfered with this classification problem. The final shape of the dataset was 18,278 and 48 columns.

## Exploratory Data Analysis

Before building models, I looked at the data using EDA and tried to make predictions about the relationships between the variables. First, I checked if there were any differences between players playing at different positions. I visualized the differences:

<br>

![height_weight](https://github.com/lukasbarbuscak/Soccer-Classification/blob/master/images/height_weight.PNG)

<br>

![value_wage](https://github.com/lukasbarbuscak/Soccer-Classification/blob/master/images/value_wage.PNG)

<br>

![foot_skill](https://github.com/lukasbarbuscak/Soccer-Classification/blob/master/images/foot_skill.PNG)

Both weight and height followed a similar pattern, with goalkeepers, central defenders, and strikers being the tallest, and heaviest, and left/right midfielders, wingers and attacking midfielders the opposite. Similarly to height and weight, more expensive players tended to have higher wages. The most expensive players making the most were generally centre-forwards and wingers, and the least expensive making the least were generally left/right defenders and goalkeepers. Players with a high rating of their weak foot (it is a likert scale from 1 to 5) generally tended to be centre-forwards, wingers, left/right midfielders, and central attacking midfielders. On the other hand, low rating weak-footers generally were left/central defenders, and goalkeepers. When it comes to players' skill moves, centre-forwards and left wingers tended to have the best average rating, with central defenders and goalkeepers having the lowest average ratings.

For the analysis of the particular playing attributes, it was easier to first group the abilities and find their mean by their general categories:
- attacking
- skill
- movement
- power
- mentality
- defending
- goalkeeping
- overall skill

It was no surprise to see that in general, means for attacking attributes were higher for attacking players (forwards, wingers), defending attributes were higher for defenders, and goalkeeping attributes were higher for goalkeepers. Let's look at attribute groups which were less obvious.

<br>

![attributes](https://github.com/lukasbarbuscak/Soccer-Classification/blob/master/images/attributes.PNG)

Generally, goalkeepers and central defenders had lower mean than other positions in all attribute groups. On the opposite side of the spectrum, the results were mixed:
- skill: the players with the highest mean in the skill group were centre-forwards, attacking midfielders, and central midfielders
- movement: the players with the highest mean in the movement group were left/right midfielders and wingers, and centre-forwards
- power: the players with the highest mean in the power group were strikers, defensive midfielders, and centre-forwards
- mentality: the highest mean was registered by defensive midfielders and central midfielders

I then got curious about the average of all attributes: I called it an "overall":

<br>

![overalll](https://github.com/lukasbarbuscak/Soccer-Classification/blob/master/images/overalll.PNG)

Players who had high average through all abilities generally tended to be central and defensive midfielders, and left and right defenders. On the other hand, strikers, central defenders, and goalkeepers had lower average than other positions.

I then moved on to analyzing the categorical variables. I looked at the most worn number by each position (which generally follow the conventions), most frequent nationality by position, and found that most players are right-footed, have a normal body type, and have medium attacking and defensive workrate.

### EDA Summary

From the EDA, we can say that on average, comparing to other positions on the pitch:
- attacking players are shorter and lighter (with the exception of strikers), have higher wages and higher value, have better weak foot and better skill moves, possess better movement skills and tend to wear numbers 7, 9, 10, 11
- defensive players tend to be taller and heavier, have lower wages and lower market value, have better defending skills, possess worse weak foot and worse skill moves, and tend to wear numbers 2, 3, 4
- central players have stronger mentality, have higher level of all skills on average, wear numbers 6, 8, and tend to be from Germany and England

## Modeling

Unfortunately, this dataset was imbalanced, which meant that for some positions the sample was very small comparing to other positions. I split my dataset into train and test. I used the SMOTE technique to upsample my train dataset to better train my model. 





