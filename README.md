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

2. **Handling Ratings of 0**:  
   In the merged dataset, all `rating` values of `0` were replaced with `np.nan`. This step is required because a rating of `0` likely represents missing or invalid feedback rather than an actual rating. Treating these as missing values (NaN) ensures they don't skew the analysis, particularly when calculating averages.

3. **Average Ratings per Recipe**:  
   The average rating was calculated by grouping the data by `recipe_id` and taking the mean of the `rating` values. This provides a single summary statistic for each recipe's overall user feedback.

4. **Adding Average Ratings Back**:  
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
