# Data Insights Showcase: Exploring Recipes and Ratings

**Name(s)**: Nahuel Canavy

## Introduction:

We're looking at a dataset from Food.com, a large recipe-sharing site. This dataset has lots of information about recipes and people's reactions to them.

### Main Question:

We want to find out: "Do recipes with more calories get lower ratings?"

This question is important because more people care about healthy eating these days. The answer could help recipe creators, diet-conscious people, and others make better food choices.

### Dataset Info:

Our dataset has many rows, 83782 exaclty, each for a different recipe. The important columns for our question are:

id: A unique number for each recipe.
name: The recipe's name.
nutrition: Shows the recipe's nutritional information, including calories, fat, sugar, sodium, protein, saturated fat, and carbohydrates.
rating: Shows how much users liked the recipe.

### Here is an overview of our dataset.

Picture of our dataset here

## Cleaning and EDA

#### Data Cleaning

Let's clean the data appropriately.
First, let's remove rows where rating or nutrition information is missing
Then, we'll split the nutrition information into separate columns and convert them from object to float.
Finally, here is the dataset we have.

Picture of our dataset cleaned.

So, in the first plot about rating, we can see that a majority of recipes have great ratings, over 4 stars. And is the second plot, we can see the distribution of calories for the recipies. We can see that a lot of recipies are under 2000 calories.

#### Univariate Analysis

Let's plot our first univariate analysis. It's going to be the distributions of ratings.

<iframe src="assets/distribution_of_ratings.html" width=800 height=600 frameBorder=0></iframe>

So, in this first plot about rating, we can see that a majority of recipes have great ratings, over 4 stars. It's only a few recipies that have low ratings.

Let's plot our second univariate analysis. It's going to be the distributions of calories for each recipies.

<iframe src="assets/distribution_of_calories.html" width=800 height=600 frameBorder=0></iframe>

For this second plot, we can see the distribution of calories for the recipies. We can see that a lot of recipies are under 2000 calories.

#### Bivariate Analysis

Let's plot our bivarite analysis. It's going to be the ratings in function of the calories.

Plot here

We can see maybe see a really intersting tendance here, that the majority of recipies that got good ratings are under 2000 calories. However, we must not forget the fact that there are a lot more recipies under 2000 calories so it biased the view of the plot.

#### Interesting Aggregates

Let's see an intersting aggregate, it's going to be the average rating for differents calories levels.

plot here

This aggregate is also really intersting. Indeed, we can see that the average rating is almost the same for every calorie level. So it would appear that the calorie level isn't influencing the recipe rating.

### Assessment of Missingness

#### NMAR Analysis

First of all, let's find which columns is missing some values :

df plot here

Well, we can that the rating columns is the one that got the most missing element. For the two others columns, description and name, it's really only a few so we're going to ignore that and focus on the rating column.

It is possible to consider the "rating" column as potentially being Not Missing At Random (NMAR).Indeed, because of the quantity of missing values that is really big, it's 2609 for a total numbers of values of 81173 making it more than 3% of all the values. This suggests that the missingness is not random. Moreover, we must recall that it's a mean of all the users ratings for a recipe, so it means that all the users didn't rate the recipe making it even more NMAR.

Here are some additional data that could help explain the missingness and potentially make it MAR:

        1. User behavior: Collecting data on user engagement and their propensity to leave ratings could be insightful. By analyzing whether users who engage more with the platform or have a history of leaving ratings are more likely to provide ratings, we can assess if the missingness in the "rating" column is related to user behavior. This information could help determine if the missingness is NMAR or MAR.

        2. Recipe attributes: Gathering data on recipe attributes such as difficulty level, cuisine type, or ingredients might provide insights into why certain recipes have missing ratings. Analyzing whether specific types of recipes or certain attributes are associated with missing ratings could help determine if the missingness is NMAR or MAR.

        3. Time-related factors: Examining the submission date or age of the recipes could provide valuable information. If missing ratings are more likely for older recipes or recipes submitted during a specific time period, it could indicate a relationship between missingness and time. This analysis would contribute to understanding whether the missingness is NMAR or MAR.

#### Missingness Dependency

Let's examine two columns: 'n_steps' and 'n_ingredients'. We hypothesize that recipes with more steps or more ingredients may be more likely to have a rating, as they might be more elaborate and thus more likely to be rated.

Let's first start to plot the distribution of n_steps and n_ingredients when ratings are missings and not missings :

plot 1

plot 2

So, we can see our distributions have kind of similar shape and mean. So we're going to run our permutation test using the K-S statistic.

After procedding with the permutation test, here are the values we obtain :
KS test for 'n_steps': statistic=0.07599870558344557, p-value=3.813369074131651e-13
KS test for 'n_ingredients': statistic=0.023797773652822762, p-value=0.112321477466234

Here is how we can interpret our result :

##### n_steps :

The p-value is extremely small (far less than 0.05). This suggests that we can reject the null hypothesis that the distributions of 'n_steps' are the same for recipes with and without a rating. In other words, the missingness in the 'rating' column is dependent on 'n_steps'. There's a significant difference in the number of steps in recipes with and without ratings.

##### n_ingredients :

The p-value is 0.112, which is greater than 0.05. This suggests that we cannot reject the null hypothesis. In this case, the missingness in the 'rating' column does not seem to be dependent on the 'n_ingredients' column. There isn't a significant difference in the number of ingredients in recipes with and without ratings.

Based on this analysis, it seems like the complexity of the recipe, as measured by the number of steps, might influence whether a recipe has a rating. However, the number of ingredients does not seem to have the same effect.

### Hypothesis Testing

The question we choose at the beginning was :

### Do recipes with more calories get lower ratings?

Therefore, we can state this pair of hypotheses :

Null Hypothesis (H0): The number of calories in a recipe does not influence the rating. This implies that there is no correlation between the number of calories in a recipe and its rating.

Alternative Hypothesis (H1): Recipes with more calories receive lower ratings. This implies that there is a negative correlation between the number of calories in a recipe and its rating.

Test Statistic: We chose the correlation coefficient as our test statistic because we are interested in the relationship between two numerical variables: 'calories' and 'rating'. The correlation coefficient quantifies the strength and direction of this relationship, making it an appropriate choice for our test statistic.

Significance Level: We chose a significance level of 0.05 because this is the standard choice for significance level in many statistical tests.

After running the hypothesis test, here are the values we obtain :
Observed correlation: -0.0013305173991796462
P-value: 0.339

Let's analyse our result :

The observed correlation is approximately -0.0013, which is very close to zero. This suggests that there is almost no linear relationship between the number of calories in a recipe and its rating.

The p-value is 0.339, which is greater than the commonly used significance level of 0.05. This means that we do not reject the null hypothesis. Therefore, based on this test, we conclude that there's no statistical evidence to suggest that recipes with more calories receive lower ratings.

I'm not really suprise since it confirm our intuition from the previous Bivariate Analysis.
