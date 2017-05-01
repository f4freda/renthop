## Renthop - Rental Listing Inquiries

Renthop is an apartment rental website, and a portfolio company of Two Sigma, a VC firm investing in data science and artificial intelligence firms. This Kaggle competition ran for 3 months ending Apr 25 2017. 

Kagglers are asked to predict the probabilities of consumers' interest level ["low", "medium", "high"] generated by each apartment listing in the test set. The dataset has a variety of data types (numerical, geographical, text and 80GB of images) and offers extensive possibilities in feature engineering. Models are evaluated based on minimizing logloss (multi-class logarithmic loss). 

**Results**: I ranked Top 12% (#298) among 2,489 particpants. My model (a single XGBoost model) achieved a logloss of 0.51552 in the final leaderboard, as compared to 0.51421 in the public leaderboard. 

### My approach
I spent 80% of my attention on feature engineering (FE) and the other 20% on XGBoost parameter tuning. Please see the files in notebooks:
- **EDA.ipynb** for Exploratory Data Analysis, in numerical and location data
- **Text_preprocessing.ipynb** for exploring cleaning up text fields, vectorization and sentiment analysis using nltkvader
- **Model.ipynb** for the XGBoost model

Below are some FE issues I encountered and tried to resolve: 

### 1. Problem of High Cardinality Data
"manager_id" and "building_id" in the dataset are hashed IDs and have many distinct values. The high cardinality presents a dilemma in predictive modeling. On the plus side, these values can contain a lot of information. On the down side, converting them all into dummy variables increase the dimensionality of the data, and could lead to overfitting. 

For this competition, manager_id contained a vast amount of information while building_id was less informative. My model ended up using sklearn's preprocessing.LabelEncoder for both of these high-cardinality fields. In addition, manager_id was also turned into a score that combines manager_id's occurrence and interest_level into a score. 

### 2. Problem of Unbalanced Categories
Our models had to predict probabilities in 3 categories of "interest_level" -- high, medium or low. In the training set, only about 8% of the entries are belong to the "high" interest level category, whereas 22% "medium" and ~70% "low" interest level. This creates the danger that a predictive model would disproportionately learn about "low" interest level listing and fail to distinguish what makes a "high" interest level one. 

### 3. Extracting Value from Location Data

### In retrospect, what I could have done differently
**Modeling**
1. Train different models -- even if they are weaker single models, they can be helpful in avoiding overfitting when stacked
2. Learn and experiment with stacking methods, particularly StackNet
3. Find better ways to cross-validate and avoid overfitting the leaderboard 

**Feature Engineering**
4. Take current methods and go much further. Based on the sharing by those who placed top 10, when they vectorized the text fields, they used 16,000 most important words by td-idf. I used perhaps the most important 100 words by td-idf. 

