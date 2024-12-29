# Sales Data Analysis Project

This project involves analyzing sales data using SQL queries and Power BI. The analysis includes customer information, transaction details, and revenue calculations across different markets and time periods.

## Database Setup

1. Download the database dump file:
   ```bash
   db_dump.sql
   ```

2. Import the dump file to your local SQL server

## SQL Queries for Analysis

### Customer Analysis
```sql
-- Show all customer records
SELECT * FROM customers;

-- Get total number of customers
SELECT count(*) FROM customers;
```

### Transaction Analysis
```sql
-- Get transactions from Chennai market
SELECT * FROM transactions where market_code='Mark001';

-- Show distinct products sold in Chennai
SELECT distinct product_code FROM transactions where market_code='Mark001';

-- Show USD transactions
SELECT * from transactions where currency="USD";
```

### Revenue Analysis
```sql
-- Get 2020 transactions with date information
SELECT transactions.*, date.* 
FROM transactions 
INNER JOIN date ON transactions.order_date=date.date 
where date.year=2020;

-- Calculate total revenue for 2020
SELECT SUM(transactions.sales_amount) 
FROM transactions 
INNER JOIN date ON transactions.order_date=date.date 
where date.year=2020 
and (transactions.currency="INR\r" or transactions.currency="USD\r");

-- Calculate January 2020 revenue
SELECT SUM(transactions.sales_amount) 
FROM transactions 
INNER JOIN date ON transactions.order_date=date.date 
where date.year=2020 
and date.month_name="January" 
and (transactions.currency="INR\r" or transactions.currency="USD\r");

-- Calculate 2020 revenue for Chennai
SELECT SUM(transactions.sales_amount) 
FROM transactions 
INNER JOIN date ON transactions.order_date=date.date 
where date.year=2020 
and transactions.market_code="Mark001";
```

## Power BI Analysis

### Data Transformation
The following formula is used to normalize amounts across different currencies:

```powerquery
= Table.AddColumn(
    #"Filtered Rows", 
    "norm_amount", 
    each if [currency] = "USD" or [currency] ="USD#(cr)" 
    then [sales_amount]*75 
    else [sales_amount], 
    type any
)
```

This formula:
- Creates a new column called "norm_amount"
- Converts USD amounts to INR (using 75 as conversion rate)
- Keeps INR amounts unchanged

### Key Features

1. Customer Analysis
   - Total customer count
   - Customer distribution

2. Transaction Analysis
   - Market-specific transactions
   - Currency-wise breakdown
   - Product distribution

3. Revenue Analysis
   - Yearly revenue trends
   - Monthly revenue analysis
   - Market-specific revenue

## Database Schema

The analysis uses three main tables:
- customers: Customer information
- transactions: Sales transaction details
- date: Date dimension table

## Notes

- Market code for Chennai is 'Mark001'
- Currency values need to be cleaned due to '\r' character in data
- USD to INR conversion rate is set at 75

## Requirements

- SQL Server/MySQL
- Power BI Desktop
- Basic SQL knowledge
- Understanding of Power Query M formula language

## Contributing

Feel free to submit issues and enhancement requests.

