# Hands-on Lab Project: Extracting and Visualizing Stock Data

## Table of Content
- [Project Overview](#project-overview)
- [Prerequisites](#prerequisites)
- [Tools Used](#tools-used)
- [Data Cleaning & Preparation](#data-cleaning--preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Mining & Predictive Modeling](#data-mining--predictive-modeling)
- [Recommendations](#recommendations)
- [Limitations](#limitations)

### Project Overview
In this project, we will take on the role of a Data Scientist or Data Analyst working for a newly established investment firm that assists clients in making informed stock investments. Our primary objective is to extract financial data—such as historical stock prices and annual revenue reports—using Python libraries and web scraping techniques. We will then analyze and visualize this data in a dashboard to identify patterns and trends, focusing on Tesla as our case study.

### Prerequisites
Since this project involves working with Python, it is essential to have the necessary libraries installed for extracting and processing stock data. The specific libraries required will depend on the development environment used to run the code. Ensure that you have installed all relevant dependencies before proceeding.

```python
#!pip install pandas==1.3.3
#!pip install requests==2.26.0
#!pip install plotly==5.3.1
!pip install yfinance==0.1.67
!mamba install bs4==4.10.0 -y
!mamba install html5lib==1.1 -y
!pip install lxml==4.6.4
!pip install nbformat>=4.2.0


import yfinance as yf
import pandas as pd
import requests
from bs4 import BeautifulSoup
import plotly.graph_objects as go
from plotly.subplots import make_subplots
from IPython.display import display

```
In addition to the required Python libraries, we will use a custom function to visualize the stock data. While understanding the inner workings of the function is not necessary, it is important to ensure that the correct inputs are provided.

The function requires:
- A **data frame** containing historical stock price data, which must include the columns `Date` and `Close`.
- A **data frame** containing historical revenue data, which must include the columns `Date` and `Revenue`.
- The **name of the stock** as a string.

The function is defined as follows:

```python
def make_graph(stock_data, revenue_data, stock):
    fig = make_subplots(rows=2, cols=1, shared_xaxes=True, subplot_titles=("Historical Share Price", "Historical Revenue"), vertical_spacing = .3)
    stock_data_specific = stock_data[stock_data.Date <= '2021--06-14']
    revenue_data_specific = revenue_data[revenue_data.Date <= '2021-04-30']
    fig.add_trace(go.Scatter(x=pd.to_datetime(stock_data_specific.Date, infer_datetime_format=True), y=stock_data_specific.Close.astype("float"), name="Share Price"), row=1, col=1)
    fig.add_trace(go.Scatter(x=pd.to_datetime(revenue_data_specific.Date, infer_datetime_format=True), y=revenue_data_specific.Revenue.astype("float"), name="Revenue"), row=2, col=1)
    fig.update_xaxes(title_text="Date", row=1, col=1)
    fig.update_xaxes(title_text="Date", row=2, col=1)
    fig.update_yaxes(title_text="Price ($US)", row=1, col=1)
    fig.update_yaxes(title_text="Revenue ($US Millions)", row=2, col=1)
    fig.update_layout(showlegend=False,
    height=900,
    title=stock,
    xaxis_rangeslider_visible=True)
    fig.show()

```

### Step 1: 
#### Use ‘yfinance’ library to Extract Stock Data

To extract stock data for Tesla (ticker symbol: **TSLA**), we will use the `Ticker` function to create a ticker object:  

```python
tesla = yf.Ticker("TSLA")
```

Using the `Ticker` object, we can extract Tesla's historical stock data with the `history` function. The `period="max"` parameter ensures that we retrieve the maximum available historical data.

```python
# Extract historical stock data for Tesla
tesla_data = tesla.history(period="max")
```

To properly structure the extracted data, we reset the index of the `tesla_data` DataFrame and display the first five rows. This ensures that the **Date** column is properly formatted and can be used for further analysis.

```python
# Reset the index to structure the DataFrame properly
tesla_data.reset_index(inplace=True)

# Display the first five rows of the dataset
tesla_data.head()
```
![tab1](https://github.com/user-attachments/assets/1f929d17-e9d6-4a72-89e7-f776caade77b)


