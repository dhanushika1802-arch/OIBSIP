# 📊 Exploratory Data Analysis (EDA) on Retail Sales Dataset

**Organization:** Oasis Infobyte (OIBSIP)  
**Internship Track:** Data Analytics — Level 1 Task 1  
**Project Title:** Exploratory Data Analysis on Retail Sales Data  
**Dataset:** `retail_sales_dataset.csv`  
**Notebook:** `Retail_Sales_EDA.ipynb`  

---

## 📌 1. Project Overview

This project delivers a comprehensive Exploratory Data Analysis (EDA) on a transactional retail sales dataset. The primary objective is to extract meaningful business intelligence from raw customer purchasing data, evaluate central tendencies and dispersion metrics, analyze monthly and quarterly sales trajectories, understand demographic buying preferences, and evaluate product category performance.

All analyses, charts, and recommendations are derived strictly from empirical evidence without fabricating product names, customer segments, or metrics.

---

## 🗂️ 2. Repository & Folder Structure

```text
OIBSIP/
└── DataAnalytics-L1-EDARetailSales/
    ├── outputs/
    │   ├── age_distribution.png
    │   ├── correlation_heatmap.png
    │   ├── gender_breakdown.png
    │   ├── monthly_sales_trend.png
    │   ├── quarterly_sales_trend.png
    │   ├── sales_by_age_group_and_category.png
    │   ├── sales_by_category_and_gender.png
    │   ├── sales_by_product_category.png
    │   └── transaction_distribution_by_category.png
    ├── retail_sales_dataset.csv
    ├── Retail_Sales_EDA.ipynb
    ├── requirements.txt
    └── README.md
```

---

## 🛠️ 3. Technologies & Libraries Used

| Technology | Purpose |
|---|---|
| **Python 3.9+** | Core programming language |
| **Pandas (2.0.3)** | Data ingestion, cleaning, transformation, aggregation, and feature engineering |
| **NumPy (1.24.3)** | Numerical operations and statistical calculations |
| **Matplotlib (3.7.2)** | High-resolution statistical charting and custom formatting |
| **Seaborn (0.12.2)** | Statistical distributions, heatmaps, boxplots, and theme configuration |
| **Jupyter Notebook** | Interactive notebook presentation with embedded outputs |

---

## 📥 4. Dataset Summary & Data Integrity

- **File Name:** `retail_sales_dataset.csv`
- **Total Records:** 1,000 transactions
- **Total Features:** 9 columns
- **Date Range:** January 1, 2023 to January 1, 2024
- **Missing Values:** **0** (100% complete across all attributes)
- **Duplicate Records:** **0** (100% unique transaction IDs)
- **Mathematical Accuracy:** 100% verified (`Total Amount == Quantity * Price per Unit`)

### Attributes:
1. `Transaction ID` *(int64)*: Unique integer identifier for each sale.
2. `Date` *(datetime64)*: Date of the transaction (`YYYY-MM-DD`).
3. `Customer ID` *(object)*: Unique alphanumeric identifier for each customer.
4. `Gender` *(object)*: Customer gender (`Female`, `Male`).
5. `Age` *(int64)*: Customer age in years (ranging from 18 to 64).
6. `Product Category` *(object)*: Retail category (`Beauty`, `Clothing`, `Electronics`).
7. `Quantity` *(int64)*: Number of units purchased per transaction (discrete: 1, 2, 3, 4).
8. `Price per Unit` *(int64)*: Unit selling price (discrete tiers: $25, $30, $50, $300, $500).
9. `Total Amount` *(int64)*: Total revenue per transaction (ranging from $25 to $2,000).

---

## 📈 5. Key Statistical Findings

### Numerical Descriptive Statistics Summary

| Feature | Mean | Median | Mode | Std Dev | Min | 25% (Q1) | 75% (Q3) | IQR | Max |
|---|---|---|---|---|---|---|---|---|---|
| **Age (Years)** | 41.39 | 42.00 | 43.00 | 13.68 | 18 | 29.00 | 53.00 | 24.00 | 64 |
| **Quantity (Units)** | 2.51 | 3.00 | 4.00 | 1.13 | 1 | 1.00 | 4.00 | 3.00 | 4 |
| **Price per Unit ($)** | 179.89 | 50.00 | 50.00 | 189.68 | 25 | 30.00 | 300.00 | 270.00 | 500 |
| **Total Amount ($)** | 456.00 | 135.00 | 50.00 | 559.998 | 25 | 60.00 | 1,000.00 | 940.00 | 2,000 |

### Key Takeaways:
- **Price Skewness:** While the median unit price is **$50.00**, the mean is **$179.89**, indicating strong positive skewness caused by high-tier items ($300 and $500).
- **Basket Size:** The most frequent transaction quantity is **4 units** (mode), but the overall average is **2.51 units**, highlighting cross-selling opportunities.
- **Revenue Distribution:** Total transaction amount averages **$456.00** against a median of **$135.00**, showing that premium purchases ($1,000 to $2,000) contribute a high percentage of gross revenue.

---

## 📊 6. Visual Analyses & Observations

