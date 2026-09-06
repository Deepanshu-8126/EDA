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

## 2. Project Summaries & Core Takeaways

### 1. YouTube India Trending Video Analysis
- **Goal:** Understand what makes a video trend on YouTube India and how engagement metrics scale with reach.
- **Data:** 33,089 unique trending records (after removing 4,263 duplicates) across 29 features (14 original, 15 engineered).
- **Core Findings:**
  - **72-Hour Golden Window:** 86.2% of videos hit trending within 1 to 3 days of upload (median = 2 days). Videos rarely trend after day 4.
  - **Best Upload Days:** Thursday, Friday, and Saturday produce 47.7% of all trending videos and the highest average views.
  - **Engagement Correlation:** Likes and comments have an extremely strong link (r = 0.92), while views and likes correlate at r = 0.82.
  - **Metadata Importance:** 91.8% of trending videos use 6 or more tags (Medium to High tag counts).

### 2. Student Performance in Exams
- **Goal:** Identify the key demographic and preparation factors that drive student scores in Math, Reading, and Writing.
- **Data:** 1,000 student records with 5 demographic attributes and 3 test scores.
- **Core Findings:**
  - **Strongest Academic Link:** Reading and writing scores have a near-perfect correlation (r = 0.95). Students who read well almost always write well.
  - **Prep Course Impact:** Completing a test preparation course adds an average of 10 points to writing scores and 5 to 7 points to math and reading.
  - **Socio-Economic Factors:** Students with standard lunch score 11 to 14 points higher than students on subsidized lunch.
  - **Gender Trends:** Females score higher in reading and writing, while males score higher in math.

### 3. Zomato Restaurant Analytics
- **Goal:** Analyze dining and delivery trends across restaurants in Bangalore.
- **Data:** 148 restaurant profiles covering online ordering, table reservations, ratings, cost for two, and restaurant types.
- **Core Findings:**
  - **Online Ordering:** Outlets offering online delivery receive substantially more reviews and customer votes.
  - **Cost vs. Rating:** Dining-out and buffet restaurants have higher average costs for two compared to quick delivery outlets.

---

## 3. End-to-End Dataset Processing Pipeline (Step-by-Step)

Here is the exact step-by-step workflow followed across these projects, explaining what was done and why each step is required in real-world data analytics:

### Step 1: Environment Setup & Pandas Display Configuration
Before loading data, we configure Pandas and plotting settings so that data is easy to read and numbers do not get truncated.

| Setting / Code | What It Does (Plain English) | Why We Use It |
|---|---|---|
| `pd.set_option("display.max_columns", None)` | Shows every column in the dataframe | By default, Pandas hides middle columns with `...`. Setting `None` lets us see all 29 columns. |
| `pd.set_option("display.max_colwidth", 120)` | Increases column text width to 120 characters | Prevents long text strings (like video titles or descriptions) from getting cut off in notebooks. |
| `pd.set_option("display.float_format", "{:,.2f}".format)` | Formats decimal numbers with commas and 2 decimals | Stops numbers from appearing in confusing scientific notation (e.g. shows `12,800,000.00` instead of `1.28e+07`). |
| `sns.set_theme(style='whitegrid')` | Adds a soft light-gray grid behind plots | Makes charts easy to read by letting viewers easily trace bar heights and data points to the axis values. |
| `plt.rcParams['figure.figsize'] = (10, 5)` | Sets the default chart size to 10 inches wide by 5 inches tall | Ensures every chart exported has a uniform, professional proportion. |
| `plt.rcParams['font.sans-serif'] = 'Segoe UI'` | Changes the default font to Segoe UI | Replaces blocky default matplotlib text with a clean, modern font. |

---

### Step 2: Safe Data Loading & Path Handling
```python
import os
import pandas as pd

raw_path = r"C:\Users\HP\Downloads\archive (7)\INvideos.csv"
clean_path = "../data/youtube_india_cleaned.csv" if os.path.exists("../data/youtube_india_cleaned.csv") else "data/youtube_india_cleaned.csv"

if os.path.exists(raw_path):
    df = pd.read_csv(raw_path)
    print("Loaded raw dataset from Downloads archive.")
else:
    df = pd.read_csv(clean_path)
    print(f"Loaded dataset from: {clean_path}")
```
- **Why use `import os` and `os.path.exists()`:** When running notebooks across different computers or folder levels (`notebooks/` vs root), paths can change. `os.path.exists()` checks if a file exists before opening it, preventing `FileNotFoundError`.
- **Why use `os.listdir()`:** Allows you to inspect what files and file extensions are inside a directory before trying to load them.

