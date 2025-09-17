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






## PROJECT STRUCTURE


1. Introduction – The objective of the project.

2. Dataset – Explanation of the dataset and its relevant variables. We will also describe the data lifecycle.

3. Notebooks – Jupyter Notebook files containing all the code for the ETL and EDA processes.

4. Machine Learning – Explanation of the CRISP-DM methodology applied to the project.

5. Report and Visualizations – Presentation of the most relevant visualizations (the complete analysis can be found in the notebooks).

6. Final Conclusions – Summary of findings addressing the objective stated in the introduction.




## DATASET

Important note:
1. To filter the data, we used the longitude and latitude variables because the State and City fields were unreliable (e.g., cities like Seattle appeared under the wrong state).
Since longitude and latitude values come from user devices, they are much more accurate.

2. Although we initially tried using the Python geohash library to merge Google and Yelp datasets into one dataset but it did not produce reliable results. The gmaps_id and yelp_id are not the same, therefore both analyses were performed separately.

Data Lifecycle

  1. Population Data (Census)
    * Collected population density and socioeconomic data from Census.com
    * Identified New Jersey as the state with the highest population density.
    * Gathered data for New Jersey’s 21 counties: median income, house values, and property taxes.


  2. Google Maps Data
    * All U.S. businesses: 11 JSON files merged and filtered by latitude/longitude.
    * New Jersey folder of the reviews: 13 JSON files merged into a single Parquet file for efficient processing.

  3. Yelp Data
    * Business file (Pickle format): filtered by latitude/longitude.
    * Review file (JSON): partitioned for processing. It is the biggest file of the dataset.
    * Tip (JSON): This will help us understand what is lacking in the food category in order to give valuable feedback to the investor.

  5. Exploratory Data Analysis (EDA) for each dataset; Google and Yelp.
     
  6. Reducing the data to SALEM and Fast Food Category. Understand why this is our goal.
     
  7. Machine Learning
     
  8. Insights



## NOTEBOOKS


The notebooks for ETL, EDA and ML are avaible in the repository. Any questions, please contact the repository owner.


## MACHINE LEARNING

We will focus specifically on consumer reviews from Google and Yelp located in New Jersey, the most densely populated state in the U.S.

🤖 Machine Learning Methodology (CRISP-DM)

