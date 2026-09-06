# Exploratory Data Analysis (EDA) Projects

A collection of end-to-end Data Analytics and Exploratory Data Analysis projects using Python, Pandas, Matplotlib, and Seaborn. This repository covers real-world datasets across three domains: Social Media Virality, Student Academic Performance, and Restaurant Dining Analytics. It shows complete data cleaning, outlier capping, feature engineering, and visual analysis.

---

## 1. Projects in This Repository

| Project Name | Domain | Dataset Size | Key Questions Answered | Documentation |
|---|---|---|---|---|
| **YouTube India Trending Analysis** | Social Media & Content Strategy | 33,089 rows, 29 columns | How fast do videos trend? Best upload days? How do likes and comments scale with views? | [View Project](YOUTUBE_TREND_ANALYSIS/README.md) |
| **Student Performance Analysis** | Education & Academic Research | 1,000 rows, 8 columns | Does test prep help? How closely are reading and writing linked? How do lunch subsidies affect scores? | [View Project](STUDENT_Performance/readme.md) |
| **Zomato Restaurant Analytics** | Food & Hospitality | 148 rows, 7 columns | Do online orders increase customer reviews? How do dining costs compare across restaurant types? | [View Project](ZOMATO_DATA_SCIENCE/) |

---

## 2. Quick Project Summaries

### 1. YouTube India Trending Video Analysis
- **Goal:** Find out what makes videos trend in India and how engagement metrics grow with reach.
- **Data:** 33,089 unique trending records (after removing 4,263 duplicates) across 29 total features (14 original, 15 engineered).
- **Key Findings:**
  - **72-Hour Window:** 86.2% of videos trend within 1 to 3 days of upload (median is 2 days). Videos rarely trend after day 4.
  - **Best Days to Upload:** Thursday, Friday, and Saturday produce 47.7% of all trending videos and get the highest average views.
  - **Engagement Link:** Likes and comments have a very strong link (r = 0.92). Likes and views link at r = 0.82.
  - **Tag Usage:** 91.8% of trending videos use 6 or more tags (Medium to High tag count).

### 2. Student Performance in Exams
- **Goal:** Understand which factors (study preparation, parental education, lunch type) affect exam scores in Math, Reading, and Writing.
- **Data:** 1,000 student records with 5 demographic background details and 3 exam scores.
- **Key Findings:**
  - **Reading and Writing Link:** Reading and writing scores have a near-perfect link (r = 0.95). Students who read well almost always write well.
  - **Prep Course Value:** Taking a test preparation course gives students about +10 extra points in writing and +5 to 7 points in math and reading.
  - **Lunch Impact:** Students with standard lunch score 11 to 14 points higher than students on subsidized lunch.
  - **Gender Patterns:** Female students lead in reading and writing, while male students score higher in math.

### 3. Zomato Restaurant Analytics
- **Goal:** Study dining and food delivery patterns across restaurants in Bangalore.
- **Data:** 148 restaurant profiles covering online ordering, table bookings, customer ratings, cost for two, and food types.
- **Key Findings:**
  - **Online Delivery:** Restaurants offering online orders get more customer reviews and higher vote counts.
  - **Cost vs. Rating:** Dine-out places and buffet restaurants cost more for two people than quick delivery outlets.

---

## 3. Step-by-Step Data Analysis Workflow

### Analytical Pipeline Flow

```
[ Raw Dataset ]
       │
       ▼
[ 1. Environment & Display Setup ] ───────── (pd.set_option, whitegrid theme, figure dimensions)
       │
       ▼
[ 2. Safe Data Loading & Path Handling ] ─── (os.path.exists checks, os.listdir verification)
       │
       ▼
[ 3. Column Header Standardization ] ─────── (lowercase, strip whitespace, snake_case conversion)
       │
       ▼
[ 4. Data Quality & Deduplication ] ──────── (remove duplicate rows, fill missing text)
       │
       ▼
[ 5. Datetime Parsing & Normalization ] ──── (pd.to_datetime, .dt.normalize, extract hour & weekday)
       │
       ▼
[ 6. Outlier Capping (99th Percentile) ] ─── (np.percentile, .clip(upper=limit) without dropping rows)
       │
       ▼
[ 7. Feature Engineering ] ───────────────── (pd.cut speed bands, pd.qcut quartiles, tag counting)
       │
       ▼
[ 8. Univariate & Bivariate EDA ] ────────── (histograms with KDE, countplots, category boxplots)
       │
       ▼
[ 9. Multivariate Analysis ] ─────────────── (correlation matrix, 2D pivot heatmaps, pairplots)
       │
       ▼
[ 10. Business Insights & Recommendations ] ─ (data-backed takeaways, upload strategy, engagement advice)
```

