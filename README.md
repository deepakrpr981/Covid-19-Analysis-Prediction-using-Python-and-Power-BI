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
```bash
Confirmed = pd.read_csv("covid19_confirmed_global.csv")
Deaths = pd.read_csv("covid19_deaths_global.csv")
Recovered = pd.read_csv("covid19_recovered_global.csv")
```