We follow the CRISP-DM methodology, which has 6 steps:

  1. Step 1 – Business Understanding
    Goal: Provide our client with data-driven insights to optimize their restaurant investment.
    How: Build a competitive landscape analysis for the top restaurant categories across the 21 counties of New Jersey.
  
    1. Identify category weaknesses specially. 
    2. Locate counties with high market opportunities- demands from the consumers are unmet.

  
  2. Step 2 – Data Understanding

    1. Generate a Vader Compound score using SentimentIntensityAnalyzer to classify reviews as positive, neutral, or negative.
    2. Analyze general sentiment metrics: mean, variance, and distribution.
    3. Group sentiment by: 
        * Socioeconomic level
        * County
        * Time (date)
    4. Creating the dummy variable "influencer" for Yelp dataset. ¿Does "influencers" have an impact in the restaurant reviews? We found three type of influencers.
    5. Creating dummy variables from the MISC Google dataset like: delivery, drive-through, casual, breakfast, lunch, dessert, etc.
    6. Heatmaps in order to see the correlation of the variables. It is important to note that correlation is not the same as causation. It just helps to understand in deeper sense the         data.
    7. ¿Has sentiment evolved over time in NJ?
    
  3. Step 3- Data preparation
     
    1. The main challenge in managing the data was its large size, which made it difficult to read on a regular computer. To address this issue, a filtering process was implemented    using the latitude and longitude variables: only rows with values within the defined boundaries were kept in the new dataframe. 
    2. As a result, the filtered data contained only restaurants located in New Jersey.
    3. The next step involved categorizing the restaurants. First, the complete list of restaurants was extracted, and an AI-assisted approach was used to create a dictionary to                classify all restaurants.
    4. Each row was then mapped to this dictionary, and restaurants marked as permanently closed were removed from the dataframe. Only the four largest categories were retained, as the         initial objective focused on areas within the highest demand.
    5. After removing unnecessary columns, the dataset was merged with the reviews.
    6. The final dataframe was saved in parquet format to proceed with the exploratory data analysis (EDA).This step helps reduce RAM usage.
    7. After loading the new DataFrame, the reviews were tokenized, and the VADER Sentiment Analysis tool was applied to determine the sentiment of each review.
    8. For the Google dataset, dummy variables were created from the MISC column to capture features such as "drive-through," "takeout," "comfort food," and others. For the Yelp dataset, a wide range of user-related variables was used to perform clustering aimed at identifying types of influencers: top influencers, mid-level influencers, and micro-influencers.
    9. Finally, the data was partitioned by food category to allow machine learning models to analyze each category separately. Nevertheless, we only focused in fast food as it was the category where it showed a demand unmet.
  
  4. Step 4- Modeling
     
    1. A heatmap was created as an initial exploratory step. As is well known, correlation does not imply causation.
    2. Because most of the variables are discrete rather than continuous, the modeling approach focused on algorithms such as logistic regression, decision trees, and similar methods like XGBoost and Random Forest.
    3. After running several models, it became evident that there was a major issue with precision, especially for the negative class. This occurred because the number of negative and neutral samples was much lower than the number of positive samples. In some cases, the model predicted only positive outcomes yet still achieved a relatively high F1-score. This was due to the class imbalance between positive, neutral, and negative values.
    4. To address this issue, the SMOTE resampling technique was applied, and the models were also trained using class_weight="balanced".
    5. These adjustments improved precision; however, the variables that explained sentiment remained limited.

  5. Step 5- Evaluation

    1. The models were evaluated using common metrics: accuracy, precision, recall, and F1-score.
    2. To improve visualization, a confusion matrix was also used.
    3. The ROC curve and AUC were included as well. For this evaluation, the data was converted into two classes—positive and negative—by merging the neutral class into the negative class to improve class balance.
    4. Cross-validation was performed, ensuring that the classes were balanced during the process.


  6. Step 6- Deployment

    1. For the deployment of the model it was used a FastApi aplication.


## REPORT AND VISUALIZATION

The data shows that the District of Columbia is the most dense place of the USA. Nevertheless, the District of Columbia is not a state. Therefore, the most dense state is New Jersey with 1,263 persons per square mile. You can confirm this information from the Web-scrapping folder.

After analyzing the data in NJ (New Jersey), this are the top food categories.

<img width="820" height="492" alt="image" src="https://github.com/user-attachments/assets/73daf4d6-4dca-4e95-aaf8-f4e5736ec9cd" />


After grouping each category by sentiment, the data showed that the County with the highest demand unmet was FAST FOOD- with the Google data and Yelp data. 

In the Google dataset, the Salem County has 0.27 in the vader sentiment, the lowest of all the food county and food categories. 

<img width="684" height="545" alt="image" src="https://github.com/user-attachments/assets/4f4d462f-abe7-4f02-9928-871d7a264609" />

Overall, the fast food category appears to have the most issues based on the reviews and their sentiment. In contrast, categories like Pizza and Pasta show less variation and generally receive higher ratings than Fast Food.

<img width="681" height="545" alt="image" src="https://github.com/user-attachments/assets/644f792a-91f9-44cb-b1a1-a51da6e4df67" />

After applying a logarithmic transformation to the user-related variables in Yelp—given their strong skewness—a clustering model was then developed to identify different types of influencers. The data showed that 3 clusters was the optimal number of clusters:

<img width="574" height="449" alt="image" src="https://github.com/user-attachments/assets/5e3817d2-76de-4284-b80a-d3634adc4125" />

As mentioned before; the clusters were named as top influencers, middel influencer and micro influencer.


The Yelp dataset also shows that the Fast Food category has the biggest problem and specially in the County of Salem.  

<img width="678" height="547" alt="image" src="https://github.com/user-attachments/assets/1e7685b4-747f-4270-be13-30818ed31417" />

At the moment to explain 
<img width="1620" height="1150" alt="image" src="https://github.com/user-attachments/assets/45941522-92dd-4c0e-8d8d-36ad65035eb6" />

