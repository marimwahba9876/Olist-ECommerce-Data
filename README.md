# Brazilian E-Commerce: Data Exploration & Cleaning Pipeline
A structured and professional data pipeline focused on exploring, understanding, and cleaning the Olist Brazilian E-Commerce Dataset. This project handles the essential first step of data analytics: transforming raw, disconnected tables into clean, reliable data ready for accurate analysis.

## 📌 Project Overview
In the real world, e-commerce data is always messy, full of missing values, and split across many tables. This project serves as the foundation. It takes 9 different datasets from Olist and runs careful programmatic checks. By analyzing table relationships and using smart data cleaning choices, the data is prepared for any future modeling or analysis without losing important sales records.

## 🏗️ Understanding the Data Structure (Schema Audit)
Before changing any data, I analyzed the structure to understand how the tables connect with each other, dividing them into a clear structure:

### 🌟 Fact Tables (The Core Transactions)
order_items: The main sales table that stores core metrics like price and freight_value, connecting products to sellers.

order_payments: Tracks how customers paid, including payment types and installment choices.

order_reviews: Contains customer satisfaction scores and review comments.

### 🧩 Dimension Tables (The Context Details)
orders: The bridge table that tracks order statuses (like shipped or canceled) and important timestamps.

customers: Maps customer details using unique tracking IDs.

products: Stores product details like categories and physical dimensions.

sellers: Contains information about the registered merchants.

geolocation & products_category: Reference tables used for looking up locations and English translations.

## 🛠️ Step-by-Step Data Pipeline
The workflow is split into two clear phases: Data Exploration and Data Cleaning.

### Phase 1: Deep Data Exploration
Instead of just looking at the surface, I performed an architectural health check on every single table:

Checking Data Shapes: Used .shape and .info() to check rows, columns, and data types across all tables.

Verifying Key Constraints: Checked the uniqueness of IDs (for example, making sure order_id acts as a unique Primary Key in the orders table).

Analyzing Missing Values: Calculated the exact percentage of null values in each column to understand why they occur (like comparing missing delivery dates against statuses like canceled).

### Phase 2: Smart Data Cleaning & Wrangling
My data cleaning approach was guided by business logic, ensuring no data was dropped without a clear reason.

Custom Null Checking Logic: I created a custom Python function (check_null_products_in_orders) to see if products with missing dimensions actually had sales. Since they were actively selling, I chose a zero-drop policy to keep all revenue data safe.

Smart Imputation (Handling Missing Data):

Filled missing physical dimensions (weight, length, etc.) with the median value to avoid being affected by extreme outliers.

Replaced missing text categories with the word "unknown".

Normalized empty review fields with clear fallback text (like "customer did not leave a comment").

Fixing Data Types:

Converted raw text date columns into true datetime64 formats to make time analysis possible.

Changed numerical identifiers (like Zip Codes) into text formats (str) so Python doesn't treat them as math numbers.

Text Formatting: Cleaned up city names using lowercase conversion and removing extra spaces (.lower().str.strip()) to prevent duplicate entries.

## 💻 Tech Stack & Tools
Python 3.x

Pandas: For data manipulation, handling nulls, and data type formatting.

NumPy: For efficient calculations and handling structural logic.

## 📈 Key Achievements & Decisions
Business-First Cleaning: Instead of just deleting rows with missing details using .dropna(), my code checks if rows are linked to sales first, protecting 100% of the financial transaction history.

Efficiency at Scale: Used optimized, vectorized Pandas expressions to audit large tables quickly, including the 1-million-row geolocation dataset.