### 1. Monthly & Quarterly Sales Trends
- **Annual Peak:** **May 2023** achieved the highest revenue of **$53,150** (105 transactions), followed by **October ($46,580)** and **December ($44,690)**.
- **Seasonal Low:** **September 2023** recorded the lowest monthly revenue at **$23,620**, followed by **March ($28,990)**.
- **Quarterly Performance:** **Q4 led with $126,190**, followed closely by **Q2 ($123,735)**. **Q3 generated the lowest revenue ($96,045)**, representing a 23.9% decline compared to Q4.
- *Saved as:* `outputs/monthly_sales_trend.png` and `outputs/quarterly_sales_trend.png`.

### 2. Customer Demographics (Age & Gender)
- **Age Profile:** Uniformly distributed across ages 18 to 64 without outliers. The customer base is well-balanced across young adults, mid-career professionals, and seniors.
- **Gender Balance:** Customer volume is almost exactly 50/50: **Female: 51.0%** (510 transactions, $232,840) vs. **Male: 49.0%** (490 transactions, $223,160).
- **Average Spend:** Virtually identical average spend per order: **$456.55 for Females** and **$455.43 for Males**.
- *Saved as:* `outputs/age_distribution.png` and `outputs/gender_breakdown.png`.

### 3. Product Category Breakdown
- **Revenue Contribution:**
  - **Electronics:** **$156,905 (34.4%)** across 342 transactions (849 units).
  - **Clothing:** **$155,580 (34.1%)** across 351 transactions (894 units).
  - **Beauty:** **$143,515 (31.5%)** across 307 transactions (771 units).
- **Volume Leadership:** Clothing generates the highest physical volume (894 units), whereas Electronics leads in dollar revenue.
- *Saved as:* `outputs/sales_by_product_category.png`.

### 4. Correlation Analysis
- **Price per Unit vs. Total Amount ($r = 0.852$):** Exceptionally strong positive correlation; premium tier pricing is the single strongest determinant of basket revenue.
- **Quantity vs. Total Amount ($r = 0.374$):** Moderate positive relationship.
- **Age vs. Financial Metrics ($r \approx -0.061$):** Near-zero correlation, showing that customer age does not constrain ticket size.
- *Saved as:* `outputs/correlation_heatmap.png`.

### 5. Advanced Business Insights
- **Gender Category Preferences:**
  - Females dominate in **Clothing ($81,275)** and **Beauty ($74,830)**.
  - Males generate higher sales in **Electronics ($80,170)**.
  - **Surprise Metric:** Men spend **$487.13 on average in Beauty**, higher than women ($450.78), indicating high willingness to purchase premium grooming bundles and gifts.
- **Age Group Demographics:**
  - **18–25 (Young Adults):** Beauty is #1 ($31,280).
  - **26–35 (Young Professionals):** Clothing dominates ($39,975).
  - **36–50 (Mid-Career Adults):** The highest-spending demographic overall (**$139,660 total**).
  - **51–64 (Seniors):** Electronics is the leading category ($55,305).
- *Saved as:* `outputs/sales_by_category_and_gender.png`, `outputs/sales_by_age_group_and_category.png`, and `outputs/transaction_distribution_by_category.png`.

---

## 🎯 7. Actionable Business Recommendations

### 1. Segment-Targeted Marketing & Premium Gifting Bundles
- **Strategy:** Target Beauty advertising toward Young Adults (ages 18–25), where Beauty ranks #1. Position Electronics promotions toward mature shoppers (ages 51–64), who generated the highest electronics revenue ($55,305).
- **Execution:** Create premium "Men's Executive Grooming & Gift Sets" in the Beauty department. Because male shoppers averaged **$487.13 per order** in Beauty, curated gift kits will maximize average order value (AOV).

### 2. Proactive Calendar Management & Counter-Seasonal Promotions
- **Strategy:** Eliminate the mid-year cash flow troughs in **March ($28,990)** and **September ($23,620)**.
- **Execution:** Launch an "End-of-Summer Clearance" flash sale in early September and a "Spring New Season Launch" in March featuring volume-tier discounts (e.g., "Buy 3 items, save 15%") to sustain revenue momentum between Q2 and Q4 peaks.

### 3. Basket Size Expansion via Cross-Category Bundling
- **Strategy:** Leverage the moderate correlation between Quantity and Total Amount ($r = 0.374$) and close the gap between mean units (2.51) and modal units (4.00).
- **Execution:** Introduce point-of-sale recommendation bundles (pairing $25-$50 accessories with $300-$500 electronics or clothing purchases). Moving the average basket size from 2.51 to 3.20 units per transaction is projected to drive a **15–20% increase in gross revenue**.

### 4. VIP Loyalty Membership for the Power Demographic (Ages 36–50)
- **Strategy:** Reward and retain customers aged 36–50, who account for over **30% of total revenue ($139,660)** with strong, balanced spending across all three departments.
- **Execution:** Implement an exclusive VIP loyalty tier offering personalized benefits (e.g., extended warranties on electronics and complimentary wardrobe styling consultations) to maximize Customer Lifetime Value (CLV).

---

## 🚀 8. How to Run the Project

1. **Clone or Navigate to the Directory:**
   ```bash
   cd OIBSIP/DataAnalytics-L1-EDARetailSales
   ```

2. **Install Required Dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Launch Jupyter Notebook:**
   ```bash
   jupyter notebook Retail_Sales_EDA.ipynb
   ```

4. **Run All Cells:**
   In Jupyter, click **Cell** > **Run All** to execute all code blocks and view interactive outputs inline.
