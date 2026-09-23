# Zepto-data-sql-analytics
Project Overview
This project simulates an end-to-end data analysis workflow for an e-commerce inventory system using SQL. It demonstrates how data analysts explore, clean, and derive business value from real-world catalog data.
Database Setup: Establish a structured schema for a complex inventory dataset.
Exploratory Data Analysis (EDA): Analyze product availability, category distributions, and pricing structures.
Data Cleaning & Transformation: Handle missing values, eliminate invalid records, and convert currency fields from paise to rupees.
Business Intelligence Queries: Formulate analytical queries to generate actionable insights on revenue, stock availability, and discount strategies.
Dataset Overview
The dataset was sourced from Kaggle and contains product listings scraped from Zepto. It reflects typical production-grade e-commerce catalog data.
Each record represents a unique Stock Keeping Unit (SKU). Multiple entries for similar product names exist to account for variations in package size, weight, pricing, and category placement.
Schema & Field Descriptions
sku_id: Unique identifier for each product entry (Synthetic Primary Key)
name: Display name of the product
category: Product category classification (e.g., Fruits, Snacks, Beverages)
mrp: Maximum Retail Price in INR (converted from paise)
discountPercent: Percentage discount applied to the MRP
discountedSellingPrice: Final selling price in INR after applying discounts
availableQuantity: Number of units currently available in stock
weightInGms: Item weight measured in grams
outOfStock: Boolean indicator tracking item stock status
quantity: Package unit quantity specification
Project Execution Workflow
The analysis process is divided into five core phases:
1. Database Schema Initialization
Define and create the inventory table with explicit data types and constraints:


CREATE TABLE zepto (
  sku_id SERIAL PRIMARY KEY,
  category VARCHAR(120),
  name VARCHAR(150) NOT NULL,
  mrp NUMERIC(8,2),
  discountPercent NUMERIC(5,2),
  availableQuantity INTEGER,
  discountedSellingPrice NUMERIC(8,2),
  weightInGms INTEGER,
  outOfStock BOOLEAN,
  quantity INTEGER
);
2. Data Ingestion
The dataset is loaded using the PostgreSQL import wizard or via the standard CLI bulk load command:


\copy zepto(category, name, mrp, discountPercent, availableQuantity, discountedSellingPrice, weightInGms, outOfStock, quantity) FROM 'data/zepto_v2.csv' WITH (FORMAT csv, HEADER true, DELIMITER ',', QUOTE '"', ENCODING 'UTF8');
Note: Any character encoding errors encountered during ingestion are resolved by re-saving the source file as UTF-8.
3. Exploratory Data Analysis (EDA)
Total record count verification
Sample data inspection for structural validation
Null value analysis across attributes
Unique category distribution checks
Stock status ratio (in-stock vs. out-of-stock)
Identification of duplicate product listings across different SKUs
4. Data Cleaning
Filtered out records with invalid pricing (MRP or discounted price equal to zero)
Normalized pricing values from paise to Indian Rupees (₹) for standard readability
5. Key Business Insights Generated
Best-Value Products: Identified top 10 SKUs with the highest discount percentages
Stock Bottlenecks: Highlighted high-MRP products currently out of stock
Revenue Estimation: Calculated potential revenue projections per product category
Low-Discount Premium Items: Isolated high-value products (MRP > ₹500) offered with minimal discounts
Category Discount Rankings: Ranked top 5 categories by average discount offered
Unit Price Metrics: Evaluated price per gram across SKUs to determine cost-effectiveness
Weight Segmentation: Categorized products into Low, Medium, and Bulk weight brackets
Logistics Analysis: Aggregated total inventory weight per category for supply chain planning


