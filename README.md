# Recipe Ratings Analysis
### By Anna Rosenbaum and Shourya Kulkarni

This project investigates recipes and ratings from Food.com to answer: 
**Do recipes with more ingredients receive higher average ratings?**

# **Introduction:** 
    The recipes and ratings dataset holds recipes and ratings from food.com. The data was originally scraped and used for other data analysis. There are two CSVs in the dataset, one that contains the recipes and the other contains the reviews and ratings submitted for the recipes. The overarching question we are hoping to answer is, What types of recipes tend to have higher average ratings? More specifically, we are looking to answer; Do recipes with more ingredients recieve higher average ratings than recipes with fewer ingredients. 

    The importance of the dataset as well as the question being answered is based on the consumer process. It is important to know whether a recipe was rated highly if it contains expensive ingredients and could more ingredients signal a longer prep or cook time which means more time away from work, family, or friends. 

    In the raw recipes dataset, there are 83,782 rows and 12 columns. The interactions dataset contains 721,927 rows and 5 columns. For the raw recipes, the main columns that are important to our analysis are the name, minutes, n_steps, and description. These columns indicate the recipe name, recipe ID, the number of minutes to prepare the recipe, the number of steps in the recipe, and the user-provided description. In the interactions dataset, the most important columns are the recipe ID, rating, and review columns. 

    The two datasets were merged together during the data cleaning process and the final dataset contained 83,782 rows and 13 columns. The new column being the average rating. 

 # **Data Cleaning** 
    Before beginning to look at the dataset in an analytical way, the dataset needed to be cleaned. Specifically it was important to merge the two datasets; raw recipes and interactions together to prodcue one workable dataset. After merging, a column needed to be added to find the average rating for the recipes from the interactions dataset. This was done by first fill all ratings of 0 with np.nan. The main reason for this is because it is the first step in distinguishing between an actual rating of 0 and a missing rating value. This also allows us to prevent skew when averaging it as 0s do affect the average.


# **Exploratory Data Analysis**
