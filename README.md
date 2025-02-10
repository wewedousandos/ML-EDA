# ML-EDA
Column Name	Data Type	Description
user_id	Integer	Unique identifier for each user.
age	Integer	Age of the user (ranging from 18 to 25).
gender	Categorical	Gender of the user (e.g., Male, Female, Non-binary).
location	Categorical	User’s geographical location.
sign_up_date	DateTime	The date the user registered on the platform.
last_active	DateTime	The last recorded activity timestamp of the user.
subscription	Categorical	Type of subscription plan (e.g., Free, Premium).
messages_sent	Integer	Total number of messages sent by the user.
matches_count	Integer	Total number of matches made by the user.
profile_complete	Float	Percentage of profile completion (0 to 100).
Data Cleaning Steps

The following cleaning steps were applied to ensure data quality and integrity:
	1.	Checked for Duplicate Rows
	•	Removed duplicate entries using data.drop_duplicates(inplace=True).
	2.	Standardized Categorical Data
	•	Converted all categorical values to lowercase and stripped extra spaces for consistency.
 categorical_cols = ['gender', 'subscription', 'location']
data[categorical_cols] = data[categorical_cols].apply(lambda x: x.str.lower().str.strip())
	3.	Handled Missing Values
	•	Dropped columns with more than 50% missing values.
	•	Filled missing categorical values with the mode and numerical values with the median.
 for col in data.select_dtypes(include='object').columns:
    data[col].fillna(data[col].mode()[0], inplace=True)
for col in data.select_dtypes(include='number').columns:
    data[col].fillna(data[col].median(), inplace=True)
    	4.	Detected and Handled Outliers
	•	Used box plots and the Interquartile Range (IQR) method to detect outliers.
	•	Capped extreme outlier values within acceptable thresholds.
 Q1 = data.quantile(0.25)
Q3 = data.quantile(0.75)
IQR = Q3 - Q1
data = data.clip(lower=(Q1 - 1.5 * IQR), upper=(Q3 + 1.5 * IQR), axis=1)
	5.	Tracked Dataset Changes
	•	Implemented a dataset hash check to monitor changes over time:
 import hashlib
def get_data_hash(df):
    return hashlib.md5(pd.util.hash_pandas_object(df, index=True).values).hexdigest()
    How to Use This Dataset
	1.	Clone the Repository:
 git clone https://github.com/your-repo/genz-dating-app-data.git
cd genz-dating-app-data
	2.	Load the Dataset in Python:
 import pandas as pd

# Load the dataset
data = pd.read_csv("GenZ_DatingApp_Data.csv")

# Convert date columns to datetime
data['sign_up_date'] = pd.to_datetime(data['sign_up_date'])
data['last_active'] = pd.to_datetime(data['last_active'])
	3.	Explore the Dataset:
 # Display the first few rows
print(data.head())

# Get a summary of the dataset
print(data.info())
print(data.describe(include='all'))
