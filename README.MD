# Exploratory Data Analysis (EDA) Projects

A comprehensive collection of end-to-end Data Analytics and Exploratory Data Analysis projects using Python, Pandas, Matplotlib, and Seaborn. This repository contains real-world datasets across multiple domains (Social Media Virality, Student Academic Performance, and Restaurant Dining Analytics), demonstrating complete data cleaning, outlier handling, feature engineering, and statistical visualization.

---

## 1. Repository Projects Overview

| Project Name | Domain | Dataset Size | Key Topics Covered | Detailed Documentation |
|---|---|---|---|---|
| **YouTube India Trending Analysis** | Social Media & Digital Content | 33,089 rows, 29 columns | Trending speed window, publish timing, engagement scaling, tag density | [View Documentation](YOUTUBE_TREND_ANALYSIS/README.md) |
| **Student Performance Analysis** | Education & Academic Analytics | 1,000 rows, 8 columns | Reading-writing correlation, test prep impact, parental education, lunch subsidies | [View Documentation](STUDENT_Performance/readme.md) |
| **Zomato Restaurant Analytics** | Food & Hospitality | 148 rows, 7 columns | Online ordering vs. walk-in dining, rating distributions, dining costs | [View Documentation](ZOMATO_DATA_SCIENCE/) |

---

## 2. Project Summaries & Key Findings

### 1. YouTube India Trending Video Analysis
- **Goal:** Analyze the mechanics of viral YouTube videos in India to determine what drives virality and how engagement metrics scale with reach.
- **Data:** 33,089 unique trending records (after removing 4,263 duplicates) across 29 total features (14 original, 15 engineered).
- **Core Findings:**
  - **72-Hour Golden Window:** 86.2% of videos hit trending within 1 to 3 days of upload (median = 2 days). Videos rarely trend after day 4.
  - **Best Upload Timing:** Thursday, Friday, and Saturday produce 47.7% of all trending videos and the highest average views.
  - **Engagement Correlation:** Likes and comments share a very strong linear correlation (r = 0.92), while views and likes correlate at r = 0.82.
  - **Metadata Practices:** 91.8% of trending videos utilize medium to heavy tag counts (6+ tags).

### 2. Student Performance in Exams
- **Goal:** Identify the demographic, social, and preparation factors that influence student performance across Math, Reading, and Writing.
- **Data:** 1,000 student records with 5 demographic attributes and 3 numerical test scores.
- **Core Findings:**
  - **Strongest Academic Link:** Reading and writing scores show a near-perfect correlation (r = 0.95). Students who read well almost always write well.
  - **Test Preparation Course:** Completing the prep course significantly raises scores across all three subjects (+10 points in writing).
  - **Socio-Economic Factors:** Standard lunch students score 11 to 14 points higher on average than free/reduced lunch students.
  - **Gender Differences:** Female students lead in reading (72.6 vs. 65.5) and writing (72.5 vs. 63.3), while male students score higher in math (68.7 vs. 63.7).

### 3. Zomato Restaurant Analytics
- **Goal:** Explore Bangalore restaurant data to evaluate consumer dining habits, delivery adoption, and pricing tiers.
- **Data:** Restaurant profiles covering online order availability, table bookings, user ratings, cost for two, and restaurant types.
- **Core Findings:**
  - **Online Delivery Adoption:** Restaurants offering online orders experience higher customer voting activity and engagement.
  - **Cost vs. Rating:** Dining-out and buffet restaurants have higher average costs for two compared to quick delivery outlets, with ratings clustering between 3.5 and 4.2.

---

## 3. Standard End-to-End EDA Process

Across each project, data is explored using a disciplined, production-grade 7-step analytical workflow:

- **Step 1: Data Understanding & Hygiene:**
  - Inspect column names, data types, missing values, and duplicate records.
  - Convert text strings into appropriate types (e.g. `pd.to_datetime()` for timestamps).
  - Clean and standardize column headers to lowercase `snake_case`.

- **Step 2: Outlier Detection & Treatment:**
  - Inspect extreme values using `.describe()` and boxplots.
  - Instead of dropping rows, apply 99th percentile capping using `np.percentile()` and `.clip(upper=limit)` to protect sample size while stabilizing visualizations.

- **Step 3: Feature Engineering & Segmentation:**
  - Create derived metrics (e.g. `days_to_trend`, engagement ratios like `likes/views`).
  - Segment continuous data into meaningful business groups using `pd.cut()` (fixed thresholds) and `pd.qcut()` (equal quartiles).
  - Extract text and time features (`publish_weekday`, `tag_count`, `has_description`).

- **Step 4: Univariate Analysis:**
  - Examine individual column distributions using histograms (`sns.histplot`) with KDE curves and countplots (`sns.countplot`).
  - Compare Mean vs. Median to identify skewness.

