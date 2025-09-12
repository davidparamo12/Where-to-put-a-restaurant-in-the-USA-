# ¿Where to put a restaurant in the USA?
The goal of this repository is to determine the most strategic location to open a new restaurant — specifically, where the likelihood of attracting customers is highest. This analysis leverages both Google data and Yelp to identify areas with the greatest potential for success.

## INTRODUCTION

Project Objective

This project aims to help a food industry investor open a new restaurant in the U.S. with the highest chance of success. The goal is to identify a location and restaurant category where gaining market share is easier and more profitable.

Approach

Find a high-volume restaurant category to ensure strong potential demand.

Detect underserved areas by locating counties where customer demand is unmet, making market entry easier.

Identify key success factors by analyzing what customers value most in that category.

We will first use web scraping to collect population density data from Census.com to choose the state with the largest potential customer base. Then, using review data from Google and Yelp, we will:

Map reviews by latitude and longitude at the county level

Apply machine learning to analyze the sentiment of each review

Identify the counties and categories with the lowest sentiment scores (unmet demand)

Finally, we will build a machine learning model to explain the causes of negative sentiment, allowing the investor to design a restaurant that meets customer needs and gains market share effectively.

Note: Although we initially tried using the Python geohash library to merge Google and Yelp datasets, it did not produce reliable results. Therefore, both analyses were performed separately.

