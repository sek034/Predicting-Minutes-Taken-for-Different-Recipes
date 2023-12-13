# Predicting-Minutes-Taken-for-Different-Recipes

*by Bryan Cha & Chloe Kim*

Our exploratory data analysis on this dataset can be found [here](https://sek034.github.io/Impact-of-Number-of-Nutritions/).

## Framing the Problem
---

## Baseline Model
---

## Final Model
---
To improve the final model, we incorporated two new features: **text length** feature and **TF-IDF** analysis feature. 

**Feature Engineering**: Our final model incorporated two new features. 

- First of all, we created a `steps_length` column by calculating the length of the text in the `steps` column, which can be a proxy for recipe complexity. We assumed that the longer texts describing the steps required for a reicipe, the more detailed or complex preparation processes it may indicate, potentially leading to longer cooking times. 

- Secondly, we used TF-IDF Vectorization on the `ingredients` column to transform the textual ingredient data into a structured format. This method will help in quantifying the uniqueness and importance of each ingredient in the context of the entire dataset. Ingredients that are unique or less common might be associated with more complex recipes, thus longer cooking times.

## Fairness Analysis
---
