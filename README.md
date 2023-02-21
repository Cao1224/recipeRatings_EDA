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
  - Know the datatypes of DataFrame
    
    
  

### Univariate Analysis


### Bivariate Analysis


### Interesting Aggregates
1. This groupby DataFrame shows the count and mean of the `protein` column for each unique recipe ID in the `full_recipes` DataFrame. The resulting DataFrame is sorted in asscending order based on the mean protein value. This DataFrame can help identify which recipes have the highest and lowest protein content, and frequency of each recipe in the `full_recipes` DataFrame.
    Below DataFrame shows first and last 5 rows of DataFrame.
|     id |   count |   mean |
|-------:|--------:|-------:|
| 461235 |       1 |      0 |
| 447587 |       3 |      0 |
| 308738 |       2 |      0 |
| 308774 |       1 |      0 |
| 335660 |       1 |      0 |
2. 

## Assesssment of Missingness

### NMAR Analysis


### Missingness Dependency


## Hypothesis
### Null and Alternative Hypothesis
- Null hypothesis:
- Alternative hypothesis

### Choice of Test Statistic and Significance Level

