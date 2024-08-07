# Covid-19-Analysis & Prediction using Python and Power BI
{In this project, I have collected data from below data source, using API with beauiful soup and performed various task like data Gathering using web scraping ,data cleaning, data modeling, joning, columns transformation, removing neagative values using python, Also designed Dashboards in Power Bi }
<img width="1234" alt="LFTjVAi8kt8O5-wn2eenlkWh7ziD0x7JTg" src="https://github.com/deepakrpr981/Covid---19-Dashboard/assets/89341801/cacb5e14-e6d1-4d91-b256-0e2948dc337f">

It has been more than a year since the outbreak of COVID, and the situation keeps up and down. We always want to keep an eye on the numbers and check how things going: are things getting better? when could be the next overseas travelling? when will the quarantine end? To track the trends and get customized visualisations, we can easily do it with Power BI. In this article, I will show you how to connect your Power BI dashboard to the data from GitHub, how to transform and clean the data merge, removing negative values adding additional columns, EDA, ETL using python pandas and Power Query Editor, then finally create dashboard using power Bi
# Life Cycle
In this article, i  am going to discuss life cycle phases of data analytics in which we will cover various life cycle phases and will discuss them one by one.
# 1. Understand the Business Issues
   The actions taken during the pandemic – broad lockdowns, restrictions on travel and dining, the widespread use of remote work – coupled with the changing behavior of customers have upended the assumptions underpinning business analytics/models. In response, many companies have been assessing the impact that COVID-driven changes in business conditions have had on their analytics/models and on their data management processes. These efforts have provided a valuable starting point for understanding how analytics/models and data need to be updated.
# 2. Understand  Data Set
There are a variety of tools you can use to organize your data. When presented with a small dataset, you can use Excel, but for heftier jobs, you’ll likely want to use more rigid tools to explore and prepare your data. Muñoz suggests R, Python, Alteryx, Power BI Prep or Tableau Desktop to help prepare your data for it’s cleaning.

# 3. Data Gathering -
Python script for web scraping COVID-19 data from a webpage. This example uses BeautifulSoup and requests to scrape data from the Worldometer website, which is a common source for COVID-19 statistics.
# # Write the Script: 

```bash
import requests
from bs4 import BeautifulSoup
import pandas as pd
url = 'https://www.worldometers.info/coronavirus/'
response = requests.get(url)
if response.status_code == 200:
  
    soup = BeautifulSoup(response.content, 'html.parser')
    table = soup.find(id='main_table_countries_today')
    data = []   
    headers = []
    for header in table.find_all('th'):
        headers.append(header.text.strip())
    for row in table.find_all('tr'):
        columns = row.find_all('td')
        if columns:
            data.append([col.text.strip() for col in columns])
    df = pd.DataFrame(data, columns=headers)
    df.to_csv('covid19_data_worldometer.csv', index=False)
    print("Data scraped and saved to covid19_data_worldometer.csv")
else:
    print("Failed to retrieve data")
print(df.head())
```

---

# Library
ach of Python's open-source libraries has its own source code. A collection of code scripts that can be used iteratively to save time is known as a library. It is like a physical library in that it has resources that can be used again, as the name suggests.

Python OS module provides the facility to establish the interaction between the user and the operating system. It offers many useful OS functions that are used to perform OS-based tasks and get related information about operating system
```bash
import os 
import numpy as np 
import pandas as pd
from matplotlib import pyplot as plt
import seaborn as sns
from math import sqrt
from datetime import datetime
%matplotlib inline
```
---
# Data Collection
Before we define what is data collection, it’s essential to ask the question, “What is data?” The abridged answer is, data is various kinds of information formatted in a particular way. Therefore, data collection is the process of gathering, measuring, and analyzing accurate data from a variety of relevant sources to find answers to research problems, answer questions, evaluate outcomes, and forecast trends and probabilities. Accurate data collection is necessary to make informed business decisions, ensure quality assurance, and keep research integrity.
```bash
Confirmed = pd.read_csv("covid19_confirmed_global.csv")
Deaths = pd.read_csv("covid19_deaths_global.csv")
Recovered = pd.read_csv("covid19_recovered_global.csv")
```
---
# check the shap of table 
```bash
print("The Shape of confirmed is: ", Confirmed.shape).head
print("The Shape of deaths is: ", Deaths.shape)
print("The Shape of recovered is: ", Recovered.shape)
```
---
# Unpivot 
Most developers are familiar with pivot tables. With the use of sums or averages, for instance, a pivot table may rapidly simplify fundamental statistical data and present it in a more comprehensible way. Before you can begin pivoting your data, though, you might need to reorder it, or in other words, “unpivot” it.

