# JOB-A-THON - November 2021 by Analytics Vidhya

# Approach for “Predicting Employee Attrition” project

**Introduction:** This file contains approach followed by me for **JOB-A-THON - November 2021 by Analytics Vidhya**. Our task was to predict employee attrition for the sales team of an Insurance Company which is struggling with high attrition rate. We have the monthly information for some employees for year 2016 and 2017 which includes input features for both train and test data. We are supposed to predict whether current employees, whose id is given in a separate csv file, will be leaving the organization in the upcoming two quarters (01 Jan 2018 - 01 July 2018) or not.

**Approach:** Actually this is a classification problem in which we have to predict whether employee will leave organization in the mentioned period or not. But we will use regression in this case. We will build a model from training data using features of candidates who have already left to predict total tenure (number of days) of employment. Then will predict tenure of employees in test data using trained model and finally add predicted tenure to joining date of employees to predict whether a candidate will quit organization in the mentioned period or not.

XGBoost is a powerful approach for building supervised regression models. XGBoost is one of the most popular machine learning algorithm these days and it is a very efficient
implementation of gradient boosting that can be used for regression predictive modeling. So we will use XGBoost algorithm to solve our problem.

**Solution:**
1) Train file has complete data of employees for training as well as testing. Test file only contain Emp_ID of those employees for which prediction is to be done.
2) First we will split original train data in 2 parts – (1) Test Data: employees whose Emp_ID is present in original Test file (2) Train Data: remaining data in original file is
our Train Data.
3) Since our data contains records for employees month-wise. This data has multiple entries for employees. We need to prepare data for model training using groupby function so that final data has a unique entry for each employee and all important features are aggregated. We will also see if new features should be created for better training of our model.
4) Now we will check various factors to decide if new features can be created from already existing features such as:-
 Checking if employee has worked in more than one city.
 Checking if employee's Education_Level changed. e.g. completed higher study and employee changed job for better prospects.
 Checking if employee got increment since some people tend to change immediately after increment.
 Checking if employee was promoted during job since some employees tend to switch job immediately after promotion for better prospects.
5) We will use number of working days (tenure) of employees who have already resigned in our model and drop "Dateofjoining" and "LastWorkingDate" from our data for training of model.
6) We will consider monthly average of "Total Business Value" by each employee in our model. We will also create 3 new features –
(1) last month business value,
(2) whether any month with negative business and
(3) whether any month with zero business for each employee in our model
7) It seems many employees have more than 1 "Quarterly Rating" during tenure. so we will keep first "Quarterly Rating". We will also introduce a new feature for considering
change in "Quarterly Rating".
0 - Last month rating is highest,
1 - No change in rating,
2 - Last month rating is neither highest nor lowest.
3 - Last month rating is lowest.
8) Now we will apply groupby to aggregate train data by Emp_ID and then merge new features with data to create new train data.
9) Now let us drop 3 columns which are not needed for model training:
'MMM-YY', 'Dateofjoining', 'LastWorkingDate'
10) Now drop employees who had not left organization during 2016 and 2017 since these rows are not useful for our model training.
11) Now we will create dummy variables from categorical variables in train/ test data for model training/ testing. Also we need to align our Test Data with our Train data so that dummy variables created are in sync in both data.
12) Now we train XGB model with train data and use Grid search to find best params automatically for our model.
13) Now using best estimators, we will make predictions for tenure of employee and finally add these tenure to joining date of respective employees to finally predict whether employee with leave company in selected period or not.
