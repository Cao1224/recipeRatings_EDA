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
    
| name                                 |     id |   minutes |   contributor_id | submitted   | tags                                                                                                                                                                                                                        | nutrition                                    |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | description                                                                                                                                                                                                                                                                                                                                                                       | ingredients                                                                                                                                                                    |   n_ingredients |          user_id |   recipe_id | date       |   rating | review                                                                                                                                                                                                                                                                                                                                           |   avg_rating |
|:-------------------------------------|-------:|----------:|-----------------:|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------|----------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------------:|------------:|:-----------|---------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------:|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings'] | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]     |        10 | ['heat the oven to 350f and arrange the rack in the middle', 'line an 8-by-8-inch glass baking dish with aluminum foil', 'combine chocolate and butter in a medium saucepan and cook over medium-low heat , stirring frequently , until evenly melted', 'remove from heat and let cool to room temperature', 'combine eggs , sugar , cocoa powder , vanilla extract , espresso , and salt in a large bowl and briefly stir until just evenly incorporated', 'add cooled chocolate and mix until uniform in color', 'add flour and stir until just incorporated', 'transfer batter to the prepared baking dish', 'bake until a tester inserted in the center of the brownies comes out clean , about 25 to 30 minutes', 'remove from the oven and cool completely before cutting']                                                  | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour'] |               9 | 386585           |      333281 | 2008-11-19 |        4 | These were pretty good, but took forever to bake.  I would send it ended up being almost an hour!  Even then, the brownies stuck to the foil, and were on the overly moist side and not easy to cut.  They did taste quite rich, though!  Made for My 3 Chefs.                                                                                   |            4 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                               | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0] |        12 | ['pre-heat oven the 350 degrees f', 'in a mixing bowl , sift together the flours and baking powder', 'set aside', 'in another mixing bowl , blend together the sugars , margarine , and salt until light and fluffy', 'add the eggs , water , and vanilla to the margarine / sugar mixture and mix together until well combined', 'add in the flour mixture to the wet ingredients and blend until combined', 'scrape down the sides of the bowl and add the chocolate chips', 'mix until combined', 'scrape down the sides to the bowl again', 'using an ice cream scoop , scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking', 'bake for 10 - 15 minutes or until golden brown on the outside and soft & chewy in the center', 'serve hot and enjoy !'] | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                            | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']                    |              11 | 424680           |      453467 | 2012-01-26 |        5 | Originally I was gonna cut the recipe in half (just the 2 of us here), but then we had a park-wide yard sale, & I made the whole batch & used them as enticements for potential buyers ~ what the hey, a free cookie as delicious as these are, definitely works its magic! Will be making these again, for sure! Thanks for posting the recipe! |            5 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |  29782           |      306168 | 2008-12-31 |        5 | This was one of the best broccoli casseroles that I have ever made.  I made my own chicken soup for this recipe. I was a bit worried about the tsp of soy sauce but it gave the casserole the best flavor. YUM!                                                                                                                                  |            5 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      1.19628e+06 |      306168 | 2009-04-13 |        5 | I made this for my son's first birthday party this weekend. Our guests INHALED it! Everyone kept saying how delicious it was. I was I could have gotten to try it.                                                                                                                                                                               |            5 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 | 768828           |      306168 | 2013-08-02 |        5 | Loved this.  Be sure to completely thaw the broccoli.  I didn&#039;t and it didn&#039;t get done in time specified.  Just cooked it a little longer though and it was perfect.  Thanks Chef.                                                                                                                                                     |            5 |


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

