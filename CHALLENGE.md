# Data Engineering Take-Home Challenge

**Time allowed:** 2–6 hours  
**Submission:** A zipped folder or GitHub repository link

---

## Objective

Build a reusable Python-based ETL pipeline that ingests raw CSV data into a PostgreSQL database.  
The solution must be robust enough to handle schema variations and data quality issues.

---

## The Dataset

You are given four CSV files in the `data/` folder:

| File | Description |
| --- | --- |
| `customers.csv` | 50 customers |
| `products.csv` | 25 products |
| `orders.csv` | 20 000 orders |
| `order_items.csv` | Line items per order |

> **Warning:** The files contain intentional data quality issues. Part of your task is to find and handle them.

---

## Technical Requirements

### 1. Ingestion (Python)

- Write a Python script that dynamically reads any of the CSV files.
- **Do not hard-code logic for a specific file.** The code will be tested against a 5th CSV file you have not seen.
- Use a configuration file (YAML, TOML, or JSON) to declare table mappings and file paths.

### 2. Storage (PostgreSQL)

- Load the cleaned data into a local PostgreSQL database.
- Define DDL with correct data types: `DECIMAL` for money, `TIMESTAMP` for dates, `UUID` or `SERIAL` for IDs.
- Apply appropriate primary key and foreign key constraints.

### 3. Data Profiling

Before loading, produce a summary report (printed or saved to file) that includes:

- Row count per file
- Null percentage per column
- Primary key uniqueness check

### 4. Data Quality Checks

Fix the following issues during transformation (you must detect them yourself):

- Inconsistent date formats
- Prices stored as strings (e.g. `$10.00`)
- Leading/trailing whitespace in text columns
- Duplicate primary keys
- Null values in required fields
- Logically invalid data (e.g. future order dates)
- Referential integrity violations (foreign key mismatches across files)

### 5. Unit Tests

Include **at least two** automated tests, for example:

- Price must be a positive number after cleaning
- No duplicate `customer_id` values after deduplication
- `order_items` must only reference valid `product_id` values

---

## Suggested Project Structure

```
submission/
├── data/               # The raw CSV files (unchanged)
├── src/
│   ├── ingestion.py    # Main ETL logic
│   ├── utils.py        # Database helpers
│   └── queries.sql     # Analytical SQL queries
├── tests/              # Pytest or assert-based tests
├── config.yaml         # Table mappings and file paths
├── requirements.txt    # Python dependencies
└── README.md           # How to run your solution
```

---

## Analytical SQL Queries

Write SQL queries (in `queries.sql` or inline) to answer these business questions:

1. **Revenue by Category**  
   Which product category generated the most revenue in the last 30 days?

2. **Churn Risk**  
   List customers who have not placed an order in the last 6 months but have a total lifetime spend over $500.

3. **Orphaned Items Audit**  
   Find all `order_items` rows that reference a `product_id` that does not exist in the `products` table.

---

## Evaluation Criteria

| Criteria | Exceptional | Passing | Failing |
| --- | --- | --- | --- |
| **Reusability** | Generic class/function driven by config; handles any CSV | Hard-coded for these 4 files only | One long script, no functions |
| **Data Types** | `DECIMAL` for money, `TIMESTAMP` for dates | `TEXT` for everything | Fails to load due to type errors |
| **Cleanliness** | Detects and fixes all issues; logs what was changed | Loads dirty data as-is | Ignores data quality entirely |
| **Testing** | `pytest` with assertions on counts and constraints | Simple print statements | No tests |
| **SQL Quality** | CTEs, window functions, proper use of `JOIN` | Subqueries that work but are unoptimised | Wrong results or syntax errors |

---

## Bonus (Optional)

- Add a `Dockerfile` and `docker-compose.yml` so the pipeline can be run with `docker compose up`.
- Use `SQLAlchemy` ORM rather than raw SQL for table creation.
- Log pipeline steps to a file using Python's `logging` module.

---

## Presentation (Panel Demo)

After submitting your solution you will present it live to the panel. The session runs **20–30 minutes** and follows this structure:

### 1. Walk Us Through Your Pipeline (10 min)

Run the pipeline live and narrate what is happening:

- Show the raw CSV files and point out the data quality issues you found.
- Demonstrate the profiling output — row counts, null percentages, uniqueness checks.
- Show the data landing in PostgreSQL (a quick `SELECT` per table is sufficient).

### 2. Code Walkthrough (10 min)

Open your editor and walk the panel through:

- How your ingestion script is generic — explain how it would handle the 5th CSV file we haven't given you.
- One transformation you are proud of (e.g. the date normalisation or the referential integrity fix).
- Your test suite — run `pytest` live and explain what each test asserts.

### 3. SQL Questions (5 min)

Run your three SQL queries live and explain the results:

- Which category drove the most revenue?
- Which customers are churn risks?
- How many orphaned order items exist, and what would you do about them in production?

### 4. Panel Q&A (5 min)

The panel may ask follow-up questions such as:

- *"If the orders file had 200 million rows, what would you change?"*
- *"How would you schedule this pipeline to run daily?"*
- *"How would you alert the team when referential integrity failures exceed a threshold?"*

> **Tip:** You do not need a perfect answer for every question. The panel is evaluating how you think, not just what you know.

---

## Submission Checklist

- [ ] All four CSV files are in `data/` (unmodified)
- [ ] Pipeline runs end-to-end with a single command (e.g. `python src/ingestion.py`)
- [ ] `README.md` explains setup, dependencies, and how to run
- [ ] At least two automated tests pass
- [ ] Three SQL queries are answered
- [ ] You are ready to demo the pipeline and answer questions live
