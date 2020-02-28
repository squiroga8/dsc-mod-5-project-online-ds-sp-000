# Module 5 Project: Classifying the Drivers of Recent Venezuelan Migration to Colombia

## Contents of this Repo:
Contained within these repo are:
- The **Module5Project_ExecutiveSummary_SerenaQuiroga_28Feb2020.pdf** - *Non-technical summary of findings, based on completed analysis*
- The data used: **Data folder**
- The **Module_5_Final_Project jupyter notebook** - *with all code and mark-ups for executing the multi-class classification models*
- An excel file of **Variables_SelectModeuls_GEIH** containing a quick reference to the survey variables and question text, used for identifying which variables were appropriate for using in data merging.

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
	- pyreadstat
	- scikit-learn
	- matplotlib
	- seaborn

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
	- A baseline classification model was produced
	- Hyperparameter tuning with GridSearchCV
	- Weighted F1 score was used as the main model performance metric
Interpret:
	- Summary of findings and model performance
	- Suggestions for additional analysis and areas for improvement