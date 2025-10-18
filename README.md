# ETL Product Data Pipeline

**Technologies:** Python, Pandas, SQLAlchemy, PostgreSQL, Requests, Logging, Postman

## 📌 Project Overview
Robust ETL pipeline to process product JSON data from a RESTful API and load it into PostgreSQL db.

### Objectives:
* Extract product data via API pagination using Postman for testing and Python for automation
* Transform data: clean, normalize, validate, calculate `price_with_discount`
* Load into PostgreSQL using staging-insert pattern
* Log all steps and errors for traceability

### Notes:
* **API Testing:** Used Postman to test endpoints, validate response structures, and verify pagination before Python implementation
* **Pagination Logic:** Implements `limit`/`skip` pattern, stops when `total` records reached
* **Staging Pattern:** Uses temporary staging table for atomic inserts

---

## 🛠️ Technologies & Libraries

* **Postman:** API endpoint testing, response validation, pagination verification
* **Python Standard Libraries:** `logging`
* **Data Handling:** `pandas` (explode nested reviews, cleaning)
* **API Client:** `requests` with Session for connection pooling
* **Database:** `sqlalchemy` for connection & transactions, PostgreSQL for storage

---

## 🔧 Pipeline Steps

### 1️⃣ Extraction
* Fetches paginated data from REST API with configurable limits
* Handles missing keys, extracts `products` key if exists, else full JSON
* Stops early when total record count reached
* Skips empty pages, continues fetching
* Combines into single raw DataFrame

### 2️⃣ Transformation
* Explodes nested `reviews` into separate rows
* Extracts `review_rating`
* Drops missing essential fields (`id`, `title`, `price`, `review_rating`)
* Validates prices (`>0`) and discounts (`0–100%`)
* Calculates `price_with_discount`
* Converts types and reorders columns

### 3️⃣ Loading
* Loads into staging table first
* Inserts into PostgreSQL with timestamps (`created_at`, `updated_at`)
* Transaction-managed, automatic rollback on errors
* Optionally drops staging table

---

## 📂 Dataset

* **Source:** DummyJSON Products API
* **Key fields:** `id`, `title`, `category`, `price`, `discountPercentage`, `rating`, `brand`, `reviews`

---

## 📊 Example Insights

* Detects invalid prices or discounts
* Normalizes nested reviews into rows
* Calculates discounted prices for analytics

---

## 🚀 How to Run
```bash
git clone https://github.com/YourUsername/ETL-Products-Pipeline.git
cd ETL-Products-Pipeline
pip install pandas sqlalchemy psycopg2-binary requests
python etl_pipeline.py
```

---
