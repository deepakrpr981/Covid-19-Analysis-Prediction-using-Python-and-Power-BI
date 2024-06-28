
{In this project, I have collected data from below data source, using API with beauiful soup and performed various task like data Gathering using web scraping ,data cleaning, data modeling, joning, columns transformation, removing neagative values using python, Also designed Dashboards in Power Bi }

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
url = 'https://www.worldometers.info/coronavirus/'
response = requests.get(url)
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
<img width="581" alt="1" src="https://github.com/deepakrpr981/Covid---19-Dashboard/assets/89341801/78bcaad3-c6e9-42fe-9541-f6f26fbf695c">

# Library
Each of Python's open-source libraries has its own source code. A collection of code scripts that can be used iteratively to save time is known as a library. It is like a physical library in that it has resources that can be used again, as the name suggests.

* Python OS module provides the facility to establish the interaction between the user and the operating system. It offers many useful OS functions that are used to perform OS-based tasks and get related information about operating system.

* NumPy One of the most popular open-source Python packages, NumPy focuses on scientific and mathematical computation. It makes it easy to work with large matrices and multidimensional data thanks to built-in mathematical functions that make it easy to compute.
* Pandas is an open-source library authorized under the Berkeley Programming Conveyance (BSD). This well-known library is frequently utilized in the field of data science. They're generally utilized for examination, control, and cleaning of information, in addition to other things.

* Matplotlib Human minds are more adaptive for the visual representation of data rather than textual data. We can easily understand things when they are visualized. It is better to represent the data through the graph where we can analyze the data more efficiently and make the specific decision according to data analysis. Before learning the matplotlib, we need to understand data visualization and why data visualization is important.

* Seaborn This package makes statistical model visualization possible. The library, which is largely based on Matplotlib, makes statistical graphics possible by:
* Math Python has a built-in math module. It is a standard module, so we don't need to install it separately. We only have to import it into the program we want to use. We can import the module, like any other module of Python, using import math to implement the functions to perform mathematical operations.

* Python DateTime module is provided in Python to work with dates and times. In python, DateTime is an inbuilt module rather than being a primitive data type, We just have to import the module mentioned above to work with dates as date object.
  <img width="553" alt="2" src="https://github.com/deepakrpr981/Covid---19-Dashboard/assets/89341801/d945ae10-becb-4989-9f7b-969ea33b85c3">

Most developers are familiar with pivot tables. With the use of sums or averages, for instance, a pivot table may rapidly simplify fundamental statistical data and present it in a more comprehensible way. Before you can begin pivoting your data, though, you might need to reorder it, or in other words, “unpivot” it.
# Data Collection
Before we define what is data collection, it’s essential to ask the question, “What is data?” The abridged answer is, data is various kinds of information formatted in a particular way. Therefore, data collection is the process of gathering, measuring, and analyzing accurate data from a variety of relevant sources to find answers to research problems, answer questions, evaluate outcomes, and forecast trends and probabilities. Accurate data collection is necessary to make informed business decisions, ensure quality assurance, and keep research integrity.
<img width="376" alt="3" src="https://github.com/deepakrpr981/Covid---19-Dashboard/assets/89341801/005d03ec-b1fa-4e3a-91a1-9d0a74d52a1e">
# Unpivot Data
Most developers are familiar with pivot tables. With the use of sums or averages, for instance, a pivot table may rapidly simplify fundamental statistical data and present it in a more comprehensible way. Before you can begin pivoting your data, though, you might need to reorder it, or in other words, “unpivot” it.


The relational operations Pivot and Unpivot in SQL or Excel are used to change one table into another in order to create a table with a more straightforward perspective. Traditionally, we may say that the pivot operator transforms the table’s row data into its column data. The Unpivot operator performs the reverse operation, turning column-based data into rows.

Examples of When to Use Unpivot

Let’s consider an example of when unpivot can be helpful. Think of a financial spreadsheet that any typical business would have, either exported from a financial tool or built manually.

Each month in this spreadsheet is represented by a column, such as January, February, March, April, etc. While it’s pretty simple to drag a range of fields and generate a chart, pulling something like this into a database for dynamic report building is a lot more difficult. That’s because each month is in a separate field or column in the database.
<img width="407" alt="4" src="https://github.com/deepakrpr981/Covid---19-Dashboard/assets/89341801/7f635ce3-a3e9-4138-a656-2c224519b4a9">
# Convert the Column Type from String to Datetime Format in Pandas DataFrame.

When we work with data in Pandas DataFrame of Python, it is pretty usual to encounter time series data. Panday is a strong tool that can handle time-series data in Python, and we might need to convert the string into Datetime format in the given dataset.

In this tutorial, we will learn how to convert the DataFrame column of string into datetime format, "dd/mm/yy". The user cannot execute any time-series based operations on the dates if they are not in the required format. To deal with this, we need to convert the dates into the required date-time format.
<img width="407" alt="5" src="https://github.com/deepakrpr981/Covid---19-Dashboard/assets/89341801/6b63f90d-8e7b-46ea-877f-529682a9d0d5">
# Joins
A JOIN clause is used to combine rows from two or more tables, based on a related column between them. It is used to merge two tables into in new table  or retrieve data from there. There are 4 types of joins, as you can refer to below:

1. If you want to access more than one table through a select statement.
2. If you want to combine two or more table then SQL JOIN statement is used .it combines rows of that tables in one table and one can retrieve the information by a SELECT statement.
3. The joining of two or more tables is based on common field between them.
4. SQL INNER JOIN also known as simple join is the most common type of join.
<img width="428" alt="6" src="https://github.com/deepakrpr981/Covid---19-Dashboard/assets/89341801/23f6f38d-a568-47d2-a68a-7ff3fe79290e">

