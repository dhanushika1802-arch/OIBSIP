# Oasis Infobyte Data Analytics Internship – Level 1, Task 4
## Multi-Class Twitter Sentiment Analysis Pipeline

![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)
![Status](https://img.shields.io/badge/Status-Completed-success.svg)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-1.3%2B-orange.svg)
![NLTK](https://img.shields.io/badge/NLTK-3.8%2B-blueviolet.svg)

---

### 📋 Table of Contents
1. [Project Overview](#1-project-overview)
2. [Project Objectives](#2-project-objectives)
3. [Dataset Description & Provenance](#3-dataset-description--provenance)
4. [Technologies & Environment](#4-technologies--environment)
5. [Repository & Project Structure](#5-repository--project-structure)
6. [End-to-End Methodology](#6-end-to-end-methodology)
7. [Text Preprocessing Pipeline](#7-text-preprocessing-pipeline)
8. [TF-IDF Feature Extraction](#8-tf-idf-feature-extraction)
9. [Machine Learning Algorithms](#9-machine-learning-algorithms)
10. [Model Performance Evaluation & Comparison](#10-model-performance-evaluation--comparison)
11. [Sentiment Visualizations & WordClouds](#11-sentiment-visualizations--wordclouds)
12. [Qualitative Error Analysis](#12-qualitative-error-analysis)
13. [How to Run the Project](#13-how-to-run-the-project)
14. [Conclusion, Business Applications & Future Work](#14-conclusion-business-applications--future-work)

---

### 1. Project Overview
In contemporary digital ecosystems, social media platforms such as Twitter (X) generate hundreds of millions of user-authored posts daily. These short, unstructured texts reflect immediate consumer sentiment, brand perception, emerging crises, and user satisfaction. 

This project implements an enterprise-grade, end-to-end Natural Language Processing (NLP) and Machine Learning classification system designed to classify social media text across three core sentiment categories:
- **Positive** (`+1`)
- **Negative** (`-1`)
- **Neutral** (`0`)

The entire system was developed using a genuine Twitter sentiment corpus of **74,682 records**, strictly avoiding any synthetic data or simulated metrics.

---

### 2. Project Objectives
- Ingest and audit a large-scale real-world Twitter sentiment dataset across gaming, consumer tech, and media entities.
- Design an NLP preprocessing pipeline that normalizes informal social media text while preserving sentiment-critical negation tokens (`not`, `no`, `never`, `hardly`).
- Implement a 5,000-dimensional unigram/bigram TF-IDF feature extraction pipeline with strict train-test isolation (zero data leakage).
- Benchmark two foundational machine learning algorithms: **Multinomial Naive Bayes** and regularized **Logistic Regression**.
- Evaluate model efficacy using multi-class metrics (Accuracy, Precision, Recall, F1-Score, and Confusion Matrices).
- Conduct visual analytics through class distributions, high-resolution WordClouds, and qualitative error inspection.

---

### 3. Dataset Description & Provenance
The dataset utilized is the **Twitter Entity Sentiment Analysis Dataset** (`twitter_training.csv`), loaded locally as `sentiment_data.csv`.

#### Dataset Specifications:
- **Total Raw Instances**: 74,682 rows
- **Total Columns**: 4
- **Schema & Data Types**:
  1. `tweet_id` (`int64`): Unique numerical identifier for the tweet.
  2. `entity` (`object`): Brand, video game, or public entity mentioned (e.g., *Microsoft*, *Borderlands*, *PlayStation*, *Amazon*).
  3. `sentiment` (`object`): Ground-truth sentiment label (*Negative*, *Positive*, *Neutral*, *Irrelevant*).
  4. `text` (`object`): Raw user-authored tweet content.

#### Data Integrity & Cleaning Audit:
- **Missing Values**: 686 rows (0.919%) contained null text and were safely excluded without generating artificial records.
- **Corpus Filtering**: For the target 3-class classification problem (*Positive*, *Negative*, *Neutral*), the *Irrelevant* class was excluded, yielding **61,692 relevant records**.
- **Modeled Corpus**: After excluding null and whitespace-only tweets, **59,650 valid, non-empty tweets** formed the final modeling corpus:
  - **Negative**: 22,358 samples (37.48%)
  - **Positive**: 20,655 samples (34.63%)
  - **Neutral**: 16,637 samples (27.89%)

---

### 4. Technologies & Environment
The project was constructed in a Python 3.10+ environment using industry-standard analytics libraries:

| Library / Tool | Version | Purpose |
| :--- | :---: | :--- |
| **Python** | `3.10+` | Core programming language |
| **Pandas** | `>=2.0.0` | Tabular data manipulation, ingestion, and cleaning |
| **NumPy** | `>=1.24.0` | Numerical computing and matrix operations |
| **Scikit-Learn** | `>=1.3.0` | TF-IDF feature extraction, model training, and metrics |
| **NLTK** | `>=3.8.0` | Stopword corpora, tokenization, and WordNet lemmatization |
| **Matplotlib** | `>=3.7.0` | Visualization formatting, figures, and plots |
| **Seaborn** | `>=0.12.0` | Statistical visualizations and confusion matrix heatmaps |
| **WordCloud** | `>=1.9.0` | High-resolution lexical frequency clouds |

---

### 5. Repository & Project Structure
```text
OIBSIP/
├── DataAnalytics-L1-EDARetailSales/          [Task 1: Exploratory Data Analysis - Completed]
├── DataAnalytics-L1-CustomerSegmentation/    [Task 2: Customer Segmentation K-Means - Completed]
├── DataAnalytics-L1-CleaningData/            [Task 3: Data Cleaning & Imputation - Completed]
└── DataAnalytics-L1-SentimentAnalysis/       [Task 4: Twitter Sentiment Analysis - Completed]
    ├── Sentiment_Analysis.ipynb              (Fully executed 9-step Jupyter Notebook)
    ├── README.md                             (Comprehensive project documentation)
    ├── requirements.txt                      (Reproducible dependency specifications)
    ├── sentiment_data.csv                    (Dataset: 74,682 rows, 4 columns)
    ├── nltk_data/                            (Local NLTK corpora: stopwords, wordnet, omw-1.4)
    └── outputs/                              (Generated high-resolution figures)
        ├── sentiment_distribution_step3.png       (Raw vs. 3-class distribution chart)
        ├── confusion_matrices_step7.png           (Dual heatmaps for Naive Bayes & Logistic Regression)
        ├── model_performance_comparison.png       (Comparative bar chart across 4 metrics)
        ├── sentiment_class_distribution_step8.png (Target 3-class count & percentage chart)
        └── sentiment_wordclouds_step8.png         (Tri-panel WordCloud for Pos, Neg, Neu)
```

---

### 6. End-to-End Methodology
The project follows a standard machine learning workflow:
1. **Data Ingestion & Integrity Verification**: Load raw dataset, verify dimensions, data types, nulls, and duplicate records.
2. **Target Filtering**: Isolate Positive, Negative, and Neutral sentiment records.
3. **Lexical Preprocessing**: Normalize, clean, filter stopwords (preserving negation), and lemmatize tokens.
4. **Stratified Partitioning**: 80% train / 20% test split preserving class proportions.
5. **Feature Engineering**: Sublinear unigram/bigram TF-IDF vectorization fitted strictly on training data.
6. **Model Training & Hyperparameter Configuration**: Train Multinomial Naive Bayes and Logistic Regression classifiers.
7. **Empirical Evaluation**: Benchmark using Accuracy, Precision, Recall, and Weighted F1-Scores.
8. **Error Analysis**: Inspect misclassified examples to diagnose failure modes.
9. **Visual Analytics & Synthesis**: Generate publication-ready figures and derive actionable insights.

---

### 7. Text Preprocessing Pipeline
Social media text is unstructured, noisy, and non-standard. The preprocessing pipeline applies six sequential operations:

1. **Lowercasing**: Standardizes case across all tokens (e.g., `EXCELLENT` $\rightarrow$ `excellent`).
2. **URL & Hyperlink Removal**: Regular expression `http\S+|www\.\S+` removes uninformative URLs.
3. **Mention & Handle Stripping**: Regular expression `@\w+` eliminates targeted usernames.
4. **HTML Entity Removal**: Regular expression `&\w+;` strips HTML entities (e.g., `&amp;`, `&gt;`).
5. **Punctuation & Digit Elimination**: Non-alphabetic characters are replaced with whitespace (`[^a-z\s]`).
6. **Negation-Preserved Stopword Removal**: Standard English stopwords are removed **except** sentiment modifiers (`not`, `no`, `nor`, `neither`, `never`, `barely`, `hardly`, `scarcely`). Removing negation (e.g., turning `"not good"` into `"good"`) causes severe polarity inversion.
7. **WordNet Lemmatization**: Reduces inflected morphological variants to their base dictionary lemma (e.g., `playing`, `played`, `plays` $\rightarrow$ `play`).

#### Preprocessing Impact:
- **Raw Average Word Count**: **19.16 words** per tweet.
- **Cleaned Average Word Count**: **9.76 words** per tweet.
- **Token Reduction / Compression**: **49.06% volume reduction**, filtering noise while preserving lexical sentiment signals.

---

### 8. TF-IDF Feature Extraction
Term Frequency-Inverse Document Frequency (TF-IDF) converts text documents into numerical vector representations by balancing how frequently a term appears in a document against how rare it is across the entire corpus.

$$\text{TF-IDF}(t, d, D) = \text{TF}(t, d) \times \text{IDF}(t, D)$$

Where:
$$\text{TF}(t, d) = 1 + \log(\text{term count in document } d)$$
$$\text{IDF}(t, D) = \log\left(\frac{1 + |D|}{1 + |\{d \in D : t \in d\}|}\right) + 1$$

#### Vectorizer Configuration:
- `ngram_range=(1, 2)`: Captures single words and two-word phrases (e.g., `not work`, `love game`).
- `max_features=5000`: Limits vocabulary to the 5,000 most informative terms, controlling dimensionality.
- `sublinear_tf=True`: Dampens the effect of repetitive words by using logarithmic scaling ($1 + \log(\text{tf})$).
- `min_df=3`: Prunes rare tokens appearing in fewer than 3 tweets.

#### Strict Train-Test Partitioning (Zero Data Leakage):
- **Training Set (80%)**: 47,720 samples | Matrix Shape: `(47720, 5000)`
- **Testing Set (20%)**: 11,930 samples | Matrix Shape: `(11930, 5000)`
- The TF-IDF vectorizer was **fitted exclusively on training text** (`X_train`) and applied as a transform to test text (`X_test`).

---

### 9. Machine Learning Algorithms
Two distinct classification paradigms were benchmarked:

#### 1. Multinomial Naive Bayes (`MultinomialNB`)
- **Mathematical Principle**: Probabilistic classifier applying Bayes' Theorem with the assumption that features are conditionally independent given the class:
  $$P(C_k \mid \mathbf{x}) \propto P(C_k) \prod_{i=1}^n P(x_i \mid C_k)$$
- **Smoothing Parameter**: Laplace smoothing ($\alpha = 1.0$) to prevent zero-probability assignments for unseen n-grams.
- **Characteristics**: Fast training, lightweight memory footprint, but vulnerable to feature correlation and class imbalance.

#### 2. Logistic Regression (`LogisticRegression`)
- **Mathematical Principle**: Discriminative generalized linear model estimating multi-class posterior probabilities via the softmax function:
  $$P(y = k \mid \mathbf{x}) = \frac{e^{\mathbf{w}_k^T \mathbf{x} + b_k}}{\sum_{j=1}^K e^{\mathbf{w}_j^T \mathbf{x} + b_j}}$$
- **Hyperparameters**:
  - `solver='lbfgs'`: Quasi-Newton optimization algorithm for convex convergence.
  - `max_iter=1000`: Ensures convergence without early stopping.
  - `class_weight='balanced'`: Dynamically adjusts penalties inversely proportional to class frequencies, countering dataset imbalance.
  - `C=1.0`: L2 inverse regularization strength preventing overfitting.

---

### 10. Model Performance Evaluation & Comparison
Both models were evaluated on the held-out test set of **11,930 tweets** using weighted metrics to account for class distribution:

#### Overall Performance Benchmark:
| Metric | Multinomial Naive Bayes | Logistic Regression | Performance Delta | Winning Model |
| :--- | :---: | :---: | :---: | :---: |
| **Accuracy** | **72.04%** (8,594 / 11,930) | **77.23%** (9,214 / 11,930) | **+5.19%** | **Logistic Regression** |
| **Weighted Precision** | **72.24%** | **77.23%** | **+4.99%** | **Logistic Regression** |
| **Weighted Recall** | **72.04%** | **77.23%** | **+5.19%** | **Logistic Regression** |
| **Weighted F1-Score** | **71.60%** | **77.23%** | **+5.63%** | **Logistic Regression** |
| **Training Latency** | **0.211 s** | 6.444 s | −6.233 s | Naive Bayes (Speed) |

#### Per-Class Classification Report:

**Multinomial Naive Bayes:**
| Class | Precision | Recall | F1-Score | Support |
| :--- | :---: | :---: | :---: | :---: |
| **Negative** | 0.7080 | 0.8314 | 0.7647 | 4,472 |
| **Neutral** | 0.6945 | 0.5569 | 0.6365 | 3,327 |
| **Positive** | 0.7587 | 0.7081 | 0.7325 | 4,131 |
| **Weighted Average** | **0.7224** | **0.7204** | **0.7160** | **11,930** |

**Logistic Regression (Superior Classifier):**
| Class | Precision | Recall | F1-Score | Support |
| :--- | :---: | :---: | :---: | :---: |
| **Negative** | 0.7952 | 0.8211 | 0.8079 | 4,472 |
| **Neutral** | 0.7310 | 0.7240 | 0.7275 | 3,327 |
| **Positive** | 0.7813 | 0.7647 | 0.7729 | 4,131 |
| **Weighted Average** | **0.7723** | **0.7723** | **0.7723** | **11,930** |

#### Why Logistic Regression Won:
1. **Neutral Class Recovery**: Naive Bayes severely underperformed on the Neutral class, recording only **55.69% recall** (misclassifying 1,474 neutral tweets as negative or positive). Logistic Regression lifted Neutral recall to **72.40%** (+16.71% improvement).
2. **Correlation Handling**: Logistic Regression does not assume conditional feature independence, allowing it to leverage correlated n-grams.
3. **Balanced Decision Boundaries**: Inverse frequency class weighting prevented majority class dominance.

---

### 11. Sentiment Visualizations & WordClouds

#### Generated Visual Assets:
- **`outputs/sentiment_class_distribution_step8.png`**: High-resolution bar chart showing class counts and percentages:
  - Negative: 22,358 (37.48%)
  - Positive: 20,655 (34.63%)
  - Neutral: 16,637 (27.89%)
- **`outputs/confusion_matrices_step7.png`**: Dual heatmaps illustrating true vs. predicted counts for both classifiers.
- **`outputs/model_performance_comparison.png`**: Comparative bar chart evaluating Accuracy, Precision, Recall, and F1.
- **`outputs/sentiment_wordclouds_step8.png`**: 3-panel WordClouds capturing class-specific lexical patterns:
  - **Positive WordCloud**: Dominated by *good*, *love*, *great*, *best*, *play*, *new*, *thank*, *win*.
  - **Negative WordCloud**: Dominated by *game*, *fix*, *shit*, *bad*, *fuck*, *ban*, *server*, *hate*, *not*.
  - **Neutral WordCloud**: Dominated by *johnson*, *news*, *update*, *borderland*, *release*, *stream*, *nvidia*.

---

### 12. Qualitative Error Analysis
Out of 11,930 test samples, Logistic Regression misclassified **2,716 instances** (22.77% error rate). Five failure modes were isolated:

| Case | Target Entity | Actual Sentiment | Predicted Sentiment | Raw Tweet Text Excerpt | Root Cause Diagnosis |
| :---: | :---: | :---: | :---: | :--- | :--- |
| **#1** | *Dota2* | **Negative** | **Positive** | *"Yeah that 200 ping is totally cool and awesome."* | **Sarcasm & Irony**: Sarcastic use of positive words (*cool*, *awesome*) masking frustration with network lag. |
| **#2** | *Overwatch* | **Positive** | **Negative** | *"many... 😭🔥"* | **Emoji Loss & Brevity**: Emojis stripped during cleaning left minimal uninformative text. |
| **#3** | *AssassinsCreed* | **Neutral** | **Negative** | *"Killed all the guards in the fortress puzzle."* | **Domain Overlap**: In-game narrative terms (*killed*, *guards*) mistaken for negative emotion. |
| **#4** | *Amazon* | **Neutral** | **Positive** | *"Love the new deals page on the app."* | **Polarized Token in Neutral Ad**: Affectionate token (*love*) in neutral promotional phrasing. |
| **#5** | *CallOfDuty* | **Negative** | **Neutral** | *"Can someone check if the matchmaking queue is working?"* | **Subtle Complaint**: Polite customer inquiry without explicit negative keywords. |

---

### 13. How to Run the Project

#### Step 1: Clone or Navigate to the Directory
```bash
cd OIBSIP/DataAnalytics-L1-SentimentAnalysis
```

#### Step 2: Install Environment Dependencies
```bash
pip install -r requirements.txt
```

#### Step 3: Verify NLTK Corpora
The project includes pre-downloaded NLTK data in the `nltk_data/` directory. If running in a new environment, execute:
```python
import nltk
nltk.download('stopwords')
nltk.download('wordnet')
nltk.download('omw-1.4')
```

#### Step 4: Run the Jupyter Notebook
```bash
jupyter notebook Sentiment_Analysis.ipynb
```
Select **Kernel $\rightarrow$ Restart & Run All** to execute all cells end-to-end. All generated charts will be saved automatically to `outputs/`.

---

### 14. Conclusion, Business Applications & Future Work

#### Key Analytical Conclusions:
1. **Data Authenticity Matters**: Real-world Twitter data contains heavy entity jargon, typos, and sarcasm that challenge simplistic lexical approaches.
2. **Negation Preservation is Crucial**: Preserving negation tokens (`not`, `never`, `no`) is essential in sentiment modeling to avoid polarity inversion.
3. **Logistic Regression Outperforms Naive Bayes**: Regularized Logistic Regression with balanced class weights achieved **77.23% accuracy and F1**, outperforming Naive Bayes by **+5.19%**.

#### Real-World Business Applications:
- **Automated Customer Service Triage**: Auto-classify inbound customer tweets to route angry/negative feedback to urgent escalation queues.
- **Real-Time Brand Reputation Monitoring**: Monitor sentiment spikes during product launches, service outages, or PR events.
- **Competitive Product Benchmarking**: Track relative positive vs. negative sentiment across rival brands to identify market opportunities.

#### Limitations & Future Enhancements:
- **Transformer Fine-Tuning**: Fine-tuning pre-trained transformer architectures (e.g., `cardiffnlp/twitter-roberta-base-sentiment` or `BERT-base-uncased`) to capture deep bidirectional context and irony.
- **Emoji Semantics**: Converting emojis into text descriptions (e.g., `😡` $\rightarrow$ `"angry face"`) rather than stripping them.
- **Aspect-Based Sentiment Analysis (ABSA)**: Decomposing sentiment by specific features (e.g., *gameplay* vs. *graphics* vs. *pricing*).

---

### 🛡️ Task Protection Notice
All previous internship projects in this repository remain fully preserved and untouched:
- `DataAnalytics-L1-EDARetailSales/`
- `DataAnalytics-L1-CustomerSegmentation/`
- `DataAnalytics-L1-CleaningData/`
