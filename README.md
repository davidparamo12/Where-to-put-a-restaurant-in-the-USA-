# ¿Where to put a restaurant in the USA?
The goal of this repository is to determine the most strategic location to open a new restaurant — specifically, where the likelihood of attracting customers is highest. This analysis leverages both Google data and Yelp to identify areas with the greatest potential for success.



## INTRODUCTION

Project Objective: This project aims to help a food industry investor open a new restaurant in the U.S. with the highest chance of success. The goal is to identify a location and restaurant category where gaining market share is easier and more profitable.

Approach:

  1. Find a high-volume restaurant category to ensure strong potential demand.
  2. Detect underserved areas by locating counties where customer demand is unmet, making market entry easier.
  3. Identify key success factors by analyzing what customers value most in that category.

Step by step:

We will first use web scraping to collect population density data from Census.com to choose the state with the largest potential customer base. Then, using review data from Google and Yelp, we will:

  * Map reviews by latitude and longitude at the county level.

  * Apply machine learning to analyze the sentiment of each review grouped by county.

  * Identify the counties and categories with the lowest sentiment scores (unmet demand)

Finally, we will build a machine learning model to explain the causes of negative sentiment, allowing the investor to design a restaurant that meets customer needs and gains market share effectively.



⚠️ Dataset Notice

The original dataset files used in this project are too large to be uploaded to GitHub (they exceed the repository file size limits).

For this reason, the datasets are not included in this repository.
If you want to reproduce the analysis, you can either:

    * Download the datasets directly from their original sources (Google Maps and Yelp), or

    * Contact the repository owner to request access to the files.

Note: Although we initially tried using the Python geohash library to merge Google and Yelp datasets, it did not produce reliable results. Therefore, both analyses were performed separately.





## PROJECT STRUCTURE


1. Introduction – The objective of the project.

2. Dataset – Explanation of the dataset and its relevant variables. We will also describe the data lifecycle.

3. Notebooks – Jupyter Notebook files containing all the code for the ETL and EDA processes.

4. Machine Learning – Explanation of the CRISP-DM methodology applied to the project.

5. Report and Visualizations – Presentation of the most relevant visualizations (the complete analysis can be found in the notebooks).

6. Final Conclusions – Summary of findings addressing the objective stated in the introduction.




## DATASET

Important note:
To filter the data, we used the longitude and latitude variables because the State and City fields were unreliable (e.g., cities like Seattle appeared under the wrong state).
Since longitude and latitude values come from user devices, they are much more accurate.

Data Lifecycle

  1. Population Data (Census)
    * Collected population density and socioeconomic data from Census.com
    * Identified New Jersey as the state with the highest population density.
    * Gathered data for New Jersey’s 21 counties: median income, house values, and property taxes.


  2. Google Maps Data
    * All U.S. businesses: 11 JSON files merged and filtered by latitude/longitude.
    * New Jersey folder: 13 JSON files merged into a single Parquet file for efficient processing.

  3. Yelp Data
    * Business file (Pickle format): filtered by latitude/longitude.
    * Review file (JSON): partitioned for processing.

  4. Exploratory Data Analysis (EDA)
     
  5. Reducing the data to SALEM and Fast Food Category.
     
  6. Machine Learning
     
  7. Insights




We will focus specifically on consumer reviews from Google and Yelp located in New Jersey, the most densely populated state in the U.S.

🤖 Machine Learning Methodology (CRISP-DM)

We follow the CRISP-DM methodology, which has 6 steps:

Step 1 – Business Understanding

Goal: Provide our client with data-driven insights to optimize their restaurant investment.

How: Build a competitive landscape analysis for 4 restaurant categories across the 21 counties of New Jersey.

Identify category strengths and weaknesses

Locate counties with high market opportunities

Understand the customer segments with highest potential

Build a predictive sentiment model to estimate how consumers might respond to a new restaurant

Step 2 – Data Understanding

Generate a Vader Compound score using SentimentIntensityAnalyzer to classify reviews as positive, neutral, or negative.

Combine Google and Yelp review sentiments into a single score.

Analyze general sentiment metrics: mean, variance, and distribution.

Group sentiment by:

Socioeconomic level

County

Time (date)

Perform:

User clustering (create “influencer” variable from Yelp)

EDA on Yelp tips to extract keywords and create dummy variables

Heatmaps to explore correlations between variables

EDA Key Questions

Has sentiment evolved over time in NJ?

How does county median income affect sentiment?

What is the link between “influencers” and restaurant sentiment?

Are tips consistent? What keywords drive positive or negative reviews?

Step 3 – Data Preparation

Clean and extract the most useful features for modeling:

Business hours (24/7 or not)

Attributes (parking, drive-through, etc.)

Star ratings (expected vs. actual satisfaction)

Step 4 – Modeling

Try multiple ML models (mostly categorical data):

Decision Trees / Random Forest – step-by-step path to positive/negative sentiment

Logistic Regression – predict positive vs. negative sentiment

XGBoost – ensemble of decision trees correcting each other’s errors

Step 5 – Evaluation

Assess if the model meets its business goal.

By this stage we will have an MVP that predicts review sentiment from input variables, offering insights to design a restaurant concept with higher success probability.

Step 6 – Deployment

Deploy the MVP so the user can input variables (location, attributes, etc.) and obtain a predicted sentiment score for their proposed restaurant.
