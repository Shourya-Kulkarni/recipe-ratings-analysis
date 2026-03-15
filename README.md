# Recipe Ratings Analysis
### By Anna Rosenbaum and Shourya Kulkarni

This project investigates recipes and ratings from Food.com to answer: 
**Do recipes with more ingredients receive higher average ratings?**

# **Introduction:** 
The recipes and ratings dataset holds recipes and ratings from food.com. The data was originally scraped and used for other data analysis. There are two CSVs in the dataset, one that contains the recipes and the other contains the reviews and ratings submitted for the recipes. The overarching question we are hoping to answer is, What types of recipes tend to have higher average ratings? More specifically, we are looking to answer; Do recipes with more ingredients recieve higher average ratings than recipes with fewer ingredients. 

The importance of the dataset as well as the question being answered is based on the consumer process. It is important to know whether a recipe was rated highly if it contains expensive ingredients and could more ingredients signal a longer prep or cook time which means more time away from work, family, or friends. 

In the raw recipes dataset, there are 83,782 rows and 12 columns. The interactions dataset contains 721,927 rows and 5 columns. For the raw recipes, the main columns that are important to our analysis are the name, minutes, n_steps, and description. These columns indicate the recipe name, recipe ID, the number of minutes to prepare the recipe, the number of steps in the recipe, and the user-provided description. In the interactions dataset, the most important columns are the recipe ID, rating, and review columns. 

The two datasets were merged together during the data cleaning process and the final dataset contained 83,782 rows and 13 columns. The new column being the average rating. 

# **Data Cleaning and Exploratory Data Analysis** 
 
Before beginning to look at the dataset in an analytical way, the dataset needed to be cleaned. Specifically it was important to merge the two datasets; raw recipes and interactions together to prodcue one workable dataset. After merging, a column needed to be added to find the average rating for the recipes from the interactions dataset. This was done by first fill all ratings of 0 with np.nan. The main reason for this is because it is the first step in distinguishing between an actual rating of 0 and a missing rating value. This also allows us to prevent skew when averaging it as 0s do affect the average.

**Head of Dataset**

![Screenshot](screenshot.png)

**Univariate Analysis**
<iframe
  src="assets/univariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The first univariate plot looked specifically at the distribution of average ratings. Based on this plot, most of the recipes were rated highly with most of the recipes clustered between four and five. With few low ratings, there is a left-skewed distribution. 

The second univariate plot looks at the distribution of prep time with the majority of the times between 10 and 40 minutes. This suggests that most of the recipes don't take too long to prepare. The plot has a right-skew as there are fewer recipes with long preparation times. 

**Bivariate Analysis**

<iframe src="assets/bivariate_box.html" width="800" height="600" frameborder="0"></iframe>


<iframe src="assets/bivariate_scatter.html" width="800" height="600" frameborder="0"></iframe>

The boxplot above looks at the average rating distribution based on the number of ingredients. The median rating hovers around 5 which means that regardless of the number of ingredients, the recipes have high ratings. 

Based on the scatterplot, which looks at the correlation between number of ingredients vs. average rating. There is not a strong relationship between the two variables. There is a slight upward trend when there are more ingredients. 

**Pivot table**
<iframe
  src="assets/pivotheatmap.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The heatmap created from pivot table shows the average rating by ingredients and steps. The average ratings were high regardless of the amount of ingredients and/or number of steps. The recipes with 21+ ingredients show highest ratings, especially when the number and complexity of steps decreased. 

# **Assessment of Missingness**

**MNAR Analysis**

We believe the `description` column is MNAR. Recipes without descriptions are likely ones where the contributor did not feel the recipe needed explanation, meaning the missingness depends on the recipe itself (such as very simple recipes), not on any observed column. We assess the missingness of the `rating` column in the merged dataset, which contains 15,036 missing values out of 234,429 rows. We test whether this missingness depends on two other columns: `minutes` and `n_ingredients`.

**Missingness Dependency**

We assess the missingness of the `rating` column in the merged dataset, which contains 15,036 missing values out of 234,429 rows. We test whether this missingness depends on 
two other columns: `minutes` and `n_ingredients`.

Missingness of `rating` VS. `minutes` :

- **Null Hypothesis:** The missingness of `rating` does not depend on `minutes`.
- **Alternative Hypothesis:** The missingness of `rating` does depend on `minutes`.
- **Test Statistic:** Difference in mean `minutes` between recipes with and without missing ratings.
- **Significance Level:** 0.05
- **Observed Difference:** 51.45 minutes
- **P-value:** 0.124

<iframe src="assets/missingness_minutes.html" width="800" height="600" frameborder="0"></iframe>

Since the p-value (0.124) is greater than 0.05, we fail to reject the null hypothesis. The missingness of `rating` does **not** depend on `minutes`.


Missingness of `rating` VS. `n_ingredients`:

- **Null Hypothesis:** The missingness of `rating` does not depend on `n_ingredients`.
- **Alternative Hypothesis:** The missingness of `rating` does depend on `n_ingredients`.
- **Test Statistic:** Difference in mean `n_ingredients` between recipes with and without missing ratings.
- **Significance Level:** 0.05
- **Observed Difference:** 0.1607
- **P-value:** 0.0


<iframe src="assets/missingness_ingredients.html" width="800" height="600" frameborder="0"></iframe>

Since the p-value (0.0) is less than 0.05, we reject the null hypothesis. The missingness of `rating` does depend on `n_ingredients`. Recipes with missing ratings tend to have 
slightly fewer ingredients on average.


# **Hypothesis Testing**

