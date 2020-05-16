# Project 3 - Web APIs and Classification


This document covers the following:


**A Study on the Use of a Classification Model**
Using tactics such as webscraping, APIs, Natural Language Processing(NLB)
Simplification and combining of existing features
Scraping techniques and the use of tokenizing, lemmatization and stop wards
Extracting features from unstructured text
Hyperparameter tuning
Exploring logistic regression and Naive Bayes 
Building a basic classification model

**EDA and Data Cleaning**
Data Exploration using Python
Studying both numerical and categorical data classification


**Exploratory and Inferential Visualizations**
How variable relationships are distributed and how they interact
Visualizations using matplotlib and seaborn
Visualizing wordcloud and histograms to help inform appropriate inputs to a logistic model
Using visualization techniques to study high freqency words and distributions

**PreProcessing and Modelling**

Applying different transformations before training machine learning models
Implementation of CountVectorizer and TfidfVectorizer


**Model Evaluation**

Naive Bayes

Advantages:
Our Naive Bayes has low computation cost and can efficiently work on a large dataset. This is useful in the long term, if we intend to expand our classification model to other overseas social platforms beyong Reddit.
Naive Bayes is a simple fast and accurate method for prediction and performs well in the case of text analytics problems. 

Disadvantages:
One drawback is the assumption of independence between different variables, wwhich may not hold true in reality.
In practice, it is almost impossible that model will get a set of predictors which are entirely independent.

Logistic Regression

Advantages:
Our Logistic Regression model is efficient and as such the most commonly used classification algorithm. Their co-efficients are interpretable and represent the change in log-odds caused by the input variables.

Disadvantages:

Logistic regression requires quite large sample sizes. This could limit our analysis, especially if sample size is small, resulting from the removal of duplicates or other irrelevant during webscraping.

Our focus will be on Sensitivity for the confusion matrix if the company is small. This is because the opportunity costs of untargeted ads could be quite large.



**Business Recommendations**

More than half of top 40 words appear in either basketball or baseball, suggesting a strong distinction between the two, which provides basis for our classification model.

We recommend a 2 fold approach based on analysis of high freqency words focusing on :

1) cost savings on our client's advertising campaign
    
	We aim to reduce costs by reducing random pay-per-click advertisements which could increase marketing costs.
	This would increase the relevance of our ads to targeted customers. For example, our baseball VR headset could be advertised on the Fangraph websites which is frequently in baseball posts.

2) formation formation of B2B partnerships 

	We also look at forging potential business partnerships by identifying customer engagement with existing vendors. One example is Daimond apparel company.

This creates an optimized, more persuasive and relevant content that could trigger a call to action as it is tailored to the customer.

Baseball posts are generally more popular than basketball (1.1m vs 51k members), with much higher mean 'ups' score. Baseball posts contain more links, while basketball posts contain more original texts.

Our recommendation is to focus on digital advertising for basketball, due to high frequency terms such as 'www', 'http' and 'https'. wW noted that without removal, these terms were in the top 20 high frequency words in the basketball reddit as compared to the baseball reddit. This informs our marketing strategy, that Youtube ads or digital marketing in the form of url links could be implemented.

While our analysis centers on the Reddit website, with a certain demographic, we are cognizant that other social media platforms could have differing demograpics and word frequencies.

Moving forward, we propose further analysis on the plot ratios  of selected plot areas in the neighbourhoods recommended to assess the feasibility of increasing housing density by building upwards.

The correlation between Year Built and Sales Price also suggests that we should be focusing our redevelopment efforts on newer developments to avoid excessive expenditure on construction costs.

