# 🛍️ Flipkart Sales & Returns Analytics Dashboard

**An end-to-end data analytics project** — from a raw, messy 420K+ row e-commerce dataset to a fully interactive Power BI dashboard, uncovering the root causes behind revenue volatility and an abnormally high return rate.

(https://github.com/akashkum121/Flipkart_Analysis_Project/blob/main/Screenshots/Dashboard.png))

---

## 📖 Table of Contents
- [Project Overview](#-project-overview)
- [Problem Statement](#-problem-statement)
- [Dataset](#-dataset)
- [Tech Stack](#-tech-stack)
- [Data Cleaning Process](#-data-cleaning-process)
- [Data Model](#-data-model)
- [Dashboard Features](#-dashboard-features)
- [Key Insights](#-key-insights)
- [Recommendations](#-recommendations)
- [How to Run](#-how-to-run)
- [Project Structure](#-project-structure)

---

## 🧭 Project Overview

This project simulates a real-world Flipkart-style e-commerce analytics workflow — starting from **raw, intentionally messy transactional data** (customers, sellers, products, orders, order items, and returns) and ending with a **single-page executive Power BI dashboard** that surfaces actionable business insights.

The goal wasn't just to build charts — it was to **diagnose a business problem** (fluctuating revenue and a 45%+ return rate) using the data itself.

---

## ❓ Problem Statement

> Why is Flipkart's revenue inconsistent month-over-month, and why is the return rate sitting at an unusually high **45.67%** across the board?

This project answers that question through structured data cleaning, modeling, and visualization — rather than assuming the answer, the dashboard is built to let the data reveal the pattern.

---

## 🗂️ Dataset

Six relational tables, ~420K–1M rows each:

| Table | Description | Rows |
|---|---|---|
| `customers` | Customer demographics & signup info | 420,000 |
| `sellers` | Seller details & ratings | 420,000 |
| `products` | Product catalog with category/pricing | 420,000 |
| `orders` | Order-level transaction data | 415,843 |
| `order_items` | Line-item level detail (grain of analysis) | 980,489 |
| `returns` | Return records linked to order items | 447,838 |

The raw data included realistic messiness: mixed date formats, inconsistent currency symbols (₹ / Rs. / INR), typos in categorical fields, duplicate rows/IDs, missing values, and out-of-range outliers — all handled programmatically (see below).

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **Python** (Pandas, NumPy, dateutil) | Data cleaning, null handling, deduplication |
| **Power Query** | Data shaping & transformation inside Power BI |
| **DAX** | KPI measures, calculated columns |
| **Power BI** | Data modeling & interactive dashboard |

---

## 🧹 Data Cleaning Summary

The raw data was cleaned before loading into Power BI (Python + Power Query):

- **Mixed date formats** (`DD/MM/YYYY`, `MM/DD/YYYY`, `26-Dec-20`, etc.) → parsed and standardized to `YYYY-MM-DD`
- **Missing dates** → filled using a realistic random date within the observed min–max range
- **Currency inconsistency** (`₹499`, `Rs.499`, `INR 499`) → cleaned to plain floats
- **Categorical typos** (`Electronics`/`electronics `/`ELECTRONICS`, `Bangalore`/`Bengaluru`/`Banglore`) → standardized via lookup mapping
- **Duplicate rows & duplicate primary keys** → identified and removed
- **Missing gender** → inferred from first name using a name-to-gender lookup; remaining gaps filled proportionally to preserve distribution
- **Missing numeric fields** (ratings, prices, brand) → filled using group-wise averages/mode (by category/city) rather than blind defaults
- **Delivery partner / email nulls** → flagged explicitly (`has_email`, `"Not Assigned"`) instead of fabricating fake values

The CSVs in this repo are the **final cleaned outputs**, ready to load directly into Power BI.

---

## 🔗 Data Model

Star-schema style model centered on `order_items` (the finest grain):

```
customers ──1:N── orders ──1:N── order_items ──1:N── returns
                                     │  └──N:1── sellers
                                     └─────N:1── products
```

![Model Relationships](model_relationships.png)

All date columns include derived `Year`, `Month`, `Month Name`, `Quarter`, and `Year-Month` fields for time-intelligence visuals, with month sorted chronologically via "Sort by Column."

---

## 📊 Dashboard Features

**KPI Strip:** Total Revenue · Total Customers · Avg Order Value · Total Orders · Return Rate %

**Visuals:**
- **Monthly Revenue** — line chart tracking revenue by quarter/month
- **Top 10 Sellers** — ranked table with seller rating
- **Refund Amount by Year/City** — geographic map view
- **Return Orders by Reason & Category** — stacked bar
- **Return Rate % by Category** — trend comparison across all product categories
- **Monthly Refund Amount** — bar chart by month

**Interactive filters:** Quarter, Year-Month, Gender, City, Category

---

## 💡 Key Insights

1. **Revenue is volatile, not steadily declining** — clear dips in February (₹10.09bn) and August (₹10.62bn) suggest seasonal demand gaps rather than a structural decline.
2. **Return rate is abnormally high AND uniform across every category** (45.49%–45.8%) — Electronics, Fashion, Grocery, and Beauty all cluster within half a percentage point of each other. A genuine product-quality issue would show much wider variance between categories; this uniformity points to a **systemic cause** (return policy, listing accuracy, or logistics) rather than a category-specific defect problem.
3. **Return reasons are almost evenly split** — Item damaged in transit, Defective product, Changed my mind, Size issue, and Wrong item delivered are all within ~0.7K of each other (~83K each). No single dominant cause means no single quick fix — it's a broad experience issue.
4. **Refund cost is a constant drag, not a seasonal spike** — staying in a tight ₹175M–₹219M band every month rather than concentrating around holiday sales.
5. **Seller ratings are mediocre, not poor** (3.67–3.79 average) — consistent with a "good enough" experience that doesn't build strong buyer confidence, likely compounding the return problem.

---

## ✅ Recommendations

- Audit return policy friction — an even 45%+ rate across unrelated categories suggests returns may be used as a default "undo," not a genuine defect signal.
- Improve product listing accuracy (sizing/specs) to target the "Size issue" and "Wrong item delivered" reasons directly.
- Run a packaging/courier QA review — "Damaged in transit" remains a consistent top-3 return reason.
- Correlate Feb/Aug revenue dips with marketing calendar and competitor activity.
- Launch a seller quality-improvement track for sellers under a 3.8 rating threshold.

---

## ▶️ How to Run
Clone this repo
Open Power BI Desktop → Get Data → Text/CSV and load each file from data/
Set up relationships between tables as shown in model_relationships.png (star schema, centered on order_items)
Rebuild visuals using the Dashboard Features and DAX measures described above

---

## 📁 Project Structure

```
├── data/
│   ├── customers.csv
│   ├── order_items.csv
│   ├── orders.csv
│   ├── products.csv
│   ├── return_order_dates.csv
│   ├── returns.csv
|   └── sellers.csv
├── dashboard_screenshot.png
├── model_relationships.png
└── README.md
```

**Included in this repo:**
- ✅ Cleaned CSV files (star-schema tables)
- ✅ Power BI file (`.pbix`) with all visuals, measures, and relationships pre-built
- ✅ Dashboard screenshot for quick preview without opening Power BI
- ✅ Data model relationship diagram (screenshot) showing table connections

---

*This project uses a synthetically generated dataset built specifically for practicing real-world data cleaning and BI dashboard workflows.*