---

### Step 3: Column Header Standardization
```python
df.columns = [col.strip().lower().replace(" ", "_") for col in df.columns]
print(f"Dataset Shape: {df.shape}")
df.head(3)
```
- **Why we do this:** Raw datasets frequently contain accidental leading or trailing spaces (e.g. `' Views '` instead of `'views'`) and inconsistent capitalization (`'Publish Time'`).
- **How it works:**
  - `.strip()` removes accidental spaces at the start and end of column names.
  - `.lower()` turns everything into lowercase.
  - `.replace(" ", "_")` turns spaces into underscores (`snake_case`).
- **Result:** You can safely access columns with `df['views']` without getting unexpected `KeyError` bugs.

---

### Step 4: Data Quality Checks & Deduplication
- **Check dimensions:** `df.shape` shows total rows and columns.
- **Identify duplicate rows:** `df.duplicated().sum()` found **4,263 duplicate records**.
- **Remove duplicates:** `df.drop_duplicates(inplace=True)` reduced records from 37,352 to **33,089 clean rows**.
- **Handle missing text:** Missing video descriptions (561 rows) were filled using `df['description'].fillna('No description', inplace=True)`.

---

### Step 5: Datetime Conversion & Timezone Normalization
- `trending_date` was converted from string format to `datetime64` using `pd.to_datetime()`.
- `publish_time` (UTC timestamp) was parsed and normalized:
  - `df['publish_date'] = df['publish_time'].dt.normalize()`: Strips the hours, minutes, and seconds so we can compare upload dates directly with trending dates.
  - `df['publish_hour'] = df['publish_time'].dt.hour`: Extracts upload hour (0 to 23).
  - `df['publish_weekday'] = df['publish_time'].dt.day_name()`: Extracts the weekday name (Monday to Sunday).

---

### Step 6: Outlier Handling with 99th Percentile Capping
- **The Issue:** Viral mega-hits with 10 to 12 Crore views stretch charts, making the remaining 99% of data look like flat lines.
- **The Solution (Capping instead of Dropping):**
  - Dropping rows deletes valuable viral examples.
  - Instead, we find the 99th percentile value using `np.percentile(df['views'], 99)` (which is 1.28 Crore).
  - We cap extreme values using `df['views'].clip(upper=limit)`. Values above 1.28 Crore are set to 1.28 Crore, keeping all 33,089 rows intact.

---

### Step 7: Complete Feature Engineering (All 15 Derived Features)

Every derived feature created across the projects was engineered to answer specific business questions:

| Engineered Feature | Data Type | Creation Logic & Code | Business Purpose (Why We Created It) |
|---|---|---|---|
| `days_to_trend` | Integer | `(df['trending_date'] - df['publish_date']).dt.days` | Measures how many days passed between upload and hitting the trending page. |
| `trend_speed_band` | Categorical | `pd.cut(df['days_to_trend'], bins=[-1, 0, 3, 7, 365], labels=['Same Day', '1 to 3 Days', '4 to 7 Days', 'More than 7 Days'])` | Segments videos into custom fixed velocity tiers to find the critical virality window. |
| `views_quartile` | Categorical | `pd.qcut(df['views'], q=4, labels=['Q1', 'Q2', 'Q3', 'Q4'])` | Splits videos into 4 equal groups (25% rows each) to compare low vs. high performers fairly. |
| `tag_count` | Integer | `df['tags'].str.count(r'\|') + 1` | Counts how many tags the creator used by counting pipe separators (`|`). |
| `tag_use_level` | Categorical | `pd.cut(tag_count, bins=[-1, 0, 5, 15, 100], labels=['No Tags', 'Low', 'Medium', 'High'])` | Groups tag density into simple creator tiers. |
| `like_rate` | Float | `df['likes'] / df['views']` | Measures viewer conversion: how many likes are generated per view. |
| `comment_rate` | Float | `df['comment_count'] / df['views']` | Measures discussion conversion: how many comments are generated per view. |
| `has_description` | Binary | `np.where(df['description'] != 'No description', 1, 0)` | Binary flag to test if having a description impacts algorithmic reach. |
| `title_length` | Integer | `df['title'].str.len()` | Character count of video title to test if short or long titles perform better. |
| `description_length`| Integer | `df['description'].str.len()` | Character count of description text. |
| `publish_date` | Datetime | `df['publish_time'].dt.normalize()` | Upload date at midnight (00:00:00) for exact calendar day math. |
| `publish_hour` | Integer | `df['publish_time'].dt.hour` | Upload hour (0 to 23) to find peak publishing hours. |
| `publish_weekday` | Categorical | `df['publish_time'].dt.day_name()` | Day of the week video was published (Monday to Sunday). |
| `comments_status` | Categorical | `np.where(df['comments_disabled'], 'Disabled', 'Enabled')` | Clear readable label for chart legends. |
| `ratings_status` | Categorical | `np.where(df['ratings_disabled'], 'Disabled', 'Enabled')` | Clear readable label for like visibility charts. |

