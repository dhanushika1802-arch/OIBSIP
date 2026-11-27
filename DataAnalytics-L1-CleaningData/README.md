# 🧹 Data Cleaning & Preprocessing on Titanic Dataset

**Organization:** Oasis Infobyte (OIBSIP)  
**Internship Track:** Data Analytics — Level 1 Task 3  
**Project Title:** Data Cleaning and Data Quality Assurance  
**Author:** Data Analytics Intern  
**Raw Dataset:** `train.csv` (891 rows × 12 columns, 100% unaltered)  
**Cleaned Output:** `cleaned_train.csv` (891 rows × 20 columns, 0 missing values)  
**Notebook:** `Data_Cleaning.ipynb`  

---

## 📌 1. Project Overview & Objectives

Data cleaning is the most critical phase in analytical workflows. Unprocessed datasets frequently contain missing observations, structural discrepancies, improper datatypes, and outliers that introduce severe statistical bias into exploratory analyses and predictive models.

This project executes a rigorous, principled data cleaning pipeline on the classic **Titanic Passenger Manifest (`train.csv`)** without fabricating records or dropping valid observations.

### Key Objectives:
1. **Raw Data Ingestion & Quality Audit**: Formulate an initial data quality report documenting completeness, schema, datatypes, and duplicate entries.
2. **Context-Aware Imputation**:
   - Impute missing `Embarked` records using statistical mode.
   - Impute missing `Age` values utilizing title-stratified median age groups derived from passenger names (`Master`, `Miss`, `Mr`, `Mrs`, `Rare/Officer`).
   - Standardize `Cabin` sparsity by engineering categorical `Deck` tiers and `Has_Cabin` ownership flags.
3. **Inconsistency Resolution**: Normalize zero-fare tickets (`Fare == 0.00`) using class-specific median fares while tracking complimentary travel via `Is_Zero_Fare`.
4. **Data Type Correction & Feature Engineering**: Cast categorical attributes (`Sex`, `Embarked`, `Deck`) and derive relational features (`Family_Size`, `Is_Alone`, `Log_Fare`).
5. **Outlier Detection via IQR**: Evaluate numerical boundaries and document evidence-based justifications for retaining genuine historical extremes.
6. **Before-and-After Verification**: Validate data health improvements using quantitative comparison tables and statistical visualizations.
7. **Production Export**: Export the analysis-ready dataset to `cleaned_train.csv` while strictly preserving `train.csv` in its raw state.

---

## 🗂️ 2. Repository & Folder Structure

```text
OIBSIP/
├── DataAnalytics-L1-EDARetailSales/              # Task 1 (Completely Unchanged)
├── DataAnalytics-L1-CustomerSegmentation/        # Task 2 (Completely Unchanged)
└── DataAnalytics-L1-CleaningData/                # Task 3 Data Cleaning
    ├── outputs/
    │   ├── age_imputation_by_title.png
    │   ├── demographic_survival_rates.png
    │   ├── fare_distribution_and_log_transform.png
    │   ├── missing_values_before_after.png
    │   └── numerical_outliers_boxplots.png
    ├── cleaned_train.csv                         # Fully cleaned production dataset
    ├── Data_Cleaning.ipynb                       # Executed Jupyter Notebook
    ├── train.csv                                 # Original raw dataset (unmodified)
    ├── requirements.txt
    └── README.md
```

---

## 🔍 3. Initial Raw Data Quality Audit (`train.csv`)

- **Total Records:** `891` rows
- **Total Features:** `12` columns
- **Duplicate Records:** `0` (100% unique `PassengerId` from 1 to 891)
- **Total Missing Entries:** **866 null values** concentrated across 3 columns