**Quick Pipeline Summary:**  
`Raw Data` ➔ `Setup & Path Checks` ➔ `Cleaning & Deduplication` ➔ `Datetime Parsing` ➔ `99th % Outlier Capping` ➔ `Feature Engineering` ➔ `Univariate EDA` ➔ `Bivariate EDA` ➔ `Multivariate Analysis` ➔ `Actionable Takeaways`

---

Here is the exact step-by-step process followed across these projects, explaining what was done and why each step is necessary in real-world data analysis:

### Step 1: Notebook & Display Setup
Before loading any file, we configure Pandas and visualization settings so data is easy to inspect and numbers do not get cut off.

| Setting / Code | What It Does | Why We Use It |
|---|---|---|
| `pd.set_option("display.max_columns", None)` | Shows every column in the table | By default, Pandas hides middle columns with dots (`...`). Setting `None` lets us see all 29 columns. |
| `pd.set_option("display.max_colwidth", 120)` | Expands text width to 120 characters | Stops long titles and descriptions from getting truncated. |
| `pd.set_option("display.float_format", "{:,.2f}".format)` | Adds commas and rounds to 2 decimals | Shows readable numbers like `12,800,000.00` instead of scientific notation like `1.28e+07`. |
| `sns.set_theme(style='whitegrid')` | Adds a soft light-gray grid behind charts | Makes it easy to match bar heights and data points to the numbers on the side axis. |
| `plt.rcParams['figure.figsize'] = (10, 5)` | Sets plot width to 10 and height to 5 inches | Makes all exported charts the exact same professional size. |
| `plt.rcParams['font.sans-serif'] = 'Segoe UI'` | Changes the chart font to Segoe UI | Uses a modern, clean font instead of the old blocky default font. |

---

### Step 2: Safe File Loading
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
- **Why use `import os` and `os.path.exists()`:** If you move notebooks between folders or share code with someone else, file paths can change. Checking with `os.path.exists()` prevents your code from crashing with a `FileNotFoundError`.
- **Why use `os.listdir()`:** Shows you the files inside a folder so you can verify file names before reading them.

---

### Step 3: Cleaning Column Names
```python
df.columns = [col.strip().lower().replace(" ", "_") for col in df.columns]
print(f"Dataset Shape: {df.shape}")
df.head(3)
```
- **Why we do this:** Raw datasets often have accidental spaces (like `' Views '`) or mixed capital letters (`'Publish Time'`).
- **How it works:**
  - `.strip()` removes leading and trailing spaces.
  - `.lower()` converts all letters to lowercase.
  - `.replace(" ", "_")` swaps spaces with underscores (`snake_case`).
- **Result:** You can easily type `df['views']` without getting unexpected `KeyError` errors.

---

### Step 4: Quality Checks & Removing Duplicates
- **Check shape:** `df.shape` shows total rows and columns.
- **Find duplicate rows:** `df.duplicated().sum()` found **4,263 duplicate records**.
- **Drop duplicates:** `df.drop_duplicates(inplace=True)` trimmed data from 37,352 rows down to **33,089 clean unique rows**.
- **Handle missing text:** Missing descriptions (561 rows) were filled with `'No description'` using `df['description'].fillna('No description', inplace=True)`.

---

### Step 5: Fixing Date and Time Columns
- Converted `trending_date` text into a real date object using `pd.to_datetime()`.
- Parsed `publish_time` timestamp to create 3 useful time features:
  - `df['publish_date'] = df['publish_time'].dt.normalize()`: Removes the hours/minutes/seconds so we can do date-level math.
  - `df['publish_hour'] = df['publish_time'].dt.hour`: Gives the hour of upload (0 to 23).
  - `df['publish_weekday'] = df['publish_time'].dt.day_name()`: Gives the day name (Monday to Sunday).

---

### Step 6: Outlier Capping at the 99th Percentile
- **The Issue:** A few mega-viral videos have 10 to 12 Crore views, while most trending videos have 2 to 3 Lakhs. This extreme spread stretches charts and flattens normal data.
- **The Solution (Capping instead of Deleting):**
  - If we delete these rows, we lose real viral video examples.
  - Instead, we find the 99th percentile cutoff using `np.percentile(df['views'], 99)` (1.28 Crore).
  - We cap values above this ceiling using `df['views'].clip(upper=limit)`. Any value higher than 1.28 Crore becomes 1.28 Crore. All 33,089 rows stay in the dataset and charts stay readable.

---

### Step 7: Complete Feature Engineering (All 15 Derived Features)

Here is every single new feature engineered in the project, what code created it, and why:

| Feature Name | Data Type | Code Used to Create | Why We Created It |
|---|---|---|---|
| `days_to_trend` | Integer | `(df['trending_date'] - df['publish_date']).dt.days` | Measures how many days it took a video to reach the trending page. |
| `trend_speed_band` | Categorical | `pd.cut(df['days_to_trend'], bins=[-1, 0, 3, 7, 365], labels=['Same Day', '1 to 3 Days', '4 to 7 Days', 'More than 7 Days'])` | Groups videos into fixed day intervals to find the critical viral window. |
| `views_quartile` | Categorical | `pd.qcut(df['views'], q=4, labels=['Q1', 'Q2', 'Q3', 'Q4'])` | Splits videos into 4 equal groups (25% each) to compare low vs high performers fairly. |
| `tag_count` | Integer | `df['tags'].str.count(r'\|') + 1` | Counts how many tags the creator added by counting pipe symbols (`|`). |
| `tag_use_level` | Categorical | `pd.cut(tag_count, bins=[-1, 0, 5, 15, 100], labels=['No Tags', 'Low', 'Medium', 'High'])` | Classifies creator tagging into simple tiers. |
| `like_rate` | Float | `df['likes'] / df['views']` | Measures viewer reaction: how many likes come from 100 views. |
| `comment_rate` | Float | `df['comment_count'] / df['views']` | Measures viewer discussion: how many comments come from 1,000 views. |
| `has_description` | Binary | `np.where(df['description'] != 'No description', 1, 0)` | A 1/0 flag to test if having a description boosts reach. |
| `title_length` | Integer | `df['title'].str.len()` | Title character count to check if short or long titles perform better. |
| `description_length`| Integer | `df['description'].str.len()` | Description character count. |
| `publish_date` | Datetime | `df['publish_time'].dt.normalize()` | Upload date with time reset to midnight (00:00:00). |
| `publish_hour` | Integer | `df['publish_time'].dt.hour` | Upload hour (0 to 23) to identify peak publishing times. |
| `publish_weekday` | Categorical | `df['publish_time'].dt.day_name()` | Day of the week the video was published (Monday to Sunday). |
| `comments_status` | Categorical | `np.where(df['comments_disabled'], 'Disabled', 'Enabled')` | Clear text label for chart legends. |
| `ratings_status` | Categorical | `np.where(df['ratings_disabled'], 'Disabled', 'Enabled')` | Clear text label for like visibility charts. |

---

## 4. Quick Revision Guide: Functions, Methods & Code Snippets

Use these tables to quickly review key Pandas, NumPy, and Seaborn functions during interview prep or revision.

### Core Data Science & Feature Engineering Methods

| Method / Concept | What It Does | When to Use It | Code Example |
|---|---|---|---|
| `pd.cut()` | Cuts continuous numbers into **custom fixed ranges** that you set. | When you have specific business rules for cutoff points (like 0 days, 1-3 days, 4-7 days). Group sizes can be different. | `df['speed'] = pd.cut(df['days'], bins=[-1, 0, 3, 7, 365], labels=['Same Day', '1-3 Days', '4-7 Days', '7+ Days'])` |
| `pd.qcut()` | Splits numbers into **equal-sized groups (quartiles)**. | When you want every group to have the exact same number of rows (like 4 quartiles with 25% of rows each). | `df['views_tier'] = pd.qcut(df['views'], q=4, labels=['Q1', 'Q2', 'Q3', 'Q4'])` |
| `np.percentile()` | Finds the cutoff number where a specific percentage of data falls below. | When setting outlier limits (e.g. 99% of videos have fewer than 1.28 Crore views). | `cutoff = np.percentile(df['views'], 99)` |
| `.clip()` | Caps values at an upper or lower limit without deleting rows. | When handling extreme viral outliers without losing any rows from the dataset. | `df['views_capped'] = df['views'].clip(upper=cutoff)` |
| `np.where()` | A fast IF-ELSE condition (`if condition then A else B`). | When creating 1/0 binary flags or category labels based on a condition. | `df['has_desc'] = np.where(df['description'] != 'No description', 1, 0)` |
| `.describe()` | Calculates 8 key summary numbers for a column. | When you want to see count, mean, standard deviation, min, 25%, median (50%), 75%, and max all at once. | `print(df['views'].describe())` |
| `.median()` | Finds the middle number (50th percentile) in sorted data. | When data is heavily skewed by a few extreme viral values. The median shows what a typical video gets. | `median_likes = df['likes'].median()` |
| `.quantile()` | Finds the cutoff number at any percentage point (e.g. 0.75 for 75%). | When checking where the top 25% or bottom 25% boundary sits. | `q3_likes = df['likes'].quantile(0.75)` |
| `.value_counts(normalize=True)` | Calculates the percentage share of each category. | When you want to see what percentage of videos were uploaded on each weekday. | `pcts = df['publish_weekday'].value_counts(normalize=True) * 100` |
| `.dt.normalize()` | Removes hours, minutes, and seconds from a datetime. | When you want just the date (set to midnight 00:00:00) so you can do day-level arithmetic. | `df['pub_date'] = df['publish_time'].dt.normalize()` |
| `.str.count()` | Counts how many times a character shows up in text. | When counting pipe symbols (`|`) in tags to calculate total tag count. | `df['tag_count'] = df['tags'].str.count(r'\\|') + 1` |