The relational operations Pivot and Unpivot in SQL or Excel are used to change one table into another in order to create a table with a more straightforward perspective. Traditionally, we may say that the pivot operator transforms the table’s row data into its column data. The Unpivot operator performs the reverse operation, turning column-based data into rows.
```bash
Confirmed = pd.melt(Confirmed, id_vars=['Province/State', 'Country/Region', 'Lat', 'Long'], 
                              var_name=['Date'])
Deaths = pd.melt(Deaths, id_vars=['Province/State', 'Country/Region', 'Lat', 'Long'], 
                           var_name=['Date'])
Recovered = pd.melt(Recovered, id_vars=['Province/State', 'Country/Region', 'Lat', 'Long'], 
                              var_name=['Date'])


print("The Shape of Confirmed is: ", Confirmed.shape)
print("The Shape of deaths is: ", Deaths.shape)
print("The Shape of recovered is: ", Recovered.shape)
```

---
# Convert the Column Type from String to Datetime Format in Pandas DataFrame
When we work with data in Pandas DataFrame of Python, it is pretty usual to encounter time series data. Panday is a strong tool that can handle time-series data in Python, and we might need to convert the string into Datetime format in the given dataset.

In this tutorial, we will learn how to convert the DataFrame column of string into datetime format, "dd/mm/yy". The user cannot execute any time-series based operations on the dates if they are not in the required format. To deal with this, we need to convert the dates into the required date-time format

```bash
Confirmed.columns = Confirmed.columns.str.replace('value', 'Confirmed')
Deaths.columns = Deaths.columns.str.replace('value', 'Deaths')
Recovered.columns = Recovered.columns.str.replace('value', 'Recovered')
```
---
# JOIN
A JOIN clause is used to combine rows from two or more tables, based on a related column between them. It is used to merge two tables into in new table or retrieve data from there. There are 4 types of joins, as you can refer to below:

1. If you want to access more than one table through a select statement.
2. If you want to combine two or more table then SQL JOIN statement is used .it combines rows of that tables in one table and one can retrieve the information by a SELECT statement.
3. The joining of two or more tables is based on common field between them.
4. SQL INNER JOIN also known as simple join is the most common type of join.
```bash
full_join = Confirmed.merge(Deaths[['Province/State','Country/Region','Date','Deaths']], 
                                      how = 'left', 
                                      left_on = ['Province/State','Country/Region','Date'], 
                                      right_on = ['Province/State', 'Country/Region','Date'])
full_join.head()
```
```bash
full_join = full_join.merge(Recovered[['Province/State','Country/Region','Date','Recovered']], 
                                      how = 'left', 
                                      left_on = ['Province/State','Country/Region','Date'], 
                                      right_on = ['Province/State', 'Country/Region','Date'])
full_join.head()
```
---
# Adding Month and Year as a new Column
```bash
full_join['Month-Year'] = full_join['Date'].dt.strftime('%b-%Y')
full_join.head()
```
--
```bash
full_join2 = full_join.copy()

#creating a new date columns - 1
full_join2['Date - 1'] = full_join2['Date'] + pd.Timedelta(days=1)
full_join2.rename(columns={'Confirmed': 'Confirmed - 1', 'Deaths': 'Deaths - 1', 'Recovered': 'Recovered - 1',
                          'Date': 'Date Minus 1'}, inplace=True)
full_join2.head()
```
---
```bash
full_join3 = full_join.merge(full_join2[['Province/State', 'Country/Region','Confirmed - 1', 'Deaths - 1', 
                            'Recovered - 1', 'Date - 1', 'Date Minus 1']], how = 'left',
                             left_on = ['Province/State','Country/Region','Date'], 
                             right_on = ['Province/State', 'Country/Region','Date - 1'])
full_join3.head()
```
---
```bash
full_join3['Confirmed Daily'] = full_join3['Confirmed'] - full_join3['Confirmed - 1']
full_join3['Deaths Daily'] = full_join3['Deaths'] - full_join3['Deaths - 1']
full_join3['Recovered Daily'] = full_join3['Recovered'] - full_join3['Recovered - 1']
print(full_join3.shape)
```
---
# Braking the numbers by Day
```bash
#creating a new df    
full_join2 = full_join.copy()

#creating a new date columns - 1
full_join2['Date - 1'] = full_join2['Date'] + pd.Timedelta(days=1)
full_join2.rename(columns={'Confirmed': 'Confirmed - 1', 'Deaths': 'Deaths - 1', 'Recovered': 'Recovered - 1',
                          'Date': 'Date Minus 1'}, inplace=True)

#Joing on the 2 DFs
full_join3 = full_join.merge(full_join2[['Province/State', 'Country/Region','Confirmed - 1', 'Deaths - 1', 
                            'Recovered - 1', 'Date - 1', 'Date Minus 1']], how = 'left',
                             left_on = ['Province/State','Country/Region','Date'], 
                             right_on = ['Province/State', 'Country/Region','Date - 1'])

#minus_onedf.rename(columns={'Confirmed': 'Confirmed - 1', 'Deaths': 'Deaths - 1', 'Recovered': 'Recovered - 1'}, inplace=True)

full_join3.head()

# Additional Calculations
full_join3['Confirmed Daily'] = full_join3['Confirmed'] - full_join3['Confirmed - 1']
full_join3['Deaths Daily'] = full_join3['Deaths'] - full_join3['Deaths - 1']
full_join3['Recovered Daily'] = full_join3['Recovered'] - full_join3['Recovered - 1']

print(full_join3.shape)
```
---
# Additing manually the numbers for first day
```bash

full_join3['Confirmed Daily'].loc[full_join3['Date'] == '2020-01-22'] = full_join3['Confirmed']
full_join3['Deaths Daily'].loc[full_join3['Date'] == '2020-01-22'] = full_join3['Deaths']
full_join3['Recovered Daily'].loc[full_join3['Date'] == '2020-01-22'] = full_join3['Recovered']

# deleting columns
del full_join3['Confirmed - 1']
del full_join3['Deaths - 1']
del full_join3['Recovered - 1']
del full_join3['Date - 1']
del full_join3['Date Minus 1']
ull_join3[full_join3["Deaths Daily"]<0]
```
---

