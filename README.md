# 📊 Marketing Analytics Project

A end-to-end marketing analytics solution built using **SQL Server**, **Python**, and **Power BI** to analyze customer behavior, product performance, and engagement data.

---

## 📁 Project Structure

```
├── sql/
│   ├── dim_customers.sql           # Customer dimension with geographic enrichment
│   ├── dim_products.sql            # Product dimension with price categorization
│   ├── fact_customer_journey.sql   # Cleaned and deduplicated customer journey data
│   ├── fact_customer_reviews.sql   # Cleaned customer review text data
│   └── fact_engagement_data.sql    # Normalized marketing engagement data
│
├── python/
│   └── customer_reviews_enrichment.py  # Sentiment analysis on customer reviews
│
├── BI_Report.pbix                  # Power BI dashboard and visualizations
└── README.md
```

---

## 🗄️ Database: SQL Server

**Database:** `PortfolioProject_MarketingAnalytics`

### Tables & Transformations

| File | Description |
|------|-------------|
| `dim_customers.sql` | Joins customers with geography table to enrich with `Country` and `City` |
| `dim_products.sql` | Categorizes products into `Low`, `Medium`, or `High` price tiers using `CASE` |
| `fact_customer_journey.sql` | Removes duplicates using `ROW_NUMBER()`, fills missing durations with date-level averages via `AVG() OVER()`, and standardizes `Stage` to uppercase |
| `fact_customer_reviews.sql` | Cleans `ReviewText` by replacing double spaces with single spaces |
| `fact_engagement_data.sql` | Splits combined `ViewsClicksCombined` column, normalizes `ContentType`, formats dates to `dd.MM.yyyy`, and filters out Newsletter content |

---

## 🐍 Python: Sentiment Analysis

**File:** `customer_reviews_enrichment.py`

**Dependencies:**
```bash
pip install pandas nltk pyodbc sqlalchemy
```

### What it does:
- Connects to SQL Server via `pyodbc` and fetches data from `fact_customer_reviews`
- Uses **VADER** (Valence Aware Dictionary and sEntiment Reasoner) from `nltk` to compute a compound sentiment score for each review
- Categorizes sentiment using **both** the text score and the star rating:

| Sentiment Score | Rating | Category |
|----------------|--------|----------|
| > 0.05 | ≥ 4 | Positive |
| > 0.05 | = 3 | Mixed Positive |
| > 0.05 | ≤ 2 | Mixed Negative |
| < -0.05 | ≤ 2 | Negative |
| < -0.05 | = 3 | Mixed Negative |
| < -0.05 | ≥ 4 | Mixed Positive |
| Neutral | ≥ 4 | Positive |
| Neutral | ≤ 2 | Negative |
| Neutral | = 3 | Neutral |

- Buckets sentiment scores into 4 ranges: `0.5 to 1.0`, `0.0 to 0.49`, `-0.49 to 0.0`, `-1.0 to -0.5`
- Outputs enriched data to `fact_customer_reviews_with_sentiment.csv`

### Output Columns:
```
ReviewID | CustomerID | ProductID | ReviewDate | Rating | ReviewText | SentimentScore | SentimentCategory | SentimentBucket
```

---

## 📈 Power BI Dashboard

**File:** `BI_Report.pbix`

The Power BI report connects to the cleaned SQL data and Python-enriched sentiment output to provide interactive visualizations including customer journey analysis, product performance, engagement metrics, and sentiment trends.

---

## 🚀 Getting Started

### Prerequisites
- SQL Server (with `SQLEXPRESS` instance) or compatible RDBMS
- Python 3.8+
- Power BI Desktop

### Steps

1. **Set up the database**
   - Create a database named `PortfolioProject_MarketingAnalytics`
   - Run the SQL scripts in the `sql/` folder to create and populate the tables

2. **Run sentiment analysis**
   ```bash
   pip install pandas nltk pyodbc sqlalchemy
   python customer_reviews_enrichment.py
   ```
   This generates `fact_customer_reviews_with_sentiment.csv`

3. **Open the Power BI report**
   - Open `BI_Report.pbix` in Power BI Desktop
   - Update the data source connection string if needed
   - Refresh the data to load the latest results

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| SQL Server | Data storage and transformation |
| Python (pandas, nltk) | Sentiment analysis and enrichment |
| Power BI | Data visualization and reporting |
| VADER (NLP) | Sentiment scoring of review text |

---

## 👤 Author

Feel free to connect or raise an issue if you have questions about the project!