- **Step 5: Bivariate Analysis:**
  - Compare numerical targets across categorical groups using boxplots (`sns.boxplot`) and barplots (`sns.barplot`).
  - Test hypotheses (e.g. Do comments-enabled videos get more views? Does test prep improve scores?).

- **Step 6: Multivariate Analysis:**
  - Compute Pearson correlation matrix (`df.corr()`) and display heatmaps (`sns.heatmap`).
  - Create multi-variable pairplots (`sns.pairplot`) and 2D pivot table heatmaps to uncover interaction effects between multiple features.

- **Step 7: Business Insights & Recommendations:**
  - Translate statistical findings into concrete, data-backed recommendations for stakeholders.

---

## 4. Quick Revision Cheat-Sheet: Functions, Methods & Code Snippets

Use this reference table to review essential Pandas, NumPy, and Seaborn methods used across these projects during interviews or project walkthroughs.

### Core Data Science & Feature Engineering Methods

| Method / Concept | What It Does | Why & When We Use It | Practical Code Example |
|---|---|---|---|
| `pd.cut()` | Bins numbers into **custom fixed ranges** | When business logic requires specific cutoffs (e.g. 0 days = Same Day, 1-3 days, 4-7 days). Different bins can have different row counts. | `df['speed_band'] = pd.cut(df['days'], bins=[-1, 0, 3, 7, 365], labels=['Same Day', '1-3 Days', '4-7 Days', '7+ Days'])` |
| `pd.qcut()` | Bins numbers into **equal-sized quartiles** | When you want each group to contain the exact same percentage of rows (e.g. 25% each in Q1, Q2, Q3, Q4). | `df['views_quartile'] = pd.qcut(df['views'], q=4, labels=['Q1', 'Q2', 'Q3', 'Q4'])` |
| `np.percentile()` | Calculates the value at a specific percentile | To find the cutoff threshold where a given percentage of data falls (e.g. 99th percentile cutoff). | `limit = np.percentile(df['views'], 99)` |
| `.clip()` | Caps values at a lower or upper bound | Outlier capping: limits extreme viral values without dropping any rows or corrupting the sample size. | `df['views_capped'] = df['views'].clip(upper=limit)` |
| `np.where()` | Vectorized IF-ELSE conditional logic | Creates binary flags or categories based on a condition much faster than `.apply()`. | `df['has_desc'] = np.where(df['description'] != 'No description', 1, 0)` |
| `.describe()` | Generates summary statistics | Gives quick statistical profile: count, mean, std, min, 25%, 50% (median), 75%, max. | `views_summary = df['views'].describe()` |
| `.median()` | Computes the 50th percentile | Resists extreme viral outliers. Much more reliable than the mean for skewed data. | `likes_median = df['likes'].median()` |
| `.quantile()` | Finds value at any decimal percentile | To inspect specific quartiles (e.g. Q1 = 0.25, Q3 = 0.75) to understand data spread. | `likes_q3 = df['likes'].quantile(0.75)` |
| `.value_counts(normalize=True)` | Computes percentages of categories | Gives proportions of categorical levels directly (e.g. percentage of each tag level). | `tag_pcts = df['tag_use_level'].value_counts(normalize=True) * 100` |
| `.dt.normalize()` | Strips timestamp down to midnight date | Removes hours/minutes/seconds so two datetime objects can be compared by date alone. | `df['publish_date'] = df['publish_time'].dt.normalize()` |
| `.str.count()` | Counts occurrences of a character in strings | Used to count delimiter characters (like `|`) to calculate tag count vectorially. | `df['tag_count'] = df['tags'].str.count(r'\|') + 1` |

---

### Comparison: `pd.cut()` vs. `pd.qcut()`

| Feature | `pd.cut()` (Value-Based Binning) | `pd.qcut()` (Quantile-Based Binning) |
|---|---|---|
| **Bin Criteria** | Based on **specific threshold values** you define | Based on **sample distribution frequencies** (equal percentage of rows) |
| **Row Count Per Bin** | **Unequal** (one bin can have 80% of rows, another 2%) | **Equal** (every bin has exactly 1/N of the total rows, e.g. 25% each for quartiles) |
| **Primary Use Case** | When specific business boundaries matter (e.g. speed: Same Day, 1-3 Days) | When comparing low vs. high performers fairly (e.g. Q1 Lowest 25% vs. Q4 Top 25%) |
| **Example in Project** | `pd.cut(df['days_to_trend'], bins=[-1, 0, 3, 7, 365], ...)` | `pd.qcut(df['views'], q=4, labels=['Q1', 'Q2', 'Q3', 'Q4'])` |

---

### Visualization & Plotting Reference

