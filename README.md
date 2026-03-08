# Recipe Ratings Analysis
### By Anna Rosenbaum and Shourya Kulkarni

This project investigates recipes and ratings from Food.com to answer: 
**Do recipes with more ingredients receive higher average ratings?**

Introduction: 
    The recipes and ratings dataset holds recipes and ratings from food.com. The data was originally scraped and used for other data analysis. There are two CSVs in the dataset, one that contains the recipes and the other contains the reviews and ratings submitted for the recipes. The overarching question we are hoping to answer is, What types of recipes tend to have higher average ratings? More specifically, we are looking to answer; Do recipes with more ingredients recieve higher average ratings than recipes with fewer ingredients. 

    The importance of this question and dataset stems from 

    In the raw recipes dataset, there are 83,782 rows and 12 columns. The interactions dataset contains 721,927 rows and 5 columns. For the raw recipes, the main columns that are important to our analysis are the name, minutes, n_steps, and description. These columns indicate the recipe name, recipe ID, the number of minutes to prepare the recipe, the number of steps in the recipe, and the user-provided description. In the interactions dataset, the most important columns are the recipe ID, rating, and review columns. 

    The two datasets were merged together during the data cleaning process and the final dataset contained 83,782 rows and 13 columns. The new column being the average rating. 