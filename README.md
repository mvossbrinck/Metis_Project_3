## Predicting Wildfire Size Class

### Description
#### The goal of this project is to be able to correctly predict when a fire will be small (<= 10 acres) vs large (> 10 acres) so that more resources can be diverted to large fires to help them be contained faster. Since the majority of wildfires are contained quickly, the intention of the model is that it is used if a fire is still not contained after 5 hours. 
&nbsp;

### Possible Impact
#### If the model is able to correctly predict large fires to better inform response teams, it can help reduce the amount of deaths, injuries, and damage that occurs due to wildfires. 
&nbsp;

### Features and Target Variables
#### Features include region, latitude, longitude, fire cause, discovery date, discovery day of year, and discovery time.

#### The target variable is fire size class - small (<= 10 acres) vs large ( > 10 acres).
&nbsp;

### Data Used
#### The data was obtained from Kaggle - https://www.kaggle.com/rtatman/188-million-us-wildfires
&nbsp;

### Tools Used
#### Connecting to Data: sqlite3
#### EDA and Visuals: pandas, numpy, seaborn, matplotlib, datetime, Tableau
#### Regression and Analysis: statsmodels, scikit-learn, scipy, XGBoost
&nbsp;

### File List
#### 1_w3school.md - Solution for first project starter set of SQL problems
#### 2_Baseball.ipynb - Solution for second project starter set of SQL problems
#### 3_Soccer.ipynb - Solution for third project starter set of SQL problems
#### 4_tennis.md - Solution for fourth project starter set of SQL problems
#### Data Set Cleaning and Filtering.ipynb - Pulling in data, filtering it, creating new variables, and exporting to CSV
#### EDA.ipynb - Exploring missing containment data and exploring patterns and trends in final data set
#### Models, Results, and Predictions.ipynb - Main code with models, performance metrics, and predictions based off selected XGBoost model 
&nbsp;

### Conclusions
#### The final model was about to correctly predict about 62% of actual large fires, so it does perform better than random chance. However, the analysis did remove some data with missing fields that likely wasn't missing at random and 62% still misses a considerable number of large fires. Therefore, this should be used in conjunction with other tools and information rather than on it's own. Also, there is potential for further improvements that should be explored that could potentially improve performance. 
