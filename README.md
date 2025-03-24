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
# Import necessary libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import yfinance as yf

# Fetch Tesla stock data
tesla = yf.Ticker("TSLA")
tesla_df = tesla.history(period="5y")

# Plot Tesla stock price
plt.figure(figsize=(12,6))
plt.plot(tesla_df.index, tesla_df["Close"], label="Tesla Closing Price", color='blue')
plt.xlabel("Date")
plt.ylabel("Closing Price (USD)")
plt.title("Tesla Stock Price Over Time")
plt.legend()
plt.show()

