# Predicting Calories

## Introduction 

The dataset contains recipes and reviews submitted to food.com. 

### Research Question
**How can we find the healthiest recipes?**

People are increasingly prioritizing their health when choosing what to eat. Identifying healthy recipes can help individuals make informed dietary choices that align with their goals, whether they are looking to reduce their intake of unhealthy fats, balance macronutrients, or find high-protein options.

---

## Relevant Dataset 

| **Column Name**       | **Description**                                                                                      |
|------------------------|-----------------------------------------------------------------------------------------------------|
| `name`                | Recipe name |
| `calories`            | Total caloric content |
| `total_fat`           | Total fat content |
| `carbohydrates`       | Total carbohydrate content (grams) |
| `protein`             | Total protein content  |
| `saturated_fat`       | Total saturated fat (in grams) |
| `sugar`               | Total sugar content (grams) |
| `sodium`              | Total sodium content (milligrams)  |

---

## Why Do We Care?  

Nutrition will always be a key part in our lives and finding faster, more convenient ways can massively improve lifestyles.

# Data Cleaning Steps 

**Left Merge of Datasets**:  
   The `recipes` dataset was merged with the `interactions` dataset using a left join. The left join ensured that every recipe in the `recipes` dataset was retained, even if it had no corresponding interactions (ratings) in the `interactions` dataset. This helped to integrate user ratings into the recipes dataset.
   
**Handling Ratings of 0**:  
   In the merged dataset, all `rating` values of `0` were replaced with `np.nan`. This step is required because a rating of `0` likely represents missing or invalid feedback rather than an actual rating. Treating these as missing values (NaN) ensures they don't skew the analysis, particularly when calculating averages.

**Average Ratings per Recipe**:  
   The average rating was calculated by grouping the data by `recipe_id` and taking the mean of the `rating` values. This provides a single summary statistic for each recipe's overall user feedback.

**Adding Average Ratings Back**:  
   The calculated average ratings were added back to the `recipes` dataset. This step enriches the recipes data by providing an aggregate measure of user preference, which can be used for further analysis or model training.


<iframe
  src="assets/recipes_head.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

---

<iframe
  src="assets/top_calories_recipes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The recipes with the most calories are predominantly desserts and high-fat indulgent meals suggesting calorie-dense foods often belong to these categories.

---

<iframe
  src="assets/protein_by_saturated_fat_category.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Recipes with higher saturated fat tend to exhibit a wider range of protein content, indicating that while high-fat recipes can be protein-rich, they also include options with lower protein levels.


---

<iframe
  src="assets/top_10_healthiest_recipes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Recipes with higher scores have relatively more protein per unit of carbohydrates and fat, indicating that they might be healthier in terms of promoting protein intake while keeping fats and carbohydrates in check.

---

In order to set our Prediction Model performing a regression analysis to predict the number of calories in a recipe based on its protein and carbohydrates content. The response variable is calories, and the features used for prediction are protein and carbohydrates. 

We chose regression because the target variable, calories, is continuous and numeric. This approach will help identify how the macronutrient composition of a recipe influences its caloric content. 

We evaluate the model using Mean Squared Error, which is sensitive to large errors, making it suitable for continuous variables where extreme values need to be penalized, and R-squared, which provides an interpretable measure of model performance.

These metrics are preferred over others like F-1 or explained variance as they directly quantify the prediction error, which aligns with the goal of making accurate and practical calorie predictions.


## Baseline Model 

The model is a Linear Regression pipeline designed to predict the number of calories in a recipe based on its macronutrient composition. It uses StandardScaler to standardize quantitative features before passing them to the regression model.

In this pipeline, the features are first standardized using StandardScaler, which normalizes the data by scaling the features to have a mean of 0 and a standard deviation of 1. This is important because it ensures that no single feature dominates the others in terms of scale, and it allows the model to process the features equally. The pipeline then trains the Linear Regression model on the scaled features to predict the calorie content.

Features:

Protein (Quantitative): The protein content in percentage daily value.

Carbohydrates (Quantitative): The carbohydrate content in percentage daily value.

Model Performance:

Mean Squared Error (MSE): 56,218.06

R-squared (R²): 0.84

The R-squared indicates that 84% of the variance which suggests a decent linear relationship between the features (protein and carbohydrates) and the response variable (calories). The MSE (56,218.06) reflects the average squared error in predictions, which isn't very good given the likely range of calorie values.

## New Model 

In the New Model we introduce 2 new engineered features: `protein_to_carbs_ratio` and `fat_to_calories_ratio`. 

Both features are derived from existing data points (protein, carbohydrates, and fat), and they provide more granular information that might improve our ability to predict calorie content. They make sense from a data-generating perspective because they represent fundamental nutritional aspects of recipes, which directly influence calorie counts. 

Modeling Algorithm and Hyperparameter Selection:

Model Choice: Ridge Regression:

Ridge regression is chosen as the model because it provides a regularized approach to linear regression. Ridge regression helps mitigate overfitting by penalizing large coefficients, which is especially useful when there are multiple features, some of which may be highly correlated. This regularization improves model generalization and ensures more stable predictions.

Hyperparameter Grid and Best Parameters:

The key hyperparameter I tuned was the regularization strength (`alpha`). A higher alpha value increases the regularization effect, while a lower value allows the model to fit the data more closely. The `GridSearchCV` approach was used to search over a range of alpha values: [0.1, 1.0, 10.0, 100.0].
Best Hyperparameter: The best hyperparameter found during the grid search was alpha = 1.0. This value provided a good balance between fitting the data well and avoiding overfitting.

Model Performance:

Mean Squared Error (MSE): 41986.41

R-squared (R²): 0.88

The final model has improved upon the baseline model:

With the introduction of the engineered features, the final model's R² increased to 0.88, reflecting a significant improvement in the ability to explain the variance in the target variable, calories.

The reduction in Mean Squared Error (MSE) also suggests better prediction accuracy.
