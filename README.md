######Aircraft Risk Assessment for a New Business Endeavour.

This project seeks to guide the company as it ventures into operation of commercial and private airplanes. The main focus is on comparing the risk levels of different aircraft makes and come up with aircraft models that present the lowest risk and highest potential for successful operations.
Using Exploratory Data Analysis(EDA), the dataset from the National Transportation and Safety Board is used to profile the Aircraft models into either low risk or high risk depending on the number of accidents recorded.

####Objectives

1.Determine the lowest-risk aircraft options for the company's expansion into the aviation industry.
2.Evaluate various Aircraft Models in terms of reliability and market reputation by looking into the number of accidents the model was involved in over time.
3.Use Exploratory Data Analysis(EDA) to clean the dataset, aggregating and formulating visualization tools for a clear presentation.
4.Create an interactive dashboard so that the users can explore the analysis.
5.A summary of the key insights from the Analysis and recommendations for the company.

####Exploratory Data Analysis

In this project, we are going to use analytical skills to determine the lowest risk aircraft options for the company's expansion into the aviation industry. We will use Aviation_Data.csv from the National Transportation Safety Board that includes aviation accident data from 1962 to 2023.
From the csv file used in this project, there are a total of  90348 entries and 32  columns.

####Import the Relevant Libraries and read the data

`python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
df = pd.read_csv('Aviation_Data.csv', dtype={6: 'object', 7: 'object', 28: 'object'})
df.head()
`
####Getting rid of all columns with a threshold of more than 30% missing values.

`python
percent_missing = df.isnull().mean() * 100
missing_value_df = pd.DataFrame({'column_name': df.columns, 'percent_missing': percent_missing})
missing_value_df.sort_values('percent_missing', inplace=True)
print(missing_value_df)
df = df.loc[:, df.isnull().mean() < 0.3]
df.head()
`
####Treat the remaining Null values with mean for all numerical columns and mode for categorical columns

`python
numerical_cols = df.select_dtypes(include=['number']).columns
categorical_cols = df.select_dtypes(exclude=['number']).columns
numerical_means = df[numerical_cols].mean()
categorical_modes = df[categorical_cols].mode().iloc[0]
df[numerical_cols] = df[numerical_cols].fillna(numerical_means)
df[categorical_cols] = df[categorical_cols].fillna(categorical_modes)
`
####Look for duplicates and drop them from the dataset.

`python
duplicates = aviation_data.duplicated().sum()
duplicates
aviation_data= aviation_data.drop_duplicates()
`
####Treat the outliers using the Interquartile Range (IQR).

`python
def treat_outliers(aviation_data, column):
    Q1 = aviation_data[column].quantile(0.25)
    Q3 = aviation_data[column].quantile(0.75)
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR

    median = aviation_data[column].median()
    aviation_data[column] = aviation_data[column].apply(lambda x: median if x < lower_bound or x > upper_bound else x)

for col in aviation_data.select_dtypes(include=['number']).columns:
    treat_outliers(aviation_data, col)
`
#### Graphical Presentations

#Number of Accidents over the Years
A graph showing the number of accidents since 1942.
![Number of Aviation Accidents Over Time](accidents_by_weather_condition.png)
