# Project 4: Predicting Outbreaks of West Nile Virus in Chicago

## Problem Statement

To train a classifier to predict when and where different species of mosquitos will test positive for West Nile Virus (WNV). WNV is most commonly spread to humans through infected mosquitos. Around 20% of people who become infected with the virus develop symptoms ranging from a persistent fever, to serious neurological illnesses that can result in death.

A more accurate method of predicting outbreaks of West Nile virus in mosquitos will help the City of Chicago and CPHD more efficiently and effectively allocate resources towards preventing transmission of this potentially deadly virus.

Success will be evaluated via the area under the receiver operating curve (ROC) and sensitivity:

* ROC is a probability curve and area under the curve (AUC) represents the degree or measure of separability. It tells how much a model is capable of distinguishing between classes. The higher the AUC, the better the model is at predicting 0s as 0s and 1s as 1s. In this context, the higher the AUC, the better the model is at distinguishing between whether the virus is present or not present.
* Sensitivity measures the proportion of actual positives that are correctly identified as such - in this case, it measures the proportion of mosquitos predicted to test positive for WNV that are correctly identified as such. This recognises the need to reduce false negatives so that the authorities can be more conservative in their mosquito control measures.

---

## Executive Summary

Around 20% of people who become infected with the virus develop symptoms ranging from a persistent fever, to serious neurological illnesses that is prolonged and even death. The datasets are obtained from Kaggle: https://www.kaggle.com/c/predict-west-nile-virus/data.

There are the train, test, weather and spray datasets. The train and test sets were processed similarly, by removing the address, block and street since latitude and longitude suffice in capturing information on location. nummosquitoes were also removed from the train set since it does not exist in the test set. Much of the cleaning effort went into the weather dataset where missing and trace values were imputed. Four variables - wetbulb, tavg, dewpoint, preciptotal - were ultimately chosen from the weather dataset and were feature engineered into our test and train sets. They were chosen based on an article found here: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3774453/, which details the environmental drivers of the virus. Feature-engineering of some of the variables were done to handle the correlation between the variables tavg, dewpoint and wetbulb.

We applied the SMOTE technique to make up for the very unbalanced dataset, with approximately 94.8% from class 0 (West Nile Virus not present). Classifier models - K-nearest Neighbours, Logistic Regression, Random Forests, Decision Trees, Extra Trees and ADA Boost with hyperparameter tuning conducted via randomised search.

The model that was found to be of the highest ROC AUC score is Logistic Regression. We observe minimal overfitting of the model when comparing both the accuracy and AUC scores of the training and validation datasets.

Besides having the highest ROC-AUC score among the models run, the Logistic Regression model has the highest sensitivity of 0.650794, which is an important metric in this case. This means that 65% of wnvpresent cases were identified correctly. False negatives need to be reduced so that mosquito control measures can be done at all areas which have the virus.

---

## Directory Structure
```
project-4: Predicting Outbreaks of West Nile Virus in Chicago
|__ code
|   |__ 01_Data_Cleaning_and_EDA.ipynb   
|   |__ 02_Preprocessing.ipynb   
|   |__ 03_Modelling.ipynb
|   |__ 04_Kaggle_Submission.ipynb
|__ assets
|   |__ mapdata_copyright_openstreetmap_contributors.txt
|   |__ spray.csv
|   |__ spray_clean.csv
|   |__ test.csv
|   |__ test_clean.csv
|   |__ test_pred.csv
|   |__ test_preproc.csv
|   |__ train.csv
|   |__ train_clean.csv
|   |__ train_preproc.csv
|   |__ weather.csv
|   |__ weather_clean.csv
|__ Project 4 Group 2.pdf
|__ README.md
```
---

## Data Cleaning

1. Train Dataset
	* dropped duplicates
2. Weather Dataset
	* imputed missing values for wetbulb using the median because the distribution is left skewed
	* imputed missing values and trace amount(<0.005) for preciptotal with 0
	* dropped duplicates
3. Spray Dataset
	* dropped duplicates
	* missing values in the time column led to dropping the column from the analysis since time is too granular for the purpose of our analysis.

---

## Pre-processing

1. Concatenated train and test datasets and kept the following columns: 'id', 'date', 'species', 'trap', 'latitude', 'longitude', 'nummosquitos', 'dataset', 'wnvpresent'
2. Feature engineered the weather variables to include wetbulb * tavg, wetbulb * dewpoint and wetbulb * dewpoint * tavg
3. Merged weather dataset with train and test set on 'date'.

