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



```python
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


