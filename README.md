# TastyData

by Yuancheng Cao (yuc094@ucsd.edu), Grace Chen

Credit: [UC San Diego DSC 80 Winter 2023 Course Project Instruction](https://dsc80.com/project3/recipes-and-ratings/)

## Introduction


## Cleaning and EDA

### Data Cleaning
1. **Load the Datasets**
  - Use `pd.read_csv()` to read 'recipes' dataset with os.path.join() to get recipes dataset.
  - Use `pd.read_csv()` to read 'interactions' dataset with os.path.join() to get interactions dataset.
2. **Merge DataFrames**
  - Left merge the recipes and interactions datasets together.
  - Fill all ratings of 0 with `np.nan` in 'rating' column.
    - **Following Question: Why we need to fill all ratings of 0 with `np.nan`.**
    - The reason why we need to fill all ratings of 0 with `np.nan` is to handle missing or incomplete data. In some cases, a rating of 0 may indicate that the recipe is not rated at all and does not necessarily reflect the quality of the recipe. By replacing these 0 values with `np.nan`, we can avoid the bias that might be introduced by using 0 ass a proxy for missing values. In addition, when we are computing statistics, `np.nan` is automatically ignored.
  - Find the average rating per recipe, as a Series
    - Group by the 'id' (Recipe ID) and get 'rating' column with [],
    - Use `mean()` method to calculate mean of each recipe,
    - Use `reset_index()` and set column names to make it as a DataFrame
    - Left merge average_rating DataFrame and recipes dataset
  - In order to facilitate the extraction of data for later analysis, split the string type of list of nutrition information in `nutrition` column into individual columns
    - Rename the columns in 'nutritions' DataFrame to match the nutrition information
    - Convert all data in 'nutritions' DataFrame to float type

3. **Find the Null values**
4. **Know the datatypes of each column**: This Series is the DataFrame before adding one-hot encode data.

|                | type    |
|:---------------|:--------|
| name           | object  |
| id             | int64   |
| minutes        | int64   |
| contributor_id | int64   |
| submitted      | object  |
| tags           | object  |
| nutrition      | object  |
| n_steps        | int64   |
| steps          | object  |
| description    | object  |
| ingredients    | object  |
| n_ingredients  | int64   |
| user_id        | float64 |
| recipe_id      | float64 |
| date           | object  |
| rating         | float64 |
| review         | object  |
| avg_rating     | float64 |
| calories       | float64 |
| total_fat      | float64 |
| sugar          | float64 |
| sodium         | float64 |
| protein        | float64 |
| saturated_fat  | float64 |
| carbohydrates  | float64 |

### Univariate Analysis


### Bivariate Analysis
<iframe src="assets/avg_protein_by_season.html" width=600 height=400 frameBorder=0></iframe>

The histogram shows the mean protein content for each season. The graph shows that the fall and winter recipes have a higher mean protein content compared to the spring and summer recipes. Because there are many holidays in the fall and winter, such as Labor Day, Veteran's Day, Thanksgiving, Christmas, New Year, etc. People often have meals with family and friends. Meat and other high-protein foods are cooked in different wayss for their meals. This may result in higher overall protein content in these recipes. On the other hand, spring and summer seasons tend to have more light and fresh meals, with a focus on vegetables and fruits, whcih generally have lower protein content.

<iframe src="assets/corr_matrix_mul_cols.html" width=600 height=400 frameBorder=0></iframe>

This correlation matrix created by `heatmap()` from `seaborn`. The correlation matrix shows the correlation between pairs of varaibles in a data set. 

### Interesting Aggregates
1. This groupby DataFrame shows the count and mean of the `protein` column for each unique recipe ID in the `full_recipes` DataFrame. The resulting DataFrame is sorted in asscending order based on the mean protein value. This DataFrame can help identify which recipes have the highest and lowest protein content, and frequency of each recipe in the `full_recipes` DataFrame. Below DataFrame shows first and last 5 rows of DataFrame.

    |     id |   count |   mean |
    |-------:|--------:|-------:|
    | 461235 |       1 |      0 |
    | 447587 |       3 |      0 |
    | 308738 |       2 |      0 |
    | 308774 |       1 |      0 |
    | 335660 |       1 |      0 |
    |   ...  |   ...   |   ...  |
    | 446886 |       2 |   2281 |
    | 449670 |       1 |   2637 |
    | 365630 |       1 |   2929 |
    | 338356 |       5 |   3605 |
    | 392286 |       1 |   4356 |

2. The pivot table is grouping the data in the `full_recipes` DataFrame by the recipe id (`id` column) and calculating the sum of values in the `low-protein` and `high-protein` columns for each unique recipe. This can be useful for the understanding how many recipes have each tags (either `low-protein` or `high-protein`). The resulting DataFrame can be used to compare the prevalence of the two tags and identify any patterns or trendss in the data. For example, after pivoting the DataFrame, I found that the recipe id is 519068 has 3 `high-protein` tags and 0 `low-protein` tag. Below DataFrame shows first 5 rows of DataFrame.

    |     id |   high-protein |   low-protein |
    |-------:|---------------:|--------------:|
    | 537459 |              0 |             0 |
    | 537485 |              0 |             0 |
    | 537543 |              0 |             0 |
    | 537671 |              0 |             0 |
    | 537716 |              0 |             0 |

3. I think the above two DataFrames are too big and need to be shrunk further. So, the following groupby table shows the count and mean of protein for recipes with `low-protein` and `high-protein` tags. The categories are defined based on the presence or absence of the respective tags in the recipes. The DataFrame helps to compare the protein content of recipes with and without these tags, and to identify any significant differences in protein content based on these tags. It can also help in creating recipes with a specific protein content range and provide insights into the popularity and nutrient information of recipes with and without these tags.

    |    |   low-protein |   high-protein |   count |     mean |
    |---:|--------------:|---------------:|--------:|---------:|
    |  0 |             0 |              0 |  192286 | 35.0086  |
    |  1 |             0 |              1 |    8229 | 93.2573  |
    |  2 |             1 |              0 |   33914 |  8.19741 |

## Assesssment of Missingness

### NMAR Analysis


### Missingness Dependency


## Hypothesis
### Null and Alternative Hypothesis
- Null hypothesis:
- Alternative hypothesis

### Choice of Test Statistic and Significance Level