| Function / Parameter | What It Does | Why & When We Use It | Code Snippet |
|---|---|---|---|
| `plt.rcParams.update()` | Sets global Matplotlib figure styling | Sets consistent figure size, fonts, title padding, and grid styling across all charts in the notebook. | `plt.rcParams.update({'figure.figsize': (10, 5), 'font.sans-serif': 'Segoe UI', 'axes.titlesize': 14})` |
| `sns.histplot()` | Histogram with density curve | Displays the frequency distribution of a continuous number along with a KDE curve. | `sns.histplot(df['views'], bins=30, kde=True)` |
| `sns.countplot()` | Bar chart of category counts | Shows frequencies of categorical levels (e.g. videos per weekday). | `sns.countplot(x='publish_weekday', data=df, order=day_order)` |
| `sns.boxplot()` | Five-number summary boxplot | Visualizes median, IQR box (25th to 75th percentile), and outlier points across categories. | `sns.boxplot(x='views_quartile', y='likes', data=df)` |
| `sns.heatmap()` | 2D color-coded matrix plot | Visualizes correlation matrices (`df.corr()`) or 2D pivot tables (`df.pivot_table()`). | `sns.heatmap(df.corr(), annot=True, cmap='coolwarm', fmt='.2f')` |
| `sns.pairplot()` | Grid of scatter and KDE plots | Shows pairwise bivariate scatter plots for all numeric features simultaneously. | `sns.pairplot(df[['views', 'likes', 'comment_count']])` |
| `plt.tight_layout()` | Automatically adjusts subplot spacing | Prevents axis labels, titles, and tick marks from clipping or overlapping. | `plt.tight_layout()` |
| `plt.savefig()` | Saves chart as high-resolution image | Exports plot cleanly to disk for reports and README documentation. | `plt.savefig('../images/views_distribution.png', dpi=300, bbox_inches='tight')` |

---

## 5. Practical Code Examples for Quick Revision

### Example 1: Calculating Median vs. 75th Percentile Spread
```python
# Check likes spread: compare median with 75th percentile (Q3)
likes_median = df['likes'].median()
likes_q3 = df['likes'].quantile(0.75)

print(f"75% of trending videos have {likes_q3:,.0f} likes or fewer, while the median is {likes_median:,.0f}.")
```
*Why this matters:* The median represents the typical video (50% above, 50% below). When the maximum value is 423,000 but the 75th percentile is only 12,011, it demonstrates extreme positive skewness.

---

### Example 2: Outlier Capping at the 99th Percentile
```python
import numpy as np

# Find the 99th percentile cutoff
views_p99 = np.percentile(df['views'], 99)

# Cap extreme values at the 99th percentile without dropping any rows
df['views_capped'] = df['views'].clip(upper=views_p99)
```
*Why this matters:* Dropping rows deletes valuable viral video data. Capping (Winsorizing at 99%) keeps all 33,089 records while preventing charts from stretching unreadably.

---

### Example 3: Full Distribution Plot with High-Resolution Export
```python
import matplotlib.pyplot as plt
import seaborn as sns

# Generate summary statistics
views_summary = df['views'].describe()
print(views_summary)

# Plot distribution
plt.figure()
sns.histplot(df['views'], bins=30, kde=True)
plt.title("Distribution of Views")
plt.xlabel("Views")
plt.ylabel("Count")
plt.tight_layout()
plt.savefig("../images/views_distribution.png", dpi=300, bbox_inches="tight")
plt.show()
```
*Why this matters:* Combining `.describe()` with `sns.histplot()` gives you both numerical confirmation and visual verification of skewness and spread.

---

## 6. Repository Folder Structure

```
EDA/
|-- README.MD                               # Root repository guide & revision cheat-sheet
|-- STUDENT_Performance/
|   |-- data/
|   |   \-- StudentsPerformance.csv         # 1,000 student academic records
|   |-- images/                             # Exported high-res charts
|   |-- notebooks/
|   |   \-- student_performance_analysis.ipynb
|   \-- readme.md                           # Student Performance project documentation
|-- YOUTUBE_TREND_ANALYSIS/
|   |-- data/
|   |   \-- youtube_india_cleaned.csv       # 33,089 cleaned trending records
|   |-- images/                             # 17 high-res visual charts
|   |-- notebooks/
|   |   \-- youtube_trend_analysis.ipynb
|   |-- requirements.txt                    # Project dependencies
|   \-- README.md                           # Comprehensive YouTube project documentation
\-- ZOMATO_DATA_SCIENCE/
    |-- Zomato data.csv                     # Bangalore restaurant dataset
    |-- image.png                           # Visual chart
    \-- zomato.ipynb                        # Zomato analysis notebook
```

---

## 7. Author

**Deepanshu Kapri**  
[GitHub Profile](https://github.com/Deepanshu-8126)