---

## 4. Quick Revision Cheat-Sheet: Functions, Methods & Code Snippets

Use these simple, plain-English reference tables when revising for interviews or reviewing project code.

### Core Data Science & Feature Engineering Methods

| Method / Concept | Simple Plain-English Definition | When to Use It | Practical Code Example |
|---|---|---|---|
| `pd.cut()` | Cuts continuous numbers into **custom fixed ranges** that you choose. | When you have business rules for cutoffs (e.g. 0 days = Same Day, 1-3 days, 4-7 days). Different bins can have different row counts. | `df['speed'] = pd.cut(df['days'], bins=[-1, 0, 3, 7, 365], labels=['Same Day', '1-3 Days', '4-7 Days', '7+ Days'])` |
| `pd.qcut()` | Cuts numbers into **equal-sized percentiles / quartiles**. | When you want every group to have the exact same number of rows (e.g. 4 quartiles where each bucket has exactly 25% of all rows). | `df['views_tier'] = pd.qcut(df['views'], q=4, labels=['Q1', 'Q2', 'Q3', 'Q4'])` |
| `np.percentile()` | Finds the exact number below which a given percentage of data falls. | When setting outlier cutoff thresholds (e.g. 99% of videos have fewer than 1.28 Crore views). | `cutoff = np.percentile(df['views'], 99)` |
| `.clip()` | Caps numbers at a maximum (or minimum) limit without deleting rows. | When you want to handle extreme viral outliers without losing any rows from your dataset. | `df['views_capped'] = df['views'].clip(upper=cutoff)` |
| `np.where()` | A fast vectorized IF-ELSE rule (`if condition then A else B`). | When creating binary 1/0 flags or label columns based on a condition. | `df['has_desc'] = np.where(df['description'] != 'No description', 1, 0)` |
| `.describe()` | Calculates 8 key statistical numbers for numeric columns. | When you want an immediate summary of count, mean, standard deviation, minimum, 25%, median (50%), 75%, and maximum. | `print(df['views'].describe())` |
| `.median()` | Finds the middle number (50th percentile) when data is sorted. | When data is skewed by extreme outliers. The median represents a typical video much better than the mean. | `median_likes = df['likes'].median()` |
| `.quantile()` | Finds the cutoff number at any decimal percentage. | When you want to see the 75th percentile (Q3) or 25th percentile (Q1) cutoff. | `q3_likes = df['likes'].quantile(0.75)` |
| `.value_counts(normalize=True)` | Counts the percentage share of each category. | When you want to find the proportion of rows belonging to each category (e.g. percentage of videos by weekday). | `pcts = df['publish_weekday'].value_counts(normalize=True) * 100` |
| `.dt.normalize()` | Removes the time portion (hours, minutes, seconds) from a timestamp. | When you want only the date (set to midnight 00:00:00) so you can do day-level arithmetic. | `df['pub_date'] = df['publish_time'].dt.normalize()` |
| `.str.count()` | Counts how many times a character appears inside text. | When counting pipe characters (`|`) in tags to get total tag count. | `df['tag_count'] = df['tags'].str.count(r'\|') + 1` |

---

### Key Comparison: `pd.cut()` vs. `pd.qcut()`

| Comparison Point | `pd.cut()` (Value-Based Binning) | `pd.qcut()` (Quantile-Based Binning) |
|---|---|---|
| **How Bins are Made** | Based on **fixed number thresholds** you specify (e.g. 0, 3, 7 days) | Based on **equal frequency** (sample distribution split into equal buckets) |
| **Row Count in Each Bin** | **Unequal** (one bin can have 86% of data, another can have 1%) | **Equal** (every bin has the exact same 25% of rows for quartiles) |
| **When to Choose** | When real-world categories have clear day or age limits | When you want to rank performance fairly into Low, Medium, and High tiers |
| **Project Example** | `pd.cut(df['days_to_trend'], bins=[-1, 0, 3, 7, 365], ...)` | `pd.qcut(df['views'], q=4, labels=['Q1', 'Q2', 'Q3', 'Q4'])` |