| Column | Data Type | Non-Null | Missing Count | Missing (%) | Diagnostic Assessment |
|---|---|---|---|---|---|
| `PassengerId` | `int64` | 891 | 0 | 0.0% | Clean integer primary key. |
| `Survived` | `int64` | 891 | 0 | 0.0% | Clean binary target (0 = Deceased, 1 = Survived). |
| `Pclass` | `int64` | 891 | 0 | 0.0% | Clean socio-economic ordinal class (1, 2, 3). |
| `Name` | `object` | 891 | 0 | 0.0% | Clean strings; contains valuable honorific titles. |
| `Sex` | `object` | 891 | 0 | 0.0% | Clean binary string (`male`, `female`). |
| `Age` | `float64` | 714 | **177** | **19.87%** | Significant missingness (~20%); contains fractional infant ages. |
| `SibSp` | `int64` | 891 | 0 | 0.0% | Integer counts of siblings/spouses aboard. |
| `Parch` | `int64` | 891 | 0 | 0.0% | Integer counts of parents/children aboard. |
| `Ticket` | `object` | 891 | 0 | 0.0% | 681 unique tickets; 210 passengers share group/family tickets. |
| `Fare` | `float64` | 891 | 0 | 0.0% | 15 passengers recorded with £0.00 fare; extreme right-skewness. |
| `Cabin` | `object` | 204 | **687** | **77.10%** | Extreme sparsity (>77% missing); row deletion prohibited. |
| `Embarked` | `object` | 889 | **2** | **0.22%** | Negligible missingness (Passengers 62 and 830). |

---

## 🛠️ 4. Data Cleaning Methodology & Technical Decisions

### Decision 1: Port of Embarkation (`Embarked`)
- **Missing Count**: 2 records (Passengers 62 and 830, both 1st Class ticket holders).
- **Resolution**: Imputed using the statistical mode (`'S'` = Southampton), representing **72.4%** of all boarding passengers.
- **Historical Consistency**: Maritime manifests confirm both passengers boarded at Southampton.

### Decision 2: Title-Stratified Age Imputation (`Age`)
- **Flaw of Naive Imputation**: Applying an overall dataset median (28.0 years) assigns adult ages to young boys with the title 'Master', distorting child survival analysis.
- **Resolution**: Extracted social honorific titles from the `Name` field (`Mr`, `Miss`, `Mrs`, `Master`, `Rare/Officer`) and imputed missing ages using the **median age of the passenger's specific title group**:
  - `Master` (Young boys): Median **3.5 years**
  - `Miss` (Young unmarried females): Median **21.0 years**
  - `Mr` (Adult men): Median **30.0 years**
  - `Mrs` (Married adult women): Median **35.0 years**
  - `Rare/Officer` (Doctors, Clergy, Officers): Median **49.0 years**
- **Result**: Preserves natural age distributions across life stages.

### Decision 3: Structural Cabin Transformation (`Cabin`)
- **Problem**: 687 missing records (77.10%). Dropping rows would delete 77% of passenger records, while imputing synthetic cabin numbers invents false information.
- **Resolution**:
  1. Extracted `Deck` letter from the first character (`A`, `B`, `C`, `D`, `E`, `F`, `G`, `T`).
  2. Imputed missing values with `'Unknown'`.
  3. Created a binary indicator `Has_Cabin` (1 = Assigned Cabin, 0 = Missing/Unknown).

### Decision 4: Inconsistent Zero Fares (`Fare == 0.00`)
- **Problem**: 15 passengers have a recorded fare of £0.00 across all 3 classes (e.g., guarantee group shipyard mechanics and crew repositioning).
- **Resolution**:
  1. Created binary flag `Is_Zero_Fare` to preserve the historical signal that these passengers travelled on complimentary passes.
  2. Imputed £0.00 values with the **median non-zero fare for their respective `Pclass`**:
     - Class 1 Zero Fares imputed with **£61.98**
     - Class 2 Zero Fares imputed with **£15.02**
     - Class 3 Zero Fares imputed with **£8.05**

### Decision 5: Feature Engineering & Datatype Corrections
- Added `Family_Size = SibSp + Parch + 1` (Total traveling party size).
- Added `Is_Alone = 1 if Family_Size == 1 else 0`.
- Added `Log_Fare = np.log1p(Fare)` to compress positive skewness for parametric models.
- Cast categorical features (`Sex`, `Embarked`, `Deck`) to `category` dtypes.

---

## 🔍 5. Outlier Detection via IQR & Retention Justification

Using the Interquartile Range method ($IQR = Q3 - Q1$, Lower: $Q1 - 1.5 \times IQR$, Upper: $Q3 + 1.5 \times IQR$):

