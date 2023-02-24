# TastyData

by Yuancheng Cao (yuc094@ucsd.edu), Grace Chen

Credit: [UC San Diego DSC 80 Winter 2023 Course Project Instruction](https://dsc80.com/project3/recipes-and-ratings/)

## Introduction
The **recipes** dataset contains 83782 unique recipes. The **interactions** dataset contains reviews and ratings for recipes in the recipes dataset.
We observed from the recipes dataframe that submission year of recipes ranged from 2008 to 2018, so we are interested into studying **interesting trends of changes to the recipes submitted over time**. This information can be **useful to food providers such as restaurant owners, cookbook author, or even the hdh team at UCSD!**
We will perform EDA on a broad range of columns and determine which exact type of change we want to investigate. The columns we will try to investigate are **'submitted'**, **'minutes'**, **'nutrition'**, **'n_steps'**, **'description'**, **'recipe_id'** from the recipes dataset and **'rating'** from the interactions dataset:
  - 'submitted' column indicate the **date recipe was submitted**, we will modify the column and extract the **year** to faciliate our analysis
  - 'minutes' column indicate suggested **minutes to prepare each recipe**
  - 'nutrition' column indicate **nutrition of each recipe** in specific form, we extract information on **protein** (as PDV) only to for our analysis
  - 'n_steps' column indicate **number of steps in each recipe**
  - 'description' column indicate **user-provided description of each recipe**
  - 'rating' column indicate **rating given by each using for each recipe**, we will compute an **average** for each recipe column based on this data
  - we decided to work with the **merged dataframe**, with **234,429 rows**
    - we realized that some recipes are represented in multiple rows, but we believe this overrepresentation will not significantly affect our data since the large dataset is relatively **robust**
We plan on investigate trends of changes in all above columns **againest the 'submitted' column**.
After EDA, we decided to investigate **if recipes submitted are getting healtheir over time**. In particular, we define recipes with **more protein** as healthier recipes. Therefore, our research question is: **is there a significant change in protein amounts for recipes over time?**

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
<iframe src="assets/fig_year_modi.html" width=600 height=400 frameBorder=0></iframe>

This histogram shows distribution of the 'submitted' column. Specifically, because we will be grouping submission date by years, this histogram also bins data from the ‘submitted’ column by years. The x axis is labeled by each submitted year, the y axis is labeled by count of recipes submitted in the corresponding year. We can see that the shape of the graph is very left skewed. This indicates that much more recipes were submitted before 2010, the earlier years that we are interested in, while much fewer recipes were submitted from 2014 or later, the late years that we are interested in. However, given that the whole dataframe is large enough, the skewed distribution should will not significantly influence our test results. 

<iframe src="assets/fig_minutes_modi.html" width=600 height=400 frameBorder=0></iframe>

These two histograms show distribution of the ‘minute’ column. The x axes are labeled by minutes, the y axes are labeled by count of recipes submitted in the corresponding year. The top histogram shows the distribution of the 'minutes' column after removing outliers > 2880 minutes, which is 0.25% of the data. We can see that the shape of the graph is extremely left skewed. Most recipes have suggested preparing time under 120 minutes. So we again removed extreme values > 120 minutes, which is 10.36% of the data and plot the bottom histogram. In the bottom histogram, we observed spikes at integer points ending in 5s and 0s, such as 30 minutes, 20 minutes, or 15 minutes.

### Bivariate Analysis
<iframe src="assets/avg_protein_by_season.html" width=600 height=400 frameBorder=0></iframe>

The histogram shows the mean protein content for each season. The graph shows that the fall and winter recipes have a higher mean protein content compared to the spring and summer recipes. Because there are many holidays in the fall and winter, such as Labor Day, Veteran's Day, Thanksgiving, Christmas, New Year, etc. People often have meals with family and friends. Meat and other high-protein foods are cooked in different wayss for their meals. This may result in higher overall protein content in these recipes. On the other hand, spring and summer seasons tend to have more light and fresh meals, with a focus on vegetables and fruits, whcih generally have lower protein content.

<iframe src="assets/protein_mb_tags.html" width=600 height=400 frameBorder=0></iframe>

The box plot allows us to explore the relationship between protein content and meat/beef related tags in dataset. First of all, the box plots of the five groupss have outliers. In contrast to the other, the ditrbution of boxplotss with 2 and 4 meat/beef realted taps iss not very diffuse. When the boxplot of the number of meat/beef related label is 0, it shows that only meat and beef have high protein, but also legumes or other foods without these tags still have high protein.

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
Out of the three columns with missing values, we believe that the column 'rating' from the interactions dataset might be NMAR. People are more likely to rate a recipe if they have extreme opinions towards the recipe. On the other hand, people are more likely not rate a recipe or simple forget to rate a reipe if they do not have strong opinions toward the recipe. Therefore, we believe that the missing values in the column 'rating' from the interactions are likely in the middle range of all possible ratings, such as 2s, 3s, and 4s. And the missingness of the column might be NMAR.

From the example recipe given directly from the website, food.com, I saw that viewers have the option to SAVE, DOWNLOAD, PRINT, or SHARE the recipe. If we can obtain information on whether each user corresponding to the user id did any of these four actions, as 4 boolean columns, we might be able to explain the missingness of ‘rating’ by performing test. This is because people who save, download print or share the recipes likely have strong opinions about the recipe, whether positive or negative, and this could relate to their willingness of rating the recipe as well.

### Missingness Dependency


## Hypothesis
### Null and Alternative Hypothesis
- Null hypothesis: There is a significant change in the average protein content of recipes between two periods (2008-2011 and 2012-2014).
- Alternative hypothesis: There is no significant change in the average protein content of recipes between two periods (2008-2011 and 2012-2014).

### Choice of Test Statistic and Significance Level
- Since we are going to group by the recipes by two periods (2008-2011 and 2012-2014) and calcualte the mean of protein content for each period, we can use a permutation test to determine if newer recipes contain more protein. The permutation test would allow for a comparison of the observed difference in mean protein content bewteen two periods against the null hypothesis that there _____.
- The test statistic is the difference in means between the two periods. Because we need to shuffle the year column in the permutation test, we can simulate the null hypothesis and calculate the p-value based on how many times the randomly generated test statistics are equal to or greater than the observed test statistic.
- A significance level of 0.05 can be chosen, which is a commonly used in statistical test. 0.05 which means that there is a 5% chance of rejecting the null hypothesis when it is actually true. This significance level is a good choice because it provides a reasonable level of confidence in the results.

### Simulate the distribution of mean differences under the null hypothesis and p-value

...
### Conclusion

