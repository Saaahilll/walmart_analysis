# Walmart Data Analysis: End-to-End SQL + Python Project 

# Walmart Sales Data Analysis Project

## Overview
This project is an end-to-end data analysis solution designed to extract critical business insights from Walmart sales data. It leverages Python for data processing and analysis, SQL for advanced querying, and structured problem-solving techniques to address key business questions.

## Project Goals
*   To explore and understand Walmart sales data.
*   To identify key factors influencing sales performance.
*   To provide actionable insights for business decision-making.

## Key Features
*   **Data Manipulation**: Cleaning, transforming, and preparing raw data for analysis.
*   **Data Analysis**: Identifying trends, patterns, and anomalies using Python libraries.
*   **SQL Querying**: Performing advanced data retrieval and analysis using SQL.
*   **Data Pipeline Creation**: Building a structured data pipeline for efficient data processing.

## Project Structure
The project repository contains the following files:

*   `Walmart.csv`: The raw sales data in CSV format.
*   `walmart_analysis.ipynb`: A Jupyter Notebook containing Python code for data processing and analysis.
*   `walmart_dataset.sql`: A SQL file containing queries for data analysis.
*   `README.md`: The project overview and setup instructions.

## Technologies Used
*   Python (with libraries such as Pandas, NumPy, Matplotlib, Seaborn)
*   SQL (PostgreSQL or MySQL)
*   Jupyter Notebook

## Setup Instructions

### Prerequisites
*   Python 3.x
*   Jupyter Notebook
*   SQL Database (PostgreSQL recommended)

### Installation
1.  Clone the repository:
    ```
    git clone [repository URL]
    cd [repository name]
    ```

2.  Install the required Python packages:
    ```
    pip install pandas numpy matplotlib seaborn
    ```

3.  Set up the SQL database:
    *   Create a new database named `walmart`.
    *   Import the data from `Walmart.csv` into a table named `walmart`.
    *   Alternatively, use the provided SQL script (`walmart_dataset.sql`) to create the table and insert the data.

## Usage
1.  Open the Jupyter Notebook (`walmart_analysis.ipynb`) and execute the cells to perform data processing and analysis.
2.  Connect to the SQL database using your preferred SQL client and execute the queries in `walmart_dataset.sql` to perform advanced data retrieval and analysis.

## Data Analysis Questions

The SQL queries address the following business questions:

*   Find different payment methods and the number of transactions for each.
*   Identify the highest-rated category in each branch.
*   Identify the busiest day for each branch based on the number of transactions.
*   Calculate the total quantity of items sold per payment method.
*   Determine the average, minimum, and maximum rating of categories for each city.
*   Calculate the total profit for each category.
*   Determine the most common payment method for each branch.
*   Categorize sales into morning, afternoon, and evening groups and find the number of invoices for each shift.
*   Identify the top 5 branches with the highest decrease ratio in revenue compared to the last year.

## Contributing
Contributions to this project are welcome. Fork the repository, create a new branch, and submit a pull request with your changes.

## Acknowledgments
Data Source: Kaggle’s Walmart Sales Dataset
Inspiration: Walmart’s business case studies on sales and supply chain optimization.
