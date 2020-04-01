# Classifying the Drivers of Recent Venezuelan Migration to Colombia: Multi-Class Classification Modeling

## Contents of this Repo:
Contained within this repo are:
- **Notebook_migration_classification_model.ipynb** - *Jupyter Notebook detailing full analysis with all code and mark-ups for executing the multi-class classification models*
- **ExecSummary_NonTechnical_migration_classification_29Feb2020.pdf** - *Non-technical summary of findings, based on completed analysis*
- **Data Folder** - *Contains .sav files of disaggregated data files as obtained from the Colombian Department of Statistics data portal*
	- *Also contained within this folder is an excel of the variables across the different survey modules used in this analysis, for matching*
- **Images Folder** - *Contains images of charts used within jupyter notebook*


## Project Overview
The goal of this project is to build a classification model, in this case a multi-class classification model. I decided to use survey data from the Colombian Government's Department of Statistics (DANE). In particular, I wanted to use the National Economic Survey, focusing on the Migration Module of the survey, to see if the available data could sufficiently classify reasons for migration from Venezuela to Colombia. With this in mind, I subset the data identifying only respondents who fit this criteria, scrubbed the data, explored the selected survey module variables, and used scikit-learn classification models and GridSearchCV to build classification models and evaluate their performance.


## Project Details:
- Data Used: Microdata was downloaded from the sites below and merged into a final dataset used for this project. Individual microdata csv and sav files are available in this repo as well.
	- 2019 GEIH (Gran Encuesta Integrada de Hogares = Grand Integrated Household Survey): http://microdatos.dane.gov.co/index.php/catalog/599/study-description
	- 2019 Migration Module of GEIH: http://microdatos.dane.gov.co/index.php/catalog/641/related_materials
In this notebook, I used the sav files so that I could access the survey metadata for understanding the variable labels and value labels.

Packages Used:
	- pandas
	- numpy
	- missingno
	- pyreadstat
	- matplotlib and seaborn
	- scikit-learn

Models Used:
	- Decision Tree Classifier
	- Support Vector Machine Classifier
	- Random Forest Classifier
	- AdaBoost Classifier


## Project Approach: OSEMiN
Obtain:
	- Data was obtained from the Colombian National Department of Statistics, specifically from the 2019 GEIH survey.
	- Data was merged into one complete dataset.
Scrub:
	- The data was sliced to only include a subset of the total data, specifically focused on respondents that originated from Venezuela as of 12 months prior to the implementation of the survey
	- Null values were closely examined and visualized prior to making decisions on whether to keep or remove them.
Explore:
	- initial exploration was conducted to understand how variables behaved in relation to the target variable
	- visualizations of categorical data prior to label encoding using get_dummies
Model:
	- A baseline classification model was produced - Decision Tree Classifier
	- Hyperparameter tuning with GridSearchCV with 3 models:
		- SVM
		- Random Forest Classifier
		- AdaBoost Classifier
	- Weighted F1 score was used as the main model performance metric due to class imbalance issue
Interpret:
	- Summary of findings and model performance
	- Suggestions for additional analysis and areas for improvement

## Summary of Analysis and Findings
Our dataset has clear class imbalance issues, with a few labels accounting for less than 5% total of the data combined, while others represent between 25-30% of the data each. For this reason, in each of our models we set the class_weight to 'balanced'. This is also why we have opted to use the weighted F1 score as our primary model performance metric.

![Training Labels](http://localhost:8889/view/Images/train_labels_barchart.png)

We used four classification algorithms, building a baseline model using a simple Decision Tree Classifier, before moving onto building SVM, Random Forest, and AdaBoost Classification models with GridSearchCV for hyperparameter tuning.

Our Baseline model using a Decision Tree classifier, as with the SVM, are both greedy algorithms, meaning they will always choose to maximize information gain at each step. This means that it doesn't matter how many trees are added to our forest if they are all trained on the same dataset. In contrast, with Ensemble methods the idea is that variance is a good thing, and we are seeking high variance amongst the trees in our Random Forest through the use of Bagging and/or Subspace Sampling. The goal is to achieve high diversity among the trees in our forest, then our model will be more resilient to noise and therefore less prone to overfitting.

Below are the model performance metrics for each of the models when run on the test set:

**Weighted F1 Scores Across Models**
--------------------------------------
Baseline Model (Decision Tree Classifier): 0.43568173874600313

Support Vector Machine Classifier: 0.5158851220445836

Random Forest Classifier: 0.5280485965896458

AdaBoost Classifier: 0.5251736191463809

The Random Forest Classifier was the highest performing, with a weighted F1 score of 0.528, which is not an ideal F1 score, but it is an improvement compared to the baseline model which only achieved a weighted F1 score of 0.436. The optimal parameters for our RF Classifier were a max_depth of 10, 120 estimators, and the gini criterion, which is used to quantify variance in multi-class classifications.

![RF Classification Report](http://localhost:8889/view/Images/rf_report.png)

Precision and Recall scores were similar across each of the labels, however, as with all of our models, there were often a few labels that had no metrics likely due to the fact that there were too few data points pertaining to that label (such as Ethnic Group Cultural Reasons, Threat/Risk due to Armed Conflict, and To make a new home.

![RF Confusion Matrix](http://localhost:8889/view/Images/rf_cm.png)

Across all models, the two numerical variables had the highest feature importance (age and number of years of schooling). It would be interesting to see how this model performs with the inclusion of more numerical data. The categorical variable with the highest feature importance was P1670-Si, which represented respondents who are currently students (school, college, or university).