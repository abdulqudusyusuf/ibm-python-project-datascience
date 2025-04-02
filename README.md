# Extracting and Visualizing Stock Data

## Table of Content
- [Project Overview](#project-overview)
- [Prerequisites](#prerequisites)
- [Step 1](#step-1)
- [Step 2](#step-2)
- [Step 3](#step-3)

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

### Step 2: 
#### Use Web Scraping to Extract Tesla Revenue Data

Use the requests library to download the webpage "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/revenue.htm"

Save the text of the response as a variable named `html_data`.

```python
url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/revenue.htm"

# Send an HTTP request and save the response text
html_data = requests.get(url).text
```

Parse the HTML data using `beautiful_soup`.

```python
soup = BeautifulSoup(html_data, 'html5lib')
```

### Extracting Tesla Revenue Data Using `BeautifulSoup`  

To extract Tesla's revenue data, we will use `BeautifulSoup` to parse the HTML content and locate the relevant table. The extracted data will be stored in a DataFrame named `tesla_revenue`, with two columns: **Date** and **Revenue**.  

```python
#We create an empty dataframe
tesla_revenue = pd.DataFrame(columns=["Date", "Revenue"])

#We extract the desired data from the 'soup' object and save it in the dataframe
for row in soup.find("tbody").find_all('tr'):
    col = row.find_all("td")
    date = col[0].text
    revenue = col[1].text
    tesla_revenue = tesla_revenue.append({"Date":date, "Revenue":revenue}, ignore_index=True)
```

Once the data is extracted, we will clean the **Revenue** column by removing commas and dollar signs to ensure proper formatting. Finally, we will display the last 5 rows of the `tesla_revenue` DataFrame to verify the data.

```python
tesla_revenue["Revenue"] = tesla_revenue['Revenue'].str.replace(',|\$',"")

#OPTIONAL: Execute the following lines to remove an null or empty strings in the Revenue column.
#tesla_revenue.dropna(inplace=True)
#tesla_revenue = tesla_revenue[tesla_revenue['Revenue'] != ""]

tesla_revenue.tail()
```

![tab2](https://github.com/user-attachments/assets/8206a872-a159-4a5b-84a9-5bd03c504048)

### Step 3: 
#### Plot Tesla Stock Graph

### Visualizing Tesla Stock Data  

To analyze Tesla’s stock performance alongside its revenue trends, we will use the `make_graph` function defined in the prerequisites section. This function takes three parameters:  

- `tesla_data`: The DataFrame containing Tesla's historical stock prices.  
- `tesla_revenue`: The DataFrame containing Tesla's revenue data.  
- `'Tesla'`: The stock name as a string.  

By calling the function with these inputs, we generate a visualization that highlights Tesla's stock price movements and revenue trends over time.

```python
make_graph(tesla_data, tesla_revenue, 'Tesla')
```
![tab3](https://github.com/user-attachments/assets/dd74a8cb-4f93-48f9-aa2f-bca96ecf90de)

 
This project is *originally based on the* **IBM Python Project for Data Science** *course on Coursera.* *For more details, visit:*  
[*IBM Python Project for Data Science*](https://www.coursera.org/learn/python-project-for-data-science)  