| Feature | Q1 | Q3 | IQR | Lower Threshold | Upper Threshold | Outliers Count | Outlier (%) | Max Observed | Cleaning Action |
|---|---|---|---|---|---|---|---|---|---|
| **`Age`** | 21.00 | 35.00 | 14.00 | 0.00 yrs | 56.00 yrs | **42** | 4.71% | 80.00 yrs | **Retained** (Valid senior ages) |
| **`Fare`** | £7.91 | £31.00 | £23.09 | -£26.72 | £65.63 | **116** | 13.02% | £512.33 | **Retained** (Valid 1st class luxury suites) |
| **`SibSp`** | 0.00 | 1.00 | 1.00 | -1.50 | 2.50 | **46** | 5.16% | 8.00 | **Retained** (Documented large families) |
| **`Parch`** | 0.00 | 0.00 | 0.00 | 0.00 | 0.00 | **213** | 23.91% | 6.00 | **Retained** (Documented large families) |

### 💡 Justification for Retaining Outliers:
1. **`Fare` Outliers (Max £512.33)**: These are **not data entry errors**. Historical records verify that Charlotte Cardeza, Thomas Drake Martinez Cardeza, and their staff booked the parlor suites (B51, B53, B55 on Ticket `PC 17755`) at the exact price of £512 6s 7d. Deleting high-spending passengers would destroy valid socioeconomic survival signals. Handled gracefully via `Log_Fare` rather than row deletion.
2. **`Age` Outliers (Max 80.0 yrs)**: Algernon Henry Barkworth (age 80) and other senior passengers are historically verified human ages.
3. **`SibSp` & `Parch` Outliers (Max 8 / 6)**: The 11-member Sage family traveling on ticket `CA. 2343` represents valid multi-generational traveling groups.

---

## 📋 6. Quantitative Before-and-After Comparison Table

| Metric / Attribute | Before Cleaning (`train.csv`) | After Cleaning (`cleaned_train.csv`) | Status / Improvement |
|---|---|---|:---:|
| **Total Rows** | 891 | 891 | ✅ Zero records lost |
| **Duplicate Rows** | 0 | 0 | ✅ Verified 100% unique |
| **Age Missing Count** | 177 (19.9%) | **0 (0.0%)** | ✅ Imputed via Title medians |
| **Age Mean / Median** | 29.70 / 28.00 | 29.39 / 30.00 | ✅ Preserved demographic distribution |
| **Cabin Missing Count** | 687 (77.1%) | **0 (0.0%)** | ✅ Extracted Deck + Has_Cabin flag |
| **Embarked Missing Count** | 2 (0.2%) | **0 (0.0%)** | ✅ Imputed with mode 'S' |
| **Zero Fare Count** | 15 passengers | **0 passengers** | ✅ Imputed via Pclass medians |
| **Fare Mean / Median** | £32.20 / £14.45 | £32.69 / £14.50 | ✅ Balanced financial metrics |
| **Total Columns** | 12 columns | **20 columns** | ✅ Added 8 high-value features |

---

## 📈 7. Visual Quality Deliverables

All visualizations are exported at 300 DPI in the [outputs](outputs) folder:
1. **`missing_values_before_after.png`**: Side-by-side bar chart showing elimination of 866 missing entries.
2. **`age_imputation_by_title.png`**: KDE distributions confirming that title-based imputation preserves natural age variance.
3. **`fare_distribution_and_log_transform.png`**: Raw right-skewed fare vs. normalized log-transformed fare distribution.
4. **`numerical_outliers_boxplots.png`**: Boxplots showing IQR boundaries and outlier spreads for `Age`, `Fare`, `SibSp`, and `Parch`.
5. **`demographic_survival_rates.png`**: Survival probabilities across cleaned `Sex` (Female 74.2% vs Male 18.9%), `Pclass` (1st 63.0% vs 3rd 24.2%), and `Embarked` ports.

---

## 🚀 8. How to Run the Project

1. **Navigate to the Directory:**
   ```bash
   cd OIBSIP/DataAnalytics-L1-CleaningData
   ```

2. **Install Dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Launch Jupyter Notebook:**
   ```bash
   jupyter notebook Data_Cleaning.ipynb
   ```

4. **Execute All Cells:**
   In Jupyter, click **Cell** > **Run All** to execute all code blocks and regenerate `cleaned_train.csv` and all visualization charts.
