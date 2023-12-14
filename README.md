# Predicting-Minutes-Taken-for-Different-Recipes

*by Bryan Cha & Chloe Kim*

Our exploratory data analysis on this dataset can be found [here](https://sek034.github.io/Impact-of-Number-of-Nutritions/).

## Framing the Problem
#### <u>Prediction Problem: Predicting Minutes Taken for Different Recipes<u>

**How we cleaned the data**: 


Since we are predicting minutes based on the different recipes, there was no need to merge the <u>recipes.csv</u> data with <u>interactions.csv</u> data, since
that would just give us duplicate rows of the exact same recipe. <br>

Data Cleaning Steps: </br>
- Sliced out only the data that is intuitive and relevant to our predictive analysis <br>
- While each element in the **minutes** column in <u>recipes.csv</u> is an integer, time in general
is a continuous variable; therefore, we converted it to type ```np.float64``` to better aid my prediction 
problem.
- Our analysis in the last project revealed outliers in the **calories** column, which might bias certain regression
models when conducting prediction, so we used the interquartile range to filter out any data that is below the <u>first quartile</u> or
above the <u>third quartile</u>
- The table below shows the resulting columns that I am using for my predictive analysis: <br>


|Column Name     |Description                                     |dtype         |
|----------------|------------------------------------------------|--------------|
|name            |Name of each Recipe                             |```object```  |
|n_steps         |# of steps involved in making recipe            |```int64```   |
|steps           |Description of steps involved in making the food|```object```  |
|n_ingredients   |# of ingredients involved in making the food    |```int64```   |
|ingredients     |Ingredients involved in making the food         |```object```  | 
|calories        |# of calories involved in the recipe            |```float64``` | 
|minutes         |Time taken to carry out the recipe              |```float64``` | 

- Since each element in the **ingredients** and **steps** columns are a list in string format,
I handled it by joining all the comma-separated strings into a single string so that it is possible to
use a tdidf-vectorizer to create new features later. I also removed any extra spaces to ensure proper parsing
and consistency. <br>

This results in the following DataFrame: <br>
|    | name                                 |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |   n_ingredients | ingredients                                                                                                                                                                                     |   minutes |   calories |
|---:|:-------------------------------------|----------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------:|-----------:|
|  0 | 1 brownies in the world    best ever |        10 | heat the oven to 350f and arrange the rack in the middle line an 8-by-8-inch glass baking dish with aluminum foil combine chocolate and butter in a medium saucepan and cook over medium-low heat stirring frequently until evenly melted remove from heat and let cool to room temperature combine eggs sugar cocoa powder vanilla extract espresso and salt in a large bowl and briefly stir until just evenly incorporated add cooled chocolate and mix until uniform in color add flour and stir until just incorporated transfer batter to the prepared baking dish bake until a tester inserted in the center of the brownies comes out clean about 25 to 30 minutes remove from the oven and cool completely before cutting                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |               9 | bittersweet chocolate unsalted butter eggs granulated sugar unsweetened cocoa powder vanilla extract brewed espresso kosher salt all-purpose flour                                              |        40 |      138.4 |
|  1 | 1 in canada chocolate chip cookies   |        12 | pre-heat oven the 350 degrees f in a mixing bowl sift together the flours and baking powder set aside in another mixing bowl blend together the sugars margarine and salt until light and fluffy add the eggs water and vanilla to the margarine sugar mixture and mix together until well combined add in the flour mixture to the wet ingredients and blend until combined scrape down the sides of the bowl and add the chocolate chips mix until combined scrape down the sides to the bowl again using an ice cream scoop scoop evenly rounded balls of dough and place of cookie sheet about 1 2 inches apart to allow for spreading during baking bake for 10 15 minutes or until golden brown on the outside and soft chewy in the center serve hot and enjoy                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |              11 | white sugar brown sugar salt margarine eggs vanilla water all-purpose flour whole wheat flour baking soda chocolate chips                                                                       |        45 |      595.1 |
|  2 | 412 broccoli casserole               |         6 | preheat oven to 350 degrees spray a 2 quart baking dish with cooking spray set aside in a large bowl mix together broccoli soup one cup of cheese garlic powder pepper salt milk 1 cup of french onions and soy sauce pour into baking dish sprinkle remaining cheese over top bake for 25 minutes or until cheese is lightly browned sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly about 10 more minutes                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |               9 | frozen broccoli cuts cream of chicken soup sharp cheddar cheese garlic powder ground black pepper salt milk soy sauce french-fried onions                                                       |        40 |      194.8 |
|  3 | millionaire pound cake               |         7 | freheat the oven to 300 degrees grease a 10-inch tube pan with butter dust the bottom and sides with flour and set aside in a large mixing bowl cream the butter and sugar with an electric mixer and add the eggs one at a time beating after each addition alternately add the flour and milk stirring till the batter is smooth add the two extracts and stir till well blended scrape the batter into the prepared pan and bake till a cake tester or knife blade inserted in the center comes out clean about 1 1 2 hours cool the cake in the pan on a rack for 5 minutes then turn it out on the rack to cool completely                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |               7 | butter sugar eggs all-purpose flour whole milk pure vanilla extract almond extract                                                                                                              |       120 |      878.3 |
|  4 | 2000 meatloaf                        |        17 | pan fry bacon and set aside on a paper towel to absorb excess grease mince yellow onion red bell pepper and add to your mixing bowl chop garlic and set aside put 1tbsp olive oil into a saut pan along with chopped garlic teaspoons white pepper and a pinch of kosher salt bring to a medium heat to sweat your garlic preheat oven to 350f coarsely chop your baby spinach add to your heated pan stir frequently for approximately 5 min to wilt add your spinach to the mixing bowl chop your now cooled bacon and add it to the mixing bowl add your meatloaf mix to the bowl with one egg and mix till thoroughly combined add your goat cheese one egg 1 8 tsp white pepper and 1 8 tsp of kosher salt and mix till thoroughly combined transfer to a 9x5 meatloaf pan and cook for 60 min or until the internal temperature is at least 160f let stand for 5min melt 1tbsp unsalted butter into a frying pan and cook up to three eggs at a time crack each egg into a separate dish in order to prevent egg shells from reaching the pan then add salt and pepper to taste wait until the egg whites are firm looking but slightly runny on top before flipping your eggs after flipping wait 10~20 seconds before removing each egg and placing it over your slices of meatloaf |              13 | meatloaf mixture unsmoked bacon goat cheese unsalted butter eggs baby spinach yellow onion red bell pepper simply potatoes shredded hash browns fresh garlic kosher salt white pepper olive oil |        90 |      267   | 
</br>


__Dependent Variable__: I chose **minutes** as the dependent variable because it is a <u>quantitative continuous</u> variable, which makes it a good target for regression. While the data seemingly only provides integers values for minutes, time is ultimately a continuous variable, which makes it more feasible to predict if converted to a floating value. In a real-world context, restaurants can use this predictive model to plan out feasible menus and individuals will be able to estimate and manage their time in the kitchen, making this predictive analysis useful in time management situations, or even for food and beverage businesses where menu planning might be required.


__Evaluation Metric__: For a regression problem like mine, metrics like mean absolute error (MAE), mean squared error (MSE), and root mean squared error (RMSE) come to mind. I will be using the **Mean Absoute Error (MAE)**, since this metric is easy to understand and robust to outliers. We will also be including the **Root Mean Squared Error (RMSE)**. However, since RMSE is unbounded, we also included a normalized RMSE, which reflects the error by scaling it to the range of the dependent variable that we have.


__Known Information at Time of Prediction__: For predicting calories, the **n_steps** and **n_ingredients** will already be known at the time of prediction, since the ingredients themselves are already available and the number of steps have to occur before the time is calculated, which means that it is feasible to use these features for minute prediction.


---

## Baseline Model

---
__Description__: As mentioned, I will be using **n_ingredients** and **n_steps** from the original dataset as initial features for my baseline model. While there are no categorical features so feature encoding will not be necessary here, there is no upper bound for the number of ingredients and number of steps since they are both <i>ordinal discrete</i> variables so some transformation might be required.



__Feature Transformations__: First step of transformation was transforming the columns in a dataset with using *ColumnTransformer* with *FunctionTransformer(np.log)*and *MinMaxScaler()*.
We transform data in *n_steps* and *n_ingredients* into log of its data in order to make them to resemble to normal distribution. Then, we normalized the values in two columns with using *MinMaxScaler()* so that they can have the same scale for better prediction. 



__Hyperparameters Used__: 
We used *GridSearchCV* to predict the ideal hyperparamters for criterion, max_depth of tree, and min_samples_plits.
Initially we used hyperparatmers with broad range *(ex.clf__max_depth' : [1, 30, 50, 100, 200])*, and we minimized the range by running the GridSearch for several times. After Several times, we got to conclusion with ```{'clf__max_depth': 2, 'clf__min_samples_split': 15, 'clf__n_estimators': 500}```


After the *GridSearchCV*, we use the ideal parameters to train and test the data with *DecisionTreeRegressor* and fit the data after.


__Model Performance__:

|Metric                                     |Train Performance      |Test Performance   |
|-------------------------------------------|-----------------------|-------------------|
|Mean Absolute Error (MAE)                  |134.4399831981876      |135.48074341738445 |
|Root Mean Squared Error (RMSE)             |4416.8314061631945     |2828.3684084594393 |
|Normalized Root Mean Squared Error (n-RMSE)|0.0042017041535038     |0.00982075774033743|

As we have have lower train mean absolute error than the test mean absolute error, we can tell that the model fits well, but the results are suboptimal because there might not be enough features to explain the data, or the features that we provided are simply insufficient in fully predicting the minutes feature. However, it seems that our RMSE metric has the test set outperforming the training set. This might be due to RMSE penalizing larger errors more heavily. We also tried to fit the data with grid search and cross validation, and the gap distance between train data and test data did not get closed up. However, the gap between the train and test data are not too far off, and we can predict that there is no error for traing and testing regression. The possible reason for the slight gap between two might caused by our two features that we used for training and testing the regression are insufficient to explain the data and predict the minute.


### Feature Transformation
These are visualization of our different numeric features before and after conducting transformations on them to offer better insight on why we need to conduct these transformations. The distribution of all three features are mostly right-skewed Firstly, we have our ordinal features **n_steps** and **n_ingredients**, which we conduct a <u>log transformation</u> (```np.log```) on, since our EDA revealed that the minimum amount of steps and ingredients are non-zero, which will not raise any error when taking the log values of these features. Next, we have our ordinal feature calories, which we used a <u>square root transformation</u> (```np.sqrt```) on instead. This is because calories do contain zero values; thus, a sqaure root transformation would better fit our purpose of trying to make the distribution more normal for the calories. We can see that all three features better reflect a symmetric and "normal" distribution after undergoing their respective transformations.


This visualization shows how doing a filtering of the dataset based on the <u>Interquartile Range</u> helps in stabilizing the distribution of the **calories** feature. Looking at the boxplot before filtering, we can see that the plot is basically dotted with several outliers of > 10000 calories. This severely skews the distribution of the calories and might also affect our prediction despite any transformations. The boxplot to the right shows the distribution of calories after conducting the filtering, and we can see that the lower end of the data (~200 to 500 calories) is now better represented and will not be undermined by large outliers. 





---
## Final Model

For final model, we added two more different variables, which are *calories and tfidf of steps* in order to get a better prediction with training and testing the data.

***Description***
the final model that we came up with includes 2 additional numeric features, which are *calories and tfidf of steps*. As tfidfi of steps has max features of 100, so these two different features should be more than enough to accruately train and test the data for prediction. We predicted that there are correlation between *minutes* with two additional variables. If the recipe is a high calories food, it tends to take more time as it contains more ingredients to be high calories food. Furthermore, high calories ingredients tend to take more time to cook in general. Also, important words in steps of recipe could affect the minutes to cook. For example, word like *oven* indicates that the cooking time will be longer than others. On the otherhands, word like "microwave* indicates that the cooking time tends to be shorter than others. For the features of final models, they are *n_step, n_ingredients, calories, and tfidf of steps*. 

***New Features***

**Calories**
We used *FunctionTransformer* to *squareroot* the calories to make it has more normal distribution. We used squareroot instead of log transformation as calories include zero values. We also used *MinMaxScaler* to normalize values between 0 and 1. 

**TFIDF of steps**
In order to reduce the complexity, we limited the number of word features that we wanted to consider to 100.
We also used tfidf to find assign the importance of each words that was used in steps and see how it affects the minutes. In order to compute tfidf of each word, we also used vectorizers to split each word and also get tfidf. 

***Algorithm Chosen***
We used *random forest regressor* as it's not sensitive to outliers and robust to overfitting. Also, it can model non-linear relationship. Furthermore, we don't have to tune many hyperparameters. Since random forest is for train each decision tree in parallel, it is more efficient to our dataset. 

We also used *GridSearchCV* in order to find the optimal numbers for our hyperparameter, which includes *clf__n_estimators*, *model__max_depth'* and *model__min_samples_split* to find the best combination that genearlize the best unseen data. I started running numbers in each parameters with big gaps, and I closed down the gaps as I repeatedly run the GridSearchCV. Therefore, I got 
*n_estimators= 100, max_depth=6, min_samples_split=30*. I had to comment the GridSearchCV and put down the results to *RandomForestRegressor* in the pipeline directly as running GridSearchCV and fitting takes too much of a time.

***Performance***
We can clearly see the improvement compared to the baseline performace. We got around 90.8 for MAE of train performace for final model, and we got 134.4 MAE for train performace for base model. Furthermore, Test Performance's MAE for final model decreased about 20 compared to base model's. Also, Normalized Root Mean Squared Error decrease for Final model for both Train Performance and Test Performance. Therefore, we can state that final model generalize better on unseen data for prediction minutes of each recipe.


__Model Performance__:
|Metric                                     |Train Performance      |Test Performance   |
|-------------------------------------------|-----------------------|-------------------|
|Mean Absolute Error (MAE)                  |90.87995849444408      |115.9155515445313  |
|Root Mean Squared Error (RMSE)             |2101.283982867101      |2900.2191665008613 |
|Normalized Root Mean Squared Error (n-RMSE)|0.0019989383398659635  |0.01007024040535162|

As you can see from the table above, our metrics are improved as compared to the baseline model. The MAE for both our train and test performance are imporved from our baseline. Since MAE is a metric whereby errors are treated equally, it is fair to say that the addition of text and numeric features to our baseline further improved the performance of the model in predicting the minutes of the recipe. In the caseof our RMSE metric, we can see that while there is little change, the model seems to have fitted better as opposed to our baseline model, with the difference between training and testing performance being closer to each other. Again, since the RMSE is a metric that is sensitive to outliers, the train and test RMSE being close to each other seems to be a indicator of a better fit.



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

The histogram below offers a visualization of the RMSE differences obtained during the permutation iterations. The red vertical line indicates the *observed* RMSE difference. 

<iframe src="graphs/fig_perm.html" width=800 height=600 frameBorder=0></iframe>


However, we can not be completely certain when drawing conclusions, therefore these findings merely indicate that our model is **reasonably fair based on the specific criteria and groups tested**; they do not represent a firm conclusion for entire unseen data. 
