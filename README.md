Saurav Kharb
Senior, University of Washington
Informatics - Data Science

Time spent on project : 7 hours

Introduction and background

The All-Star NBA team is an annual exhibition game played between the top players of the western and the eastern conference. The starters are chosen by counting votes from fans, media and current players. Essentially, the season’s top performing players get to play in the All-Star NBA teams. We can look at a variety of factors to determine our question of interest - Predict the number of All-NBA selections for any given player at any point in their NBA career. 
My aim was to find various data points that could tell us about a player’s performance in a season. I then combined these performance and demographic stats with the actual number of total All-star selections to treat as labels for our model. Each row would represent a single player.  Since we are predicting the number of All-star selections, a regression model would be an appropriate way to proceed in this case. This project was computed primarily in Python (Jupyter Notebook). 

Data Collection and preparation
My model was trained on a collection of three datasets pulled from different sources
Player Data : Contains demographic and player related information for all NBA players. (Collected from Kaggle dataset - NBA players stats since 1950)
Season Stats : Contains stats for each player for each season they have played. Stats include basic and advanced metrics such as number of 2-pointers, PER etc. (Collected from Kaggle dataset - NBA players stats since 1950)
Labels : The actual number of All-Star selections for each player.
To get this data I wrote a simple scraper in python to scrape basketball-reference.com)
See scrape_labels.ipynb for more details
These three datasets were combined by common columns such as Player Name. The final dataset consisted of 401 players with 1 or more All-Star NBA predictions and around 50 players with zero All-star selections. The players with zero All-star selections were picked randomly from the set of players that were present in the Season Stats table but did not have any selections in an All-Star team. There were a lot of columns with missing values. These were especially prevalent for players who played in the 90’s. This may be because of lack of record keeping and information collection. I dropped columns that had too many missing values, and removed rows for those columns that had only a few missing values. Substituting the missing values with aggregate information did not seem like an appropriate choice. In addition, I added dummy variables for columns Team and Position. This was because I thought it was very important to keep the position of a player in mind while predicting. Also, the team makes a great difference in how the player’s performance is perceived by fans and media. But since these two were categorical variables, I created dummy variables representing 0 and 1 for each team and position. 

Modelling and Analysis

Feature selection
In order to estimate which features would be optimal to predict our labels, I decided to try out two different approaches. In the first approach I calculated Pearson’s coefficient to determine the correlation between our labels and features. The table below shows 12 features that correlate the most with our labels.




In my second approach I used Sklearn’s SelectKBest with f_regression to determine the most appropriate features. The top ten features extracted from this approach were : 
'2P',
 '2PA',
 'DWS',
 'FG',
 'FGA',
 'FT',
 'FTA',
 'MP',
 'OWS',
 'PER',
 'PTS',
 'TRB',
 'WS',
 'WS/48'

Both of these do make sense since 2P, total points, PER are some good estimates of a player’s performance. 

Next step was to split the data into training and testing groups. I made a 80-20% train-test split. I then built a pipeline to perform some feature transformation before training. I used a MinMax() scaler to transform each feature to a single scale. The scoring was to be done in terms of r^2 value returned. I tested three models - Ridge Regression, Logistic Regression and Random Forest Regression to predict our labels. Random Forest (accuracy=96%) regression outperformed RIdge (91%) which outperformed Logistic Regression (~50%). Overall, I think the models performed pretty well showing that Stats alone can tell us a lot about the NBA all star predictions.

Ridge Regression Scatterplot



Random Forest Scatterplot



Reflection :
The model predicted 6 NBA-All star selections for both Stephen Curry and Kyrie Irving

I still think we have a chance of improving the model had we had more insight into media related data. While the features we had included showed good signs, they were clearly not wholesome in predicting the labels. I also tried an alternate approach, where instead of one player as a single row (which only leaves us with around 500 rows to train with) I instead used all rows for each players (aka all seasons for each players). This expanded the dataset to around 4500 rows but did not result in an increase in prediction accuracy. Given the time constraint and scope of this project, 



