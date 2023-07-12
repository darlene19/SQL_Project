Final-Project-Transforming-and-Analyzing-Data-with-SQL

This project aims to apply existing knowledge and understanding we have learned on using PostgreSQL, and find meaningful patterns and gain insights from the analyzed data. An ecommerce dataset is provided that includes information about an online retailer - the website’s traffic, products available, products sold, revenue, and more. 

Project/Goals

1. Create an ecommerce database in PostgreSQL.
2. Create five tables and download and import csv file to PostgreSQL.
3. Address potential issues by cleaning and transforming data.
4. Analyze data by answering questions on what the data can tell us and provide insights based from these answers.
5. Identify the risk areas by developing a QA process.

Process
1. Create dataset in PostgreSQL named ‘ecommerce’.
2. Create tables in the dataset.

CREATE TABLE all_sessions (
    fullVisitorId text,
    channelGrouping text,
    time integer,
    country text,
    city text,
    totalTransactionRevenue float,
    transactions integer,
    timeOnSite integer,
    pageviews integer,
    sessionQualityDim integer,
    date date,
    visitId bigint,
    type text,
    productRefundAmount float,
    productQuantity integer,
    productPrice integer,
    productRevenue float,
    productSKU text,
    v2ProductName text,
    v2ProductCategory text,
    productVariant text,
    currencyCode text,
    itemQuantity integer,
    itemRevenue float,
    transactionRevenue float,
    transactionId varchar,
    pageTitle text,
    searchKeyword text,
    pagePathLevel1 text,
    eCommerceAction_type integer,
    eCommerceAction_step integer,
    eCommerceAction_option varchar
);

CREATE TABLE analytics (
    visitNumber integer,
    visitId bigint,
    visitStartTime integer,
    date date,
    fullvisitorId text,
    userid integer,
    channelGrouping text,
    socialEngagementType text,
    units_sold integer,
    pageviews integer,
    timeonsite integer,
    bounces integer,
    revenue float,
    unit_price integer
);

CREATE TABLE products (
    SKU text,
    name text,
    orderedQuantity integer,
    stockLevel integer,
    restockingLeadTime integer,
    sentimentScore float,
    sentimentMagnitude float,

    PRIMARY KEY(SKU),
    UNIQUE(SKU)
);

CREATE TABLE sales_by_sku (
    productSKU text,
    total_ordered integer
);

CREATE TABLE sales_report (
    productSKU text,
    total_ordered integer,
    name text,
    stockLevel integer,
    restockingLeadTime integer,
    sentimentScore float,
    sentimentMagnitude float,
    ratio float
)
 
3. Import csv files by following the steps on the link below:

https://youtu.be/6Jf7eTkIaR4

4. Data cleaning, transformation and analysis

See file cleaning_data.md.

5. Answering the questions provided, and provide more questions that needs to be addressed.

See files starting_with_questions.md and starting_with_data.md.

6. Develop a QA process by identifying and describing the risk areas.

See file QA.md.

Results



Challenges

1. Importing the csv files resulted in some errors, mainly because the created columns in each table have different data type than what is in the csv files.  This can resolve by taking time to analyze the raw data and determine what is the best data type that describes the values in the columns. Knowledge in different data types will help a lot too.

2. There were a lot of NULL values in most of the columns. Deciding on whether or not to include them depends on knowing the objective of the project.

Future Goals

1. Since all_sessions is such a big table with 27 columns, I plan on creating 2 tables from it. This will also minimize the query time in pgAdmin, and potential crashing of the program.

2. I will analyze more on the values in each column and how they were directly related to each other. For example, in analytics table, the revenue doesn’t exactly match the value of units_sold * unit_price. It is possible that some products has discounts subtracted from them or tax, customs or other fees were added.











