# 🎯 Customer Segmentation Analysis via RFM Modeling & K-Means Clustering

**Organization:** Oasis Infobyte (OIBSIP)  
**Internship Track:** Data Analytics — Level 1 Task 2  
**Project Title:** Customer Segmentation Analysis  
**Author:** Data Analytics Intern  
**Dataset:** `Online Retail.xlsx` (UCI Machine Learning Repository)  
**Notebook:** `Customer_Segmentation.ipynb`  

---

## 📌 1. Project Overview & Business Value

In modern e-commerce and retail analytics, one-size-fits-all marketing campaigns yield low conversion rates and high customer churn. **Customer Segmentation** addresses this challenge by grouping customers based on shared purchasing behaviors.

This project implements an end-to-end customer segmentation pipeline utilizing:
1. **RFM Modeling (Recency, Frequency, Monetary Value)**: Quantifies customer relationship recency, transaction cadence, and lifetime commercial value.
2. **Rule-Based RFM Scoring**: Establishes initial heuristic quartiles and operational segments.
3. **Unsupervised Machine Learning ($K$-Means Clustering)**: Identifies empirical, mathematical clusters in scaled RFM feature space.
4. **Actionable Marketing Playbooks**: Translates quantitative cluster centroids into targeted customer engagement and retention strategies.

---

## 🗂️ 2. Repository & Folder Structure

```text
OIBSIP/
├── DataAnalytics-L1-EDARetailSales/       # Task 1 (Unchanged)
└── DataAnalytics-L1-CustomerSegmentation/ # Task 2 Project
    ├── outputs/
    │   ├── cluster_3d_scatter.png
    │   ├── cluster_frequency_vs_monetary.png
    │   ├── cluster_profiles_heatmap.png
    │   ├── cluster_recency_vs_frequency.png
    │   ├── cluster_recency_vs_monetary.png
    │   ├── elbow_method_silhouette.png
    │   ├── rfm_distribution.png
    │   └── rfm_segments_count.png
    ├── Customer_Segmentation.ipynb
    ├── Online Retail.xlsx
    ├── requirements.txt
    └── README.md
```

---

## 🛠️ 3. Technologies & Libraries Used

| Technology | Version | Purpose |
|---|---|---|
| **Python** | `3.9+` | Core programming language |
| **Pandas** | `2.0.3` | Data cleaning, filtering, aggregation, and RFM engineering |
| **NumPy** | `1.24.3` | Numerical operations, logarithmic feature transformations |
| **OpenPyXL** | `3.1.5` | Ingestion of raw `.xlsx` multi-sheet transactional data |
| **Scikit-Learn** | `1.3.0` | `StandardScaler`, `KMeans`, and `silhouette_score` |
| **Matplotlib** | `3.7.2` | High-resolution publication-quality 2D/3D visualizations |
| **Seaborn** | `0.12.2` | Statistical distributions, heatmaps, and pair projections |
| **Jupyter** | `1.0.0` | Executed interactive notebook with complete embedded outputs |

---

## 🧹 4. Data Cleaning & Transformation Pipeline

The raw dataset (`Online Retail.xlsx`) contains transactional logs from a UK-based online retail store between December 1, 2010 and December 9, 2011.

### Data Cleaning Funnel:

```text
Total Raw Transactions Ingested:    541,909 rows
├── Drop Missing CustomerID:        -135,080 rows (Cannot attribute buyer)
├── Remove Duplicate Transactions:   -5,225 rows (Pervasive re-logging errors)
├── Remove Cancellations ('C'):      -8,872 rows (Invoices starting with 'C')
└── Filter Positive Qty & Price:       -40 rows (Negative debt / test entries)
────────────────────────────────────────────────────────────────────────────
Final Cleaned Transaction Log:      392,692 rows (72.5% retained)
Total Unique Customers Analyzed:      4,338 customers
```

- **Reference Date for Recency:** `2011-12-10 12:50:00` ($\max(\text{InvoiceDate}) + 1\text{ day}$).
- **Line Item Revenue:** $\text{TotalAmount} = \text{Quantity} \times \text{UnitPrice}$.

---

## 📊 5. RFM Baseline Statistics & Distributions

Across the **4,338 unique customers**, RFM metrics were computed:

| Metric | Mean | Median | Std Dev | Min | 25% (Q1) | 75% (Q3) | Max |
|---|---|---|---|---|---|---|---|
| **Recency (Days)** | 92.54 | 51.00 | 100.01 | 1.00 | 18.00 | 142.00 | 374.00 |
| **Frequency (Orders)** | 4.27 | 2.00 | 7.70 | 1.00 | 1.00 | 5.00 | 209.00 |
| **Monetary ($ Spend)** | $2,048.69 | $668.57 | $8,985.23 | $3.75 | $306.48 | $1,660.60 | $280,206.02 |

### Key Behavioral Insights:
- **Recency:** 50% of the customer base ordered within the last 51 days of the tracking period.
- **Frequency:** Strong right-skewness; while the median customer made **2 orders**, high-frequency repeat commercial clients placed up to **209 orders**.
- **Monetary:** High-ticket corporate buyers spend up to **$280,206.02**, pulling the mean spend ($2,048.69) significantly above the median ($668.57).

---

## 🏷️ 6. Rule-Based RFM Segmentation

Using 1–4 quartile scoring for $R$, $F$, and $M$ ($R$ inverted where 4 = most recent; $F$ and $M$ where 4 = highest volume/spend):

