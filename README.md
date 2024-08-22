

# Mumbai Property Price Analysis Project

This project involves analyzing the sales prices of houses in Mumbai by scraping data from the 99acres website. The raw data was cleaned using MS Excel and then analyzed using MySQL to answer specific questions related to the Mumbai housing market.

## Table of Contents

- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Data Cleaning](#data-cleaning)
- [Analysis Questions](#analysis-questions)
- [SQL Queries](#sql-queries)
- [Results](#results)
- [Getting Started](#getting-started)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## Project Overview

The **Mumbai Property Price Analysis Project** is an end-to-end data analysis project aimed at understanding the housing market in Mumbai. The main objectives are:

1. Scrape property price data from 99acres.
2. Clean and preprocess the raw data.
3. Analyze the data using SQL to extract meaningful insights.
4. Answer specific questions regarding property prices in Mumbai.

## Dataset

The dataset was scraped from the 99acres website and includes the following key features:

- Location of the property
- Price of the property
- Area in square feet
- Number of bedrooms
- Other relevant details (e.g., property type, age of the property, etc.)

## Data Cleaning

The raw data obtained from the web scraping process was cleaned using MS Excel. The following steps were taken:

- Removed duplicates
- Handled missing values
- Standardized data formats (e.g., currency, area measurements)
- Filtered out irrelevant data

## Analysis Questions

The following questions were answered using the cleaned dataset:

1. What is the average price of 1 bhk, 2 bhk and 3 bhk apartments in some of the major cities in Mumbai? Arrange the result such that avg price for each type of 
room is shown in separate column
```
select 
 round(avg(price), 2) as 1bhk, 
 (select round(avg(price), 2) from mumbai where location in ('andheri', 'thane', 'goregaon', 'chandivali', 'worli', 'bandra', 'juhu') and bhk = 2) as 2bhk, 
 (select round(avg(price), 2) from mumbai where location in ('andheri', 'thane', 'goregaon', 'chandivali', 'worli', 'bandra', 'juhu') and bhk = 3) as 3bhk 
 from mumbai where location in ('andheri', 'thane', 'goregaon', 'chandivali', 'worli', 'bandra', 'juhu') and bhk = 1;
```
2. I want to buy an apartment which is 2 bhk and within a range of 60 Lac to 1 Cr, display the apartments in Thane where I can find such apartments.
```
select * from mumbai where price between 60 and 100 and bhk = 1 and location = 'thane';
```
3. What size of an apartment can I expect with a price of 60 Lac to 1 Cr in different major suburbs of Mumbai for houses under construction?
```
select * from mumbai where status = 'under construction' and price between 60 and 100 and location in ('andheri', 'thane', 'goregaon', 'chandivali', 'worli', 
'bandra', 'juhu');
```
4. What are the 200 most expensive apartments in major suburbs of Mumbai? Display the ad title along with city, suburb, cost, size.
```
select *, 'Mumbai' as City from mumbai where location in ('andheri', 'thane', 'goregaon', 'chandivali', 'worli', 'bandra', 'juhu') order by price desc limit 200;
```
5. What is the percentage of ready to move & under construction ads on 99 acres?
```
select round(((select count(*) from mumbai where status = 'ready to move')/(select count(*) from mumbai)) * 100 ,2) as 'ready to move %', 
 round(((select count(*) from mumbai where status = 'under construction')/(select count(*) from mumbai)) * 100 ,2) as 'under construction %' 
 from mumbai limit 1;
```
6. What is the avg sale price for apartments with 2bhk and 3bhk area in major suburbs of Mumbai?
```
select round(avg(price), 2) from mumbai where location in ('andheri', 'thane', 'goregaon', 'chandivali', 'worli', 'bandra', 'juhu') and bhk in (2, 3);
```
7. What is the average price for apartments in Mumbai in different suburbs? Categorize the result based on 2 bhk and over 3bhk houses.
```
select location, round(avg(case when bhk = 2 then price else null end), 2) as 2_bhk, round(avg(case when bhk = 3 then price else null end), 2) as 3_bhk from 
mumbai group by location;
```
8. Which are the top 3 most luxurious neighbourhoods in Mumbai? Luxurious neighbourhoods can be defined as suburbs which has the most no of apartments 
costing over 10 Cr in cost.
```
select location, count(location) as count from mumbai where price > 1000 group by location order by count desc limit 3;
```
9. Most small families would be looking for apartment with 1 bhk or 2 bhk in size. Identify the top 5 most affordable neighbourhoods in Mumbai.
```
select location from mumbai where bhk in (1, 2) group by location order by count(location) desc limit 5;
```
10. Which suburb in mumbai has the most and least ready to move houses?
```
select 
 (select location from mumbai where status = 'ready to move' group by location order by count(location) desc limit 1) as max, 
 (select location from mumbai where status = 'ready to move' and location is not null group by location order by count(location) asc limit 1) as min 
 from mumbai limit 1;
```
11. What is the average sale price in some of the major suburbs in Mumbai?
```
select round(avg(price), 2) as average from mumbai where location in ('andheri', 'thane', 'goregaon', 'chandivali', 'worli', 'bandra', 'juhu');
```

## SQL Queries

The analysis was conducted using MySQL. The SQL queries used to answer the questions are available above with the questions.

## Results

The results of the analysis include insights into the Mumbai housing market, such as:

- Areas with the highest and lowest property prices
- Price trends based on property characteristics
- Statistical summaries of the dataset

## Getting Started

### Prerequisites

To run this project, you will need:

- [MySQL](https://dev.mysql.com/downloads/mysql/)
- A spreadsheet application (e.g., MS Excel)
- Python (optional, if you wish to replicate the scraping process)/ Octoparse(for non-coders)

### Installation

1. **Install MySQL**

   ```
   https://dev.mysql.com/downloads/installer/
   ```

2. **Clone the repository:**

   ```bash
   git clone https://github.com/RohitPradhan108/Mumbai_Property_Price_Analysis_Project.git
   ```

3. **Set up the MySQL database:**

   Import the cleaned dataset into your MySQL environment.

   ```sql
   CREATE DATABASE mumbai_property;
   USE mumbai_property;
   ```

4. **Run SQL queries:**

   Execute the provided SQL queries to perform the analysis.

### Usage

- Use the SQL queries to analyze the property data.
- Modify the queries or dataset to explore further insights.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Thanks to Octoparse/99acres for providing the data.
- Special thanks to the open-source community for tools and libraries that made this project possible.

---

You can copy and paste this markdown into your README file, adjusting any specific details as needed.