---

## Exploratory Visualisations

1. Train dataset
	* Count plot of presence of West Nile Virus and species of mosquitoes.
	This led to the discovery that only 3 species of mosquitoes are hosts to the virus. They are culex pipiens/restuans, culex restuans and culex pipiens.
	* Line plots of average number of mosquitoes by months. The peak is around the months of July and August.
	* Map of trap locations. There are a total of 136 trap locations and that T900 refers to the location (at O'hare airport) which has the most number of traps at 750.

2. Weather dataset
	* Distribution plots for 4 of the variables - tavg, dewpoint, preciptotal, wetbulb.
	tavg, dewpoint and wetbulb have left-skewed distributions while preciptotal has an overwhelming mode of 0.
	* Scatter plots of tavg, wetbulb and dewpoint against temperature.
	There is clear separation between tavg and dewpoint while there seems to be overlap between wetbulb with dewpoint and wetbulb with tavg, which may suggest correlation.

3. Spray dataset
	* Map of spray location and trap locations.
	Spray locations are centralised to specific clusters within the area covered by the trap locations.

---

## Modelling

1. Applied Synthetic Minority Oversampling Technique (SMOTE) to balance the classes
2. Chose classifier models - K-nearest Neighbours, Logistic Regression, Random Forests, Extra Trees, Decision Trees and ADA Boost and ran randomised search for hyperparameter tuning
3. Model Selection
The model was selected based on accuracy, ROC AUC and sensitivity. The accuracy of the production model is 68.1% on the validation set, compared to the baseline accuracy of 94.8%. The reason for the lower accuracy is that the dataset is extremely imbalanced. The baseline model will also never predict that a location has West Nile Virus, which is probably going to lead to outbreaks of the disease.
The final model picked is Logistic Regression with 65% sensitivity rate. We want to maximise sensitivity because the cost of predicting a false negative can lead to loss of lives.

---

## Cost-Benefit Analysis


We can quantify the costs of pesticide spraying by looking at the explicit costs associated with spray implementation.

Approximately USD 2.6M spray cost :

### Quantitative Cost Factors

**Method 1**

1. Cost of Zenivex E4 = USD 80 per gallon (https://www.cmmcp.org/pesticide-information/pages/zenivex-e4-etofenprox)
2. Estimated cost of pesticides for each sprayer truck is USD 800-1,600 (based on spraying at 4.5 - 9 ounces per minute, at a vehicle speed of 10 - 15 mph)
3. Estimated cost of pesticides for all sprayer trucks is USD 1.6M in total (based on 1,000 trucks requires to cover the total area of Chicago is 606.1 km (http://www.gfmosquito.com/wp-content/uploads/2013/06/2013-North-Dakota-Bid-Tabulation.pdf)

Total cost = USD 1.6-3.25M

**Method 2**

4. Cost of spray trucks (USD 400-650 per hour x 5 hrs x 1,000 trucks = USD 2-3.25M) (http://meepi.org/wnv/overkillma.htm)
5. Miscellaneous advertising costs related to mosquito control notices

### Qualitative Cost Factors

Human and ecologic risks associated with the pesticide spray (http://envinfo.org/appendix%20ii-c.html)

### Quantitative Benefit Factors

We can quantify the benefit of pesticide spraying by looking at the aversion of loss of income which could have resulted from WNV. 	

Approximately USD $1.23M from medical/hospitalization expenses and loss of income due to WNV, as follows:

1. Potential aversion of loss in household income:

	- The annual median household income in Chicago was USD 57,238, as of 2018  and can be used as an estimate to measure the loss of income during patient recovery from WNV (https://datausa.io/profile/geo/chicago-il/).
	- Given that 1 in 5 experience symptoms such as headache, body aches, joint pains, vomiting, diarrhea, or rash , this could mean that approximately 35 people out of 176 may have been hospitalized or received medical treatment (https://www.cdc.gov/westnile/symptoms/index.html). If these were working adults and required two weeks off work to recover, this would have resulted in a total loss of income of USD 2,200 per patient over 2 weeks or USD 77,000 for 35 patients.


2. Potential aversion of hospital and medical expenses:

	- Method 1: In 2018, there were 176 WNV cases reported in Illinois where Chicago  is located.  On average, each WNV patient spends approximately USD 33,143 per inpatient and USD 6,317 per outpatient for all treatments. Total cost of 35 patients would amount to 1,160,005. Therefore the total monetary loss caused by WNV in 2018 could be as high as USD 1,237,005 in total (https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3322011).
	- Method 2: Studies have shown that the WNV hospitalization costs are annually USD 56M for the United States. If we divide this amount by 50 states, this amounts to USD 1.12M a year for Illinois where Chicago is located (https://www.sciencedaily.com/releases/2014/02/140210184713.htm).


3. Potential aversion of medical expenses of non-WNV issues
Our analysis also does not include medical costs associated with non-WNV issues which may have been resolved through spraying of pesticides, such as mosquito-bite allergenicity or potential adverse sequelae, which are unaccounted for but may be significant.

4. Potential aversion of loss of income in business or tourism: more data is required on this

### Qualitative Benefit Factors

1. Pain and distress are difficult to quantify monetarily
2. Averted medical costs associated with non-WNV issues are difficult to account for
3. Benefits from mosquito spraying such as increased quality of life from fewer people falling sick and increased workplace productivity from fewer people falling ill and going on medical leave, are less easily quantifiable.

### Cost-benefit Assessment

It is inconclusive that benefits of pesticide spraying exceeded its costs. Based on quantitative analysis, the actual/realised monetary costs associated with spraying appear to exceed the benefits. However, given that WNV cases may be underestimated and factoring in the qualitative factors, mentioned above, results are inconclusive. Regarding unreported figures, the actual number of persons affected with WNF remains unknown as the total number of WNF cases have been underreported or underdiagnosed. The Centers for Disease control estimates that for every confirmed case, there’s 30 to 60 cases that go unreported (https://www.abc10.com/article/news/local/).


---

## Conclusions and Recommendations


### A. Our Classifier

We have trained a classifier to predict when and where different species of mosquitos will test positive for West Nile Virus (WNV). Our classifier is a Logistic Regression which gives a rather high sensitivity rate of 65%.

**Limitations**

1. The limitation of this model lies in the use of time-series data which could mean that the data is autocorrelated. This can lead to implications on the computation of standard errors.
2. Inclusion of all dates in the training and testing dataset. The spray dataset could have been used to exclude observations from the training and test sets. If Chicago Municipality would like to know how to allocate spraying efforts, we could have filter out the effect of recent spraying from our predictive model, as it is target-linked.

**Area for further investigation**

We have yet to explore the variable of number of mosquitoes. A larger population size of mosquitoes would mean that there are more mosquitoes which can become infected, and it also increases the number of interactions among hosts, allowing for a faster spread of the virus. The size of the population could be estimated from the number of mosquitoes found in each trap. However, as this variable is not present in the test set, we dropped it from our training dataset. In future analysis, we could impute the value from other variables so that it can be used as an input.

### B. Further Efforts in Spraying

Based on our observations, the duration and magnitude of spraying could be improved by the following approaches:

**A Targeted Approach**

1. Targeting WNV geographic clusters that we identified in our EDA, so as to allow for broader coverage.
2. Increase spraying frequency during WNV seasonal periods, as identified during our EDA, such as in August where mosquitoes numbers tend to increase.
3. Targeting of one species (Culex Pipiens/Restuans) which stands out as a carrier with higher rates of WNV in our EDA. We propose reducing this species in particular through targeted biological mosquito control which would make spraying more efficient and less costly.

As 14,244 sprays were conducted over 10 days, this could be viewed as excessive and temporal. Our EDA comparing spray locations and WNV incidences also suggests that spray efficacy could be limited since incidences of WNV were still present during periods after pesticide spray.

**A Holistic Approach**

1. Implementing other vector control measures such as education campaigns (such as using insect repellents and emptying water from containers to reduce exposure) (http://phsneb.org/programs/population-protection/environmental/west-nile-virus/).
2. More use of larvicides instead of adulticides. Spraying of pesticides (adulticides) mainly works against winged mosquitoes as compared to larvicides that work in standing water against mosquito larvae. Larvicides can be targeted to mosquito breeding sites, which could help to reduce cost. Given that there are over 210,000 catch basins in Chicago which are excellent breeding grounds for mosquitoes, more frequent use of larvicides could be carried out (https://www.chicago.gov/city/en/depts/cdph/provdrs/healthy_communities/svcs/report_standing_water.html).

---

## Data Sources


Kaggle website (https://www.kaggle.com/c/predict-west-nile-virus/data)
