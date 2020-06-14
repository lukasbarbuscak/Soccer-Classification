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

<br>

![smote](https://github.com/lukasbarbuscak/Soccer-Classification/blob/master/images/smote.PNG)

As we can see, SMOTE increased the number of observations by creating synthetic ones based on nearest neighbours.

The metric I have chosen for measuring the accuracy of the classification model is the accuracy score. In other words, the score will tell me the percentage the model attributed the correct class to an observation (in this case, whether the model predicted the correct posittion of a player). As my baseline model, I used a dummy clasifier returning predictions by following the training set’s class distribution. The accuracy score was around 6.8%.

I chose three models that had a potential to improve this accuracy score:
- Decision Tree: this simple algorithm is ideal for multiclass prediction
- Random Forest: essentially creates multiple trees, and averages their results
- Gradient Boosting: weak learners improve performance gradually, and the key is the optimization of a loss function, allowing for the model to improve

I have summarized the results in the following table:
| Model  | MSE |
| ------------- | ------------- |
| Baseline  | 6.84%  |
| Decision Tree  | 60.04%  |
| Random Forest  | 72.10%  |
| Gradient Boosting  | 72.54%  |

After a bit of hyperparameter tuning, both gradient boosting and random forest performed approximately the same, with gradient boosting performing a bit better on the test data.

I saved the model, its predictions, and I analyzed which features were the most important: skill moves rating, left footedness, and abilities being some of the most important in the model.

<br>

![importances](https://github.com/lukasbarbuscak/Soccer-Classification/blob/master/images/importances.PNG)

## Discussion

To better understand the where the model performed well and where it made mistakes, I plotted a confusion matrix as a heatmap. I used percentages supplementary to counts, since absolute values were not necessarily the best measure because of the imbalanced test dataset.

<br>

![confusion](https://github.com/lukasbarbuscak/Soccer-Classification/blob/master/images/confusion.PNG)

While the predicted values of some positions matched their true values very well (goalkeeper, left/right/central defender, striker), other predicted values did not match the true values at all. I analyzed why that might have been the case:
- a lot of positions share the same area of the pitch, and therefore their attributes might be very similar: for instance, centre-forwards and strikers, or right/left defenders and right/left wing-backs
- in case of left/right midfielders/wingers, in addition to operating in the same areas of the pitch, their roles are mirrored, and what side of the pitch they play depends on the instructions, and their role: for instance, a player can be a winger or an inside forward, depending on which foot they prefer to use
- some positions on the pitch are not much about the positions, but also specific roles: for example, attacking midfielders can often free-roam around the attacking areas of the pitch, therefore they share common traits with wingers, or strikers

## Conclusion

To sum up, I have developed a model that is predicting soccer players' position based on their ability scores, financial information, and other features. After performing the exploratory data analysis, I fitted three models and compared their accuracy scores, comparing how well the models performed comparing to the baseline model, and also each other. The model performing the best was the gradient boosting model. I saved the model and saved the results of the prediction in a csv file. I also included the analysis of feature importances, and saved it in a separate file. Finally, I analyzed the accuracy of the model using a confusion matrix.

Future iterations of this type of analysis might consider analyzing general roles of players (for instance, based on the same instructions for multiple positions, such as right midfielder/winger, or centre-forward/striker). As mentioned in the discussion section, some positions are just too similar to each other, and the features used in the model alone are not able to distinguish between them accurately and consistently enough.
