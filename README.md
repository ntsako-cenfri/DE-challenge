# Data Engineering Take-Home Challenge

**Time allowed:** 2 hours  
**Submission:** A zipped folder

---

## Objective

Write a Python script that reads the four CSV files, fixes the obvious data quality issues, loads the cleaned data into a database, and answers two SQL questions.

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

- Write a Python script that reads each CSV file using `pandas`.
- Use functions — avoid writing one long procedural script.
- **Plus:** If your script is written in a way that it could handle a new CSV file with minimal changes (e.g. driven by a config or a reusable function), that is a strong signal and will be noted positively.

### 2. Storage

- Load the cleaned data into a local database. PostgreSQL is preferred; SQLite is acceptable.
- Use correct data types: `DECIMAL`/`REAL` for money, `DATE` or `TEXT` for dates, `INTEGER` for IDs.

### 3. Data Cleaning

Fix these issues during transformation:

- Prices stored as strings (e.g. `$10.00`) — strip the `$` and cast to a number
- Leading/trailing whitespace in category names
- Duplicate primary keys — keep the first occurrence
- Null values in email — treat as optional, do not error
- Future `order_date` values — drop those rows

### 4. One Test

Include **at least one** automated check, for example:

- Price must be a positive number after cleaning
- No duplicate `customer_id` values after deduplication

---

## Suggested Project Structure

```
submission/
├── data/               # The raw CSV files (unchanged)
├── ingestion.py        # Cleaning + loading logic
├── queries.sql         # SQL answers
├── requirements.txt    # Python dependencies
└── README.md           # How to run your solution
```

---

## SQL Queries

Write **at least two** SQL queries against the loaded data. The queries are your choice — pick anything that produces a meaningful result from the dataset.

Some ideas to get you started (you do not have to use these):

- Total revenue per product or category
- Top 5 customers by spend
- Number of orders per status
- Average order value per month
- Products that have never been ordered
- Any other insight you find interesting in the data

We are looking for correct results and clean, readable SQL.

---

## Evaluation Criteria

| Criteria | Marks | Exceptional | Passing | Failing |
| --- | --- | --- | --- | --- |
| **Data Cleaning** | 30 | Finds and fixes all listed issues | Fixes some issues | Loads dirty data as-is |
| **SQL Quality** | 25 | Correct results, clean readable queries | Correct results, messy query | Wrong results or no queries |
| **Code Structure** | 20 | Clean functions, easy to follow | Works but hard to read | One long script, no functions |
| **Data Types** | 15 | `DECIMAL` for money, `DATE` for dates | `TEXT` for everything | Fails to load due to type errors |
| **Testing** | 10 | Automated assertion (pytest or assert) | Print statement only | No check at all |
| **Total** | **100** | | | |

---

## Presentation (Panel Demo)

After submitting, you may be required to demonstrate your work to the panel:

1. Run your pipeline live and show the data landing in the database (a quick `SELECT` per table is enough)
2. Walk through one cleaning step in your code and explain why you made that choice
3. Run your SQL queries and briefly explain what each one shows
4. The panel will ask one or two follow-up questions — there are no trick answers, we want to see how you think

> **Tip:** A working solution that you can explain clearly is more valuable than a perfect solution you can't.

---

## Submission Checklist

- [ ] All four CSV files are in `data/` (unmodified)
- [ ] Pipeline runs end-to-end with a single command (e.g. `python ingestion.py`)
- [ ] `README.md` explains how to set up and run
- [ ] At least one automated check passes
- [ ] At least two SQL queries are included and return correct results
- [ ] You are ready to demonstrate your work if required
