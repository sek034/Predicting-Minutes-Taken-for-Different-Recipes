# Predicting-Minutes-Taken-for-Different-Recipes

*by Bryan Cha & Chloe Kim*

Our exploratory data analysis on this dataset can be found [here](https://sek034.github.io/Impact-of-Number-of-Nutritions/).

## Framing the Problem
---

## Baseline Model
---

---
## Final Model
---
To improve the final model, we incorporated two new features: **text length** feature and **TF-IDF** analysis feature. 

**Feature Engineering**: 
Our final model incorporated two new features. 

- First of all, we created a `steps_length` column by calculating the length of the text in the `steps` column, which can be a proxy for recipe complexity. We assumed that the longer texts describing the steps required for a reicipe, the more detailed or complex preparation processes it may indicate, potentially leading to longer cooking times. 

- Secondly, we used TF-IDF Vectorization on the `ingredients` column to transform the textual ingredient data into a structured format. This method will help in quantifying the uniqueness and importance of each ingredient in the context of the entire dataset. Ingredients that are unique or less common might be associated with more complex recipes, thus longer cooking times.

**Model Buildiing**: 

We used a more sophisticated model for this final model by incorporating **RandomForestRegressor** to capture more complex relationships in the data than a simple DecisionTreeRegressor. On top of the `'log_transform'` and `'normalize'` features from baseline model, we also normalized `'steps_length'` column by *MinMaxScalar()* and created tfidf features for `ingredients` column. 

**Hyperparameters Used**: 

Similar to our baseline model, we used `GridSearchCV` to determine which combination of hyperparameters to use, which included *max_depth*, *max_features*, and *n_estimators*. Based on our `GridSearchCV` result, we decided to use  

**Performance on the Model**: 

---
## Fairness Analysis
---
For our fairness analysis, we asked the question, **"does our final model perform better for recipes that take longer than an hour than it does for recipes that take 50 minutes or less?"** To test this, we ran a permutation test by shuffling the minutes of **Group X** where recipes take *longer than 50 minutes* to make and **Group Y** where recipes take *50 minutes or less*. 

We chose **RMSE** as our evaluation metric as it provides a somewhat clear measure of the average error in our predictions with a regression model. 

- **Null Hypothesis**: The model's performance, as measured by RMSE, is the *same* for both Group X and Group Y. Any observed differences in RMSE between the two groups are due to random chance.

- **Alternative Hypothesis**: The model's performance, as measured by RMSE, *differs* between Group X and Group Y. 

- **Test Statistic**: We used the *absolute difference in RMSE* between Group X and Group Y.
  
- **Significance Level**: 0.05

To perform the permutation test, we calculated the inital RMSE for each group using the actual data. We then shuffled the `minutes` values between two groups, and the RMSEs are recalculated for these permuted groups. 

**Permutation Testing Result**:
Our permutation test resulted in a p-value of **0.071**, which is *greater* than our significance level, 0.05. Therefore, we have a strong evidence to ***fail to reject*** the null hypothesis. This ***suggests*** that the model's performance is the same across the two groups: recipes that take longer than an hour and those that take 50 minutes or less. 

However, we can not be completely certain when drawing conclusions, therefore these findings merely indicate that our model is **reasonably fair based on the specific criteria and groups tested**; they do not represent a firm conclusion for entire unseen data. 
