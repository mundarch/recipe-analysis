# recipe-analysis

## Introduction 

The dataset contains recipes and reviews submitted to food.com. 

### Research Question
**How can we find the healthiest recipes?**

People are increasingly prioritizing their health when choosing what to eat. Identifying healthy recipes can help individuals make informed dietary choices that align with their goals, whether they are looking to reduce their intake of unhealthy fats, balance macronutrients, or find high-protein options.

---

## Dataset 

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
  src="assets/protein_by_fat.html"
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

---

In order to set our Prediction Model performing a regression analysis to predict the number of calories in a recipe based on its protein and carbohydrates content. The response variable is calories, and the features used for prediction are protein and carbohydrates. 

We chose regression because the target variable, calories, is continuous and numeric. This approach will help identify how the macronutrient composition of a recipe influences its caloric content. 

We evaluate the model using Mean Squared Error, which is sensitive to large errors, making it suitable for continuous variables where extreme values need to be penalized, and R-squared, which provides an interpretable measure of model performance.

These metrics are preferred over others like F-1 or explained variance as they directly quantify the prediction error, which aligns with the goal of making accurate and practical calorie predictions.





