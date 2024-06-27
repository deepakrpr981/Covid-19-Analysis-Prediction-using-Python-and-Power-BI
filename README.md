# Project OverView
It has been more than a year since the outbreak of COVID, and the situation keeps up and down. We always want to keep an eye on the numbers and check how things going: are things getting better? when could be the next overseas travelling? when will the quarantine end? To track the trends and get customized visualisations, we can easily do it with Power BI. In this article, I will show you how to connect your Power BI dashboard to the data from GitHub, how to transform and clean the data merge, removing negative values adding additional columns, EDA, ETL using python pandas and Power Query Editor, then finally create dashboard using power Bi
# Dashboard
![Dashboad](https://github.com/deepakrpr981/Covid---19-Dashboard/assets/89341801/c162d461-ba24-43b3-a38b-488fd967d6e7)
# Life Cycle
In this article, i  am going to discuss life cycle phases of data analytics in which we will cover various life cycle phases and will discuss them one by one.
# 1. Understand the Business Issues
   The actions taken during the pandemic – broad lockdowns, restrictions on travel and dining, the widespread use of remote work – coupled with the changing behavior of customers have upended the assumptions underpinning business analytics/models. In response, many companies have been assessing the impact that COVID-driven changes in business conditions have had on their analytics/models and on their data management processes. These efforts have provided a valuable starting point for understanding how analytics/models and data need to be updated.
# 2. Understand Your Data Set
There are a variety of tools you can use to organize your data. When presented with a small dataset, you can use Excel, but for heftier jobs, you’ll likely want to use more rigid tools to explore and prepare your data. Muñoz suggests R, Python, Alteryx, Power BI Prep or Tableau Desktop to help prepare your data for it’s cleaning.

# 3. Data Gathering -
Python script for web scraping COVID-19 data from a webpage. This example uses BeautifulSoup and requests to scrape data from the Worldometer website, which is a common source for COVID-19 statistics.
# # Write the Script: 
import requests
from bs4 import BeautifulSoup
import pandas as pd

# URL of the webpage containing the COVID-19 data
url = 'https://www.worldometers.info/coronavirus/'

# Make an HTTP GET request to the URL
response = requests.get(url)

# Check if the request was successful
if response.status_code == 200:
    # Parse the HTML content of the webpage
    soup = BeautifulSoup(response.content, 'html.parser')

    # Find the table containing the COVID-19 data
    table = soup.find(id='main_table_countries_today')

    # Initialize a list to store the data
    data = []

    # Extract table headers
    headers = []
    for header in table.find_all('th'):
        headers.append(header.text.strip())

    # Extract table rows
    for row in table.find_all('tr'):
        columns = row.find_all('td')
        if columns:
            data.append([col.text.strip() for col in columns])

    # Create a DataFrame
    df = pd.DataFrame(data, columns=headers)

    # Save the DataFrame to a CSV file
    df.to_csv('covid19_data_worldometer.csv', index=False)

    print("Data scraped and saved to covid19_data_worldometer.csv")
else:
    print("Failed to retrieve data")

# Inspect the first few rows of the DataFrame
print(df.head())
