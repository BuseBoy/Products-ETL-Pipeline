# ETL Products Data Pipeline

**Technologies:** Python, Pandas, SQLAlchemy, PostgreSQL, Jupyter, Logging, JSON, Postman  

---

## 📌 Project Overview

Robust ETL pipeline to process **product JSON data** from a RESTful API and load it into **PostgreSQL db**.  

**Objectives:**
- Extract product data via **API pagination** using Postman Collection Runner (`limit`/`skip`) and Python loops  
- Transform data: clean, normalize, validate, calculate `price_with_discount`  
- Load into PostgreSQL using **staging-insert pattern**  
- Log all steps and errors for traceability  

**Notes:**
- Pagination → Normally we can GET the full data since API is small, but I did to make it compatible with real business cases.
- Homogeneous JSON structure → `load.json` used.


---

## 🛠️ Technologies & Libraries

- **Postman:** GET requests with `limit` & `skip`, batch simulation with Collection Runner  
- **Python Standard Libraries:** `json`, `logging`  
- **Data Handling:** `pandas` (explode nested reviews, cleaning)  
- **Database:** `sqlalchemy` for connection & transactions, PostgreSQL for storage  
- **Notebook:** Jupyter for analysis & documentation  

---

## 🔧 Pipeline Steps

### 1️⃣ Extraction
- Reads multiple JSON files or API responses  
- Handles missing files & invalid JSON  
- Extracts `products` key if exists, else full JSON  
- Combines into single raw DataFrame  

### 2️⃣ Transformation
- Explodes nested `reviews` into separate rows  
- Extracts `review_rating`  
- Drops missing essential fields (`id`, `title`, `price`, `review_rating`)  
- Validates prices (`>0`) and discounts (`0–100%`)  
- Calculates `price_with_discount`  
- Converts types and reorders columns  

### 3️⃣ Loading
- Loads into **staging table** first  
- Loads into PostgreSQL db.
- Transaction-managed, automatic rollback on errors  
- Optionally drops staging table  

---

## 📂 Dataset

- Source: [DummyJSON Products API](https://dummyjson.com/products)  
- Key fields: `id`, `title`, `category`, `price`, `discountPercentage`, `rating`, `brand`, `reviews`  

---

## 📊 Example Insights

- Detects invalid prices or discounts  
- Normalizes nested reviews into rows  
- Calculates discounted prices for analytics  

---

## 🚀 How to Run

```bash
git clone https://github.com/YourUsername/ETL-Products-Pipeline.git
cd ETL-Products-Pipeline
pip install pandas sqlalchemy psycopg2-binary
jupyter notebook etl_pipeline.ipynb