---

### Comparison: `pd.cut()` vs. `pd.qcut()`

| Feature | `pd.cut()` (Value-Based Binning) | `pd.qcut()` (Quantile-Based Binning) |
|---|---|---|
| **How Bins are Created** | Based on **exact number cutoffs** you choose (e.g. 0, 3, 7 days) | Based on **equal sample counts** (splits data into equal percentage groups) |
| **Row Count in Each Bin** | **Unequal** (one bin can hold 86% of data, another only 1%) | **Equal** (every bin has the exact same 25% of rows for quartiles) |
| **When to Choose** | When real-world categories have clear day or age boundaries | When you want to rank performance fairly into Low, Medium, and High groups |
| **Project Example** | `pd.cut(df['days_to_trend'], bins=[-1, 0, 3, 7, 365], ...)` | `pd.qcut(df['views'], q=4, labels=['Q1', 'Q2', 'Q3', 'Q4'])` |

---

### Visualization Setup Reference

| Function / Setting | What It Does | Why We Use It | Example Code |
|---|---|---|---|
| `sns.set_theme(style='whitegrid')` | Adds light gray grid lines behind charts | Helps viewers easily trace bars and dots to the numbers on the side axis. | `sns.set_theme(style='whitegrid')` |
| `plt.rcParams['figure.figsize'] = (10, 5)` | Sets chart size to 10 inches wide by 5 inches tall | Makes every chart exported have a uniform, clean shape. | `plt.rcParams['figure.figsize'] = (10, 5)` |
| `plt.rcParams['font.sans-serif'] = 'Segoe UI'` | Uses Segoe UI font for all titles and labels | Makes charts look modern, clean, and polished. | `plt.rcParams['font.sans-serif'] = 'Segoe UI'` |
| `sns.histplot(kde=True)` | Plots a histogram with a smooth density curve | Shows where most numbers cluster and reveals skewness. | `sns.histplot(df['views'], bins=30, kde=True)` |
| `sns.countplot()` | Plots a bar chart of category counts | Shows how many items fall into each category (e.g. videos per weekday). | `sns.countplot(x='publish_weekday', data=df)` |
| `sns.boxplot()` | Draws a box showing Median, 25th-75th percentile, and outliers | Best way to compare number spreads across different groups. | `sns.boxplot(x='views_quartile', y='likes', data=df)` |
| `sns.heatmap()` | Draws a color-coded correlation or pivot table grid | Fastest way to spot strong positive (red) or negative (blue) relationships. | `sns.heatmap(df.corr(), annot=True, cmap='coolwarm')` |
| `sns.pairplot()` | Generates a grid of scatter plots for numeric columns | Lets you look at connections between multiple features at once. | `sns.pairplot(df[['views', 'likes', 'comment_count']])` |
| `plt.tight_layout()` | Automatically adjusts spacing around the chart | Prevents axis labels, titles, and ticks from getting cut off at the image edge. | `plt.tight_layout()` |
| `plt.savefig()` | Saves the chart to disk as a high-resolution image | Exports charts cleanly at 300 DPI for reports and documentation. | `plt.savefig('../images/views_dist.png', dpi=300, bbox_inches='tight')` |

---

## 5. Practical Code Examples for Quick Revision

### Example 1: Explaining Median vs. 75th Percentile Spread
```python
# Check likes spread: compare median (50th percentile) with 75th percentile (Q3)
likes_median = df['likes'].median()
likes_q3 = df['likes'].quantile(0.75)

print(f"75% of trending videos have {likes_q3:,.0f} likes or fewer, while the median is {likes_median:,.0f}.")
```
- **What this means:** The median is 2,757 likes, and 75% of videos have 12,011 likes or fewer. But the maximum like count is over 423,000. This shows extreme positive skewness. A few viral mega-hits pull the average up, while a typical trending video succeeds with moderate, solid engagement numbers.

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
- **What this means:** Deleting viral videos removes real, important data. Instead, capping values above 1.28 Crore at 1.28 Crore preserves all 33,089 records while keeping charts readable and easy to interpret.

---

### Example 3: Full Distribution Chart with High-Resolution Export
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
- **What this means:** Using `.describe()` gives you the exact numbers, and `sns.histplot()` shows the visual shape and curve. You get both numerical proof and a clear visual together.

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