| Segment | Customer Count | Percentage Share | Description |
|---|---|---|---|
| **Dormant / Lost** | 1,507 | 34.7% | Inactive for >3–6 months, low lifetime spend. |
| **Champions / Loyal** | 1,349 | 31.1% | Highly active, frequent repeat buyers with high revenue. |
| **Recent / Potential** | 820 | 18.9% | Bought recently but low order history; prime onboarding targets. |
| **At Risk / Need Attention** | 662 | 15.3% | Previously high-spend/frequency customers whose recency is drifting. |

---

## 🤖 7. Machine Learning: K-Means Clustering

### Preprocessing & Scaling:
1. **Logarithmic Transformation (`np.log1p`)**: Resolved severe right-skewness and stabilized variance across Recency, Frequency, and Monetary attributes.
2. **StandardScaler**: Normalized transformed features to $\mu = 0$ and $\sigma = 1$.

### Optimal K Selection:
- **The Elbow Method**: Within-Cluster Sum of Squares (WCSS / Inertia) dropped sharply from $K=1$ ($13,014$) to $K=2$ ($6,484$), $K=3$ ($4,870$), and reached a clear elbow inflection at **$K=4$ (Inertia = $3,939$)**.
- **Silhouette Coefficient**: Maintained a strong score of **0.338** at $K=4$, validating distinct separation and meaningful commercial granularity.

---

## 👥 8. Final Customer Personas & Profiles

| Cluster Name | Customer Count | Pct Share | Avg Recency | Avg Frequency | Avg Monetary Spend | Median Spend |
|---|---|---|---|---|---|---|
| **Champions (High-Value VIPs)** | 713 | 16.4% | **12.2 days** | **13.8 orders** | **$8,088.02** | $4,582.50 |
| **Potential Loyalists (Regulars)** | 1,166 | 26.9% | **71.6 days** | **4.1 orders** | **$1,801.78** | $1,288.60 |
| **Recent / New Explorers** | 837 | 19.3% | **17.7 days** | **2.2 orders** | **$557.32** | $430.70 |
| **Dormant / At-Risk Customers** | 1,622 | 37.4% | **181.5 days** | **1.3 orders** | **$341.00** | $230.15 |

---

## 🎯 9. Actionable Marketing Playbooks

### 1. Champions (High-Value VIPs) — *Strategy: White-Glove Retention & Advocacy*
- **Characteristics**: Extremely active (average recency of 12 days) and massive commercial spend ($8,088 average spend).
- **Tactics**:
  - Assign dedicated VIP account support and priority order fulfillment.
  - Provide early-bird access to seasonal catalogs and limited-edition product drops.
  - Implement a points-based loyalty tier that rewards referrals and advocacy over price discounting.

### 2. Potential Loyalists (Regulars) — *Strategy: Basket Expansion & Upselling*
- **Characteristics**: Regular cadence (4 orders) and healthy basket size ($1,802 average spend).
- **Tactics**:
  - Deploy automated cross-selling engines (e.g. "Frequently Purchased Together").
  - Offer minimum threshold promotions (e.g. "Spend $200+ to receive complimentary shipping & gift").
  - Incentivize category exploration to expand purchase diversity.

### 3. Recent / New Explorers — *Strategy: Onboarding & Second-Purchase Acceleration*
- **Characteristics**: Bought within the last 2.5 weeks but have only completed 1–2 lifetime orders.
- **Tactics**:
  - Trigger an automated 3-part post-purchase email sequence (Order care guide + Brand story + 14-day re-order discount voucher).
  - Collect feedback via post-delivery NPS surveys to eliminate onboarding friction.

### 4. Dormant / At-Risk Customers — *Strategy: Win-Back & List Hygiene*
- **Characteristics**: Inactive for ~6 months (181.5 days average recency) with low lifetime engagement ($341 average spend).
- **Tactics**:
  - Execute a multi-stage "We Miss You" win-back re-engagement campaign with deep, time-limited discounts (e.g. 20–25% off next order).
  - Conduct churn survey outreach to determine whether dissatisfaction was product-related or service-related.
  - Suppress unengaged emails after 90 days of non-response to protect domain sender reputation.

---

## 📈 10. Saved Visualization Deliverables

All charts are saved in 300 DPI high-resolution format in the [outputs](outputs) folder:
1. `rfm_distribution.png`: Raw distributions for Recency, Frequency, and Monetary dimensions.
2. `rfm_segments_count.png`: Customer breakdown across rule-based RFM segments.
3. `elbow_method_silhouette.png`: Dual-panel Elbow curve (Inertia) and Silhouette score comparison across $K$.
4. `cluster_recency_vs_monetary.png`: 2D scatter plot of Recency vs. Monetary value (log scale).
5. `cluster_frequency_vs_monetary.png`: 2D scatter plot of Frequency vs. Monetary value (log-log scale).
6. `cluster_recency_vs_frequency.png`: 2D scatter plot of Recency vs. Frequency (log scale).
7. `cluster_3d_scatter.png`: Interactive 3D scatter plot of clusters across all three log-transformed RFM dimensions.
8. `cluster_profiles_heatmap.png`: Normalized heatmap illustrating the relative strengths of each cluster centroid.

---

## 🚀 11. How to Run the Project

1. **Navigate to the Project Directory:**
   ```bash
   cd OIBSIP/DataAnalytics-L1-CustomerSegmentation
   ```

2. **Install Required Packages:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Launch the Notebook:**
   ```bash
   jupyter notebook Customer_Segmentation.ipynb
   ```

4. **Execute All Cells:**
   In Jupyter, select **Cell** > **Run All** to run the complete data cleaning, modeling, and visualization pipeline.