```bash
full_join3['Deaths Daily']=np.where(full_join3['Deaths Daily']<0 ,0,full_join3['Deaths Daily'])
full_join3['Confirmed Daily']=np.where(full_join3['Confirmed Daily']<0 ,0,full_join3['Confirmed Daily'])
full_join3['Recovered Daily']=np.where(full_join3['Recovered Daily']<0 ,0,full_join3['Recovered Daily'])
```
---
# Data Cleaning Cycle
It is the method of analyzing, distinguishing, and correcting untidy, raw data. Data cleaning involves filling in missing values, handling outliers, and distinguishing and fixing errors present in the dataset. Whereas the techniques used for data cleaning might vary in step with different types of datasets. In this tutorial, we will learn how to clean data using pandas. The following are standard steps to map out data cleaning:

```bash
#get the all columns name 
full_join3.keys()
#check the data types  of each columns 
full_join3.dtypes
#count the all missing values of particular columns 
full_join3.isnull().sum()
#identify the missing values with heatmap
sns.heatmap(full_join3.isnull())
#get those columns name which have missing values 
[col for col in full_join3.columns if full_join3[col].isnull().sum()>0 ]

#fill the numerical  columns which had contain null values 
full_join3['Lat'] = full_join3['Lat'].fillna(full_join3['Lat'].mean())
full_join3['Long'] = full_join3['Long'].fillna(full_join3['Long'].mean())
full_join3['Recovered'] = full_join3['Recovered'].fillna(full_join3['Recovered'].median())
full_join3['Recovered Daily'] = full_join3['Recovered Daily'].fillna(full_join3['Recovered Daily'].median())

fill the categorical columns which had contain null values 
full_join3['Province/State'] = full_join3['Province/State'].fillna(full_join3['Province/State'].mode()[0])

full_join3.isnull().sum()
```

# Data Export?
Data export is the extraction and conversion of raw data from their existing format into a format required by another application. Exporting data is also a way of backing up data or moving it between two different versions of programs. This process usually contains information such as user ID, user app key, individual ID, IP address, and custom user variables (custom var name).

Exporting data can prove as a part of a backup strategy. This helps in extracting data and storing it separately. Data export saves a huge chunk of costs incurred in organizing and tracking data. An export command can be usually found in the “file” menu. This command is used to extract specific data like photos and videos. Text files do not contain any specific data to be exported.
```sql
path = "C:\\Users\\DELL\\Desktop\\"

# Changing my CWD
os.chdir(path)

full_join3.to_csv('CoronaVirus Data.csv')
```
# Dasgboard Using PowerBI
<img width="614" alt="covvid" src="https://github.com/user-attachments/assets/5f51bd5c-9e1d-4299-88df-80d0f7790ba1">