**Question:** Do recipes with more ingredients receive higher average ratings than recipes with fewer ingredients?

**Null Hypothesis:** Recipes with more ingredients have the same or lower average rating than recipes with fewer ingredients.

**Alternative Hypothesis:** Recipes with more ingredients have higher average ratings than recipes with fewer ingredients.

**Justification:** We chose a permutation test because we do not have information about any population distribution. We use a one-tailed test since our alternative hypothesis is directional.

**Test Statistic:** Difference in mean avg_rating between recipes above and below the median number of ingredients (9).

**Significance Level:** 0.05

- Observed difference: -0.0040
- P-value: 0.8220


<iframe src="assets/hypothesis_test.html" width="800" height="600" frameborder="0"></iframe>


**Conclusion:** Since the p-value (0.8220) is much greater than 0.05, we fail to reject the null hypothesis. There is no significant evidence that recipes with more ingredients receive higher average ratings. We cannot conclude the null hypothesis is true, it is only that we do not have sufficient evidence to reject it.


# **Framing a Prediction Problem**

**Prediction Problem:** Predict the average rating (`avg_rating`) of a recipe.

**Type:** Regression

**Response Variable:** `avg_rating`. We chose this because our entire project investigates what factors influence recipe ratings, and predicting 
the rating directly answers our research question.

**Evaluation Metric:** RMSE (Root Mean Squared Error). We chose RMSE because it is in the same units as our response variable (rating points), 
making it easy to interpret. A lower RMSE means our predictions are closer to the actual ratings.

**Features known at time of prediction:** We only use features that exist before any ratings are submitted, such as `n_ingredients`, `minutes`, and `n_steps`. We do not use any columns calculated from ratings since those would not be available before a recipe is rated.

# **Baseline Model**

**Model:** Linear Regression in a sklearn Pipeline

**Features:**
- `n_ingredients` (quantitative) : Number of ingredients
- `minutes` (quantitative) : Cooking time in minutes

Both features are quantitative so no encoding was needed. We applied `StandardScaler` to standardize both columns to the same scale.

**Performance:**
- Train RMSE: 0.6420
- Test RMSE: 0.6359

The train and test RMSE are very close, meaning the model is not overfitting. However, an RMSE of around 0.64 on a 1-5 rating scale is not ideal. The predictions are off by about 0.64 stars on average. We believe this model can be improved by adding more meaningful features in the final model.


# **Final model** 

The improvements made to the baseline model can be summarized into three points: 

- An algorithm change from Linear Regression to `Random Forest Regressor`
- Preprocessing using `ColumnTransformer` to apply specific transformations to specific columns
- Optimization using `GridSearchCV` to find the best model complexity. 

Diving deeper, we improved our baseline model by introducing two feature transformations in order to represent the data generating process in a better way. Firstly the `QuantileTransformer` was used on the minutes variable. Looking at the raw data, we saw that the cooking times were skewed in an extreme way; some recipes taking 10,000 minutes while others taking 30 minutes. Instead of using a linear model which would be affected greatly by outliers, we used the `QuantileTransformer`. Using this, we were able to map the distribution into a normal bell-shaped curve. The reason this is beneficial is that it allows our model to distinguish between differences in short time recipes like 15 minutes and 45 minutes as well as long time recipes like 8 hours and 4 hours with the same weight. Instead of focusing specifically on the minutes, the model now focuses on the relative rank for the effort instead of the actual number of minutes.

Secondly, we used `Binarizer` on `n_steps`. Adding `n_steps` and applying the `Binarizer`, using a threshold of 10, allows the model to understand the number of steps in a recipe in a more "human" way. The `Binarizer` allowed for a complexity flag to be made. This then ensured that the model was able to catch the idea that people might be more inclined to rate a complex recipe harshly if it did not turn out correct.

Transitioning to a `RandomForestRegressor` allows our model to assume that the different variables and features interact instead of a fixed or stagnant amount of change which would happen with linear regression. Random forest uses decision trees in order to decipher the non-linear interactions. Choosing the hyperparameters used for the model was done by `GridSearchCV` with 3-fold cross-validation. The best hyperparameters were identified as `max_depth` of 10 and `n_estimators` of 100. This means that the model was able to catch enough complexity at a depth of 10 without an overfitting error.

Comparing the baseline model to this improved model, we see improvements with a lower test RMSE and a reduced residual bias. The RMSE was lower due to the better predictions from the `RandomForestRegressor` and the residual bias was reduced due to the transformation of minutes.


# **Fairness Analysis**

- **Group X(simple):**  recipes with five or fewer steps 
- **Group Y(Complex):** recipes with more than five steps 
- **Evaluation metric:** RMSE, this was chosen as it is the most appropriate metric for our model as it judges large errrors in precition ratings to ensure the model understands more difficult recipes.
- **Test Statistic:** the difference in RMSE, group y RMSE - group X RMSE
- **Significance level:** 0.05

- **Null Hypothesis:** Our model is fair. The RMSE for the simple recipes and the complex recipes are the same, any observed difference is due to random change and sampling noise. 
- **Alternative Hypothesis:** Our model is unfair. The RMSE for complex recipes is higher than the RMSE for simple recipes. This suggests that the model has issues when predicting ratings for complex recipes.

- **Results:** The calculated p-value from this analysis was 0.412 which is larger than the significance level of 0.05. This means that we fail to reject the null hypothesis.

In conclusion, there is no statisitically signifcant evidence that suggests that the model has issues performing on complex reciples versus simple recipes. This suggests that the feature engineering done to improve the model encouraged generalization across levels of recipe complexity. 









