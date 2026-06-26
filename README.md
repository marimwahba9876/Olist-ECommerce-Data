# Brazilian E-Commerce Data Exploration & Wrangling Pipeline
A meticulous, production-ready data pipeline dedicated to the deep exploration, architectural auditing, and programmatic cleaning of the Olist Brazilian E-Commerce Dataset. This phase focuses entirely on transforming raw, multi-table relational data into a pristine, high-integrity analytical asset through rigorous data wrangling and schema validation.

## 📌 Project Overview
Real-world e-commerce data is inherently messy, fragmented, and prone to anomalies. This project serves as the foundational data engineering layer that takes 9 interdependent datasets from Olist and applies rigorous programmatic checks. By auditing schema constraints, distinguishing transactional topologies, and applying defensive imputation strategies, the data is prepared for high-performance downstream modeling without sacrificing business-critical records.

## 🏗️ Schema Topology & Architectural Audit
Before modifying any data, a structural audit was performed to establish the relationship between tables, mapping them into an analytical schema framework:

### 🌟 Fact Tables (Transactional Core)
order_items: The granular operational anchor recording transaction metrics (price, freight_value) and linking products to sellers.

order_payments: Tracks payment sequences, methods, and installment structures.

order_reviews: Contains customer sentiment scores and text logs.

### 🧩 Dimension Tables (Contextual Anchors)
orders: The central structural bridge tracking order status lifecycle and critical operational timestamps.

customers: Maps localized customer entities via unique tracking IDs.

products: Stores physical dimensions, structural attributes, and catalog metadata.

sellers: Houses merchant registry records.

geolocation & products_category: Contextual dimensional lookup references.

## 🛠️ Pipeline Architecture & Execution Steps
The pipeline is split into two primary phases: Deep Programmatic Exploration and Defensive Data Wrangling.

Phase 1: Deep Structural Exploration
Instead of generic high-level overviews, every table underwent an algorithmic health check:

Dimensionality Testing: Validated shapes, structural boundaries, and dynamic memory footprints using .shape and .info().

Key Constraint Validation: Systematically verified the uniqueness of Candidate Keys (e.g., confirming order_id as a strict Primary Key in orders while analyzing its multi-row distribution inside the Fact tables).

Anomalous Missing Value Audits: Calculated relative missingness percentages to isolate systematic systemic drops (e.g., isolating missing delivery dates against active order statuses like shipped or canceled).

Phase 2: High-Integrity Data Wrangling
Data cleaning was approached through a business-logic-first lens, ensuring no records were dropped arbitrarily if they contained transaction value.

Impact-Driven Null Auditing (Custom Business Logic): Built an automated programmatic check (check_null_products_in_orders) to evaluate missing product dimensions against actual transaction charts. Discovering that null-attribute items had active sales, a strict zero-drop policy was enforced to preserve revenue tracking.

Defensive Imputation Strategies:

Missing physical metrics (weight, length, height, width) were imputed using median distribution calculations to shield metrics from outlier distortion.

Missing textual product categories were standardized to "unknown".

Missing customer review text fields were systematically normalized to fallback values ("customer did not leave a comment", "review not provided").

Strict Structural Type Casting:

Converted raw string representations of temporal metadata into precise native datetime64[us] objects across all operational lifecycle checkpoints.

Cast structural numerical codes (like Zip Code Prefixes) into string datatypes to prevent mathematical skewing during analytical indexing.

Text Sanitization: Standardized structural text variables via lowercase conversions and whitespace stripping (.lower().str.strip()) to eliminate semantic duplicates.

## 💻 Tech Stack & Tooling
Python 3.x

Pandas: For core structural dataframe operations, vectorization, and type-casting.

NumPy: Used for fast vectorized structural logic and mathematical aggregations.

## 📈 Technical Highlights & Engineering Decisions
Revenue-Preserving Imputation: Rather than performing generic drop operations (.dropna()) on rows with missing features, the code evaluates transactional intersections first, ensuring 100% financial data retention.

Memory-Efficient Data Auditing: Leveraged native pandas vectorized expressions to compute structural metrics rapidly across large datasets (e.g., the 1-million-row geolocation matrix).