---

### Visualization & Plotting Setup Reference

| Function / Setting | What It Does (Plain English) | Why We Use It | Example Code |
|---|---|---|---|
| `sns.set_theme(style='whitegrid')` | Adds light gray grid lines behind charts | Helps viewers trace bars and scatter points to exact numbers on the Y-axis. | `sns.set_theme(style='whitegrid')` |
| `plt.rcParams['figure.figsize'] = (10, 5)` | Sets plot width to 10 and height to 5 inches | Standardizes chart proportions so all visuals look consistent in reports. | `plt.rcParams['figure.figsize'] = (10, 5)` |
| `plt.rcParams['font.sans-serif'] = 'Segoe UI'` | Uses the Segoe UI font for all titles and axis labels | Makes plots look modern, clean, and publication-ready. | `plt.rcParams['font.sans-serif'] = 'Segoe UI'` |
| `sns.histplot(kde=True)` | Plots a histogram with a smooth density curve | Shows where most numbers cluster and checks for skewness. | `sns.histplot(df['views'], bins=30, kde=True)` |
| `sns.countplot()` | Plots a bar chart of category frequencies | Shows how many records fall into each category (e.g. videos per weekday). | `sns.countplot(x='publish_weekday', data=df)` |
| `sns.boxplot()` | Draws a box showing Median, 25th-75th percentile, and outliers | Best way to compare numerical spreads across different categories. | `sns.boxplot(x='views_quartile', y='likes', data=df)` |
| `sns.heatmap()` | Draws a color-coded correlation or pivot grid | Quickest way to spot strong positive (red) or negative (blue) relationships. | `sns.heatmap(df.corr(), annot=True, cmap='coolwarm')` |
| `sns.pairplot()` | Generates a grid of scatter plots for all numeric columns | Lets you look at relationships between multiple features at the same time. | `sns.pairplot(df[['views', 'likes', 'comment_count']])` |
| `plt.tight_layout()` | Automatically adjusts spacing around the chart | Prevents axis labels, titles, and ticks from getting cut off at image borders. | `plt.tight_layout()` |
| `plt.savefig()` | Saves the chart to disk as a high-resolution image | Exports charts cleanly at 300 DPI for documentation and README embedding. | `plt.savefig('../images/views_dist.png', dpi=300, bbox_inches='tight')` |

---

## 5. Practical Code Walkthroughs for Quick Revision

### Example 1: Explaining Median vs. 75th Percentile Spread
```python
# Check likes spread: compare median (50th percentile) with 75th percentile (Q3)
likes_median = df['likes'].median()
likes_q3 = df['likes'].quantile(0.75)

print(f"75% of trending videos have {likes_q3:,.0f} likes or fewer, while the median is {likes_median:,.0f}.")
```
- **Plain English Meaning:** If the median is 2,757 likes and 75% of videos have 12,011 likes or fewer, but the maximum is over 423,000, it proves that extreme viral hits heavily skew the average. Most trending videos succeed with modest, solid engagement numbers.

---

### Example 2: Safe Outlier Capping at the 99th Percentile
```python
import numpy as np

# 1. Find the 99th percentile cutoff (where 99% of values sit below)
views_p99 = np.percentile(df['views'], 99)
print(f"99th Percentile Cutoff for Views: {views_p99:,.0f}")

# 2. Cap extreme values at this ceiling without dropping any rows
df['views_capped'] = df['views'].clip(upper=views_p99)
```
- **Plain English Meaning:** Instead of deleting mega-viral videos (which removes genuine data), we cap values above 1.28 Crore at 1.28 Crore. This keeps all 33,089 records while keeping charts readable.

---

### Example 3: Full Distribution Chart with Clean Export
```python
import matplotlib.pyplot as plt
import seaborn as sns

# 1. Check summary statistics
views_summary = df['views'].describe()
print(views_summary)

# 2. Plot distribution
plt.figure()
sns.histplot(df['views'], bins=30, kde=True)
plt.title("Distribution of Views")
plt.xlabel("Views")
plt.ylabel("Count")
plt.tight_layout()
plt.savefig("../images/views_distribution.png", dpi=300, bbox_inches="tight")
plt.show()
```
- **Plain English Meaning:** Combines `.describe()` (exact numbers) with `sns.histplot()` (visual shape) so you can immediately see both the numbers and the curve.

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
