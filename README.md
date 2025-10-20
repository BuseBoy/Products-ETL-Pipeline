# ETL Product Data Pipeline

**Technologies:** Python, Pandas, SQLAlchemy, PostgreSQL, Requests, Logging

## 📌 Project Overview
This project implements a robust ETL pipeline to process product JSON data from an API and load it into a PostgreSQL database. The pipeline:

- Extracts product data from API endpoints and combines all pages into a pandas DataFrame.
- Transforms data: normalizes column names, explodes nested reviews, removes duplicates/missing values, calculates discounted price.
- Loads cleaned data into PostgreSQL using a staging-to-main table pattern with conflict handling.
- Logs all steps for traceability and debugging.

---

## 🔧 Pipeline Steps

### 1️⃣ Extraction
- Fetches paginated data from API with `limit`/`skip` logic.
- Extracts `products` key if present, else uses full JSON.
- Skips empty pages and stops once all records are fetched.
- Returns a combined raw DataFrame.

### 2️⃣ Transformation
- Explodes nested `reviews` into separate rows.
- Extracts `reviewer_rating` and `reviewer_name`.
- Drops rows with missing critical fields and duplicate reviews.
- Validates `price` (>0) and `discount_percentage` (0–100%).
- Calculates `price_with_discount`.
- Standardizes column types and order.
- Returns a clean, ready-to-load DataFrame.

### 3️⃣ Loading
- Loads data into a staging table temporarily.
- Merges staging into main table, updating existing rows on conflict (`id + reviewer_name`).
- Automatically sets `created_at` and `updated_at`.
- Uses transactions for atomic operations.
- Optionally drops the staging table after load.

---

## 📂 Dataset
- **Source:** DummyJSON Products API
- **Key fields:** `id`, `title`, `category`, `price`, `discountPercentage`, `rating`, `brand`, `reviews`

---

## 🚀 How to Run
Clone the repository, install dependencies, and run the pipeline:

```bash
git clone https://github.com/YourUsername/ETL-Products-Pipeline.git
cd ETL-Products-Pipeline
pip install pandas sqlalchemy psycopg2-binary requests
