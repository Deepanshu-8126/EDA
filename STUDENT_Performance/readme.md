# Student Exam Performance Analysis

An Exploratory Data Analysis (EDA) project on 1,000 high school students to understand what factors actually drive exam scores and identify which students need the most academic support.

---

## 1. Project Overview

In this project, I analyzed exam performance across Math, Reading, and Writing using a dataset of 1,000 students. The goal was to look beyond just marks and understand the real factors behind performance gaps — such as socio-economic background (lunch type), test preparation, parental education levels, and gender. 

By analyzing these relationships, schools and educators can identify at-risk students early and design targeted academic interventions rather than treating all students with a one-size-fits-all approach.

---

## 2. Key Questions This Project Answers

- **Does test preparation actually help?** How much of a score boost does completing a test prep course give?
- **How big is the socio-economic gap?** What is the score difference between students on standard lunch vs. free/reduced lunch?
- **Where do gender differences lie?** Are boys or girls systematically ahead in specific subjects?
- **Does parental education matter?** How strongly does a parent's education level reflect in a student's marks?
- **Which subjects are connected?** If a student struggles in Reading, are they also likely to struggle in Writing or Math?

---

## 3. Dataset Summary

The dataset contains **1,000 student records** from Kaggle with 8 demographic and exam features, along with 3 engineered columns created during analysis.

| Column | Type | Description |
|---|---|---|
| `gender` | Categorical | `female` or `male` |
| `race/ethnicity` | Categorical | Anonymized ethnic group (`group A` to `group E`) |
| `parental level of education` | Categorical | Parent's highest education level (`some high school` to `master's degree`) |
| `lunch` | Categorical | Lunch type: `standard` or `free/reduced` (proxy for socio-economic background) |
| `test preparation course` | Categorical | `completed` or `none` |
| `math score` | Numeric | Score out of 100 |
| `reading score` | Numeric | Score out of 100 |
| `writing score` | Numeric | Score out of 100 |
| `total` | Numeric | Combined score across all 3 subjects (out of 300) *(Engineered)* |
| `average` | Numeric | Mean score across the 3 subjects (0 - 100) *(Engineered)* |
| `grade` | Categorical | Letter grade: `A` (80+), `B` (70-79), `C` (60-69), `D` (50-59), `F` (<50) *(Engineered)* |

**Data Source:** [Kaggle - Students Performance in Exams](https://www.kaggle.com/datasets/spscientist/students-performance-in-exams)

---

## 4. What Was Done in This Project

1. **Data Cleaning & Verification:**
   - Checked for missing values: Verified 0 null values across all columns.
   - Checked for duplicates: Verified 0 duplicate rows in the 1,000 records.
   - Standardized column names and verified numeric ranges (all scores fall within 0-100).
2. **Feature Engineering:**
   - Calculated `total` score (sum of Math, Reading, Writing).
   - Calculated `average` score to measure overall performance on a standard 0-100 scale.
   - Assigned letter `grade` (A to F) based on standard academic grading tiers.
3. **Univariate Analysis:**
   - Analyzed score distributions across subjects (overall mean score: 67.8).
   - Looked at demographic counts (518 females vs. 482 males; 645 standard lunch vs. 355 free/reduced lunch).
4. **Bivariate Comparisons:**
   - Compared average scores across test prep status, lunch types, parental education, and ethnicity.
5. **Correlation & Multivariate Analysis:**
   - Built a correlation heatmap across all subjects and overall average score.
   - Generated a multi-subject pairplot colored by gender to examine linear patterns.
   - Analyzed multivariate interactions (Gender + Test Prep, Parental Education + Lunch Type).
6. **Student Profiling:**
   - Profiled the Top 10% highest-scoring cohort against the Bottom 10% at-risk cohort.

---

## 5. Key Insights & Findings

### 1. Lunch Type is the Single Strongest Factor
- Students with **standard lunch** averaged **70.8**, while students with **free/reduced lunch** averaged **62.2** — a clear gap of **8.6 points overall**.
- In **Math**, the difference is even wider: standard lunch students scored **70.0** vs **58.9** for free/reduced lunch (over **11 points difference**).
- The free/reduced lunch cohort has the largest share of low performers and course failures, proving that nutrition and socio-economic support heavily influence classroom focus and outcomes.

---

### 2. Test Preparation Course Adds 7.6 to 10 Points
- Students who completed test preparation averaged **72.7**, compared to **65.0** for those who did not take any prep (an overall gain of **+7.6 points**).
- The biggest gain was in **Writing**, where prepared students scored **74.4** vs **64.5** (+9.9 points higher).
- This improvement was consistent across all ethnic groups and income levels.

---

### 3. Gender Differences Depend on the Subject
- **Reading:** Female students scored **72.6** vs **65.5** for male students (+7.1 points).
- **Writing:** Female students scored **72.5** vs **63.3** for male students (+9.2 points).
- **Math:** Male students scored **68.7** vs **63.6** for female students (+5.1 points).
- Overall average: Female students averaged **69.6** vs **65.8** for male students.

![Gender Distribution](images/univariate_gender.png)

---

### 4. Parental Education Has a Direct Linear Relationship with Scores
- Students whose parents have higher education consistently achieve better average scores:
  - **Master's degree:** 73.6 average
  - **Bachelor's degree:** 71.9 average
  - **Associate's degree:** 69.6 average
  - **Some college:** 68.5 average
  - **Some high school:** 65.1 average
  - **High school:** 63.1 average
- There is a **10.5 point gap** between students of Master's degree parents and students whose parents finished only high school.

---

### 5. What the Correlation Heatmap Shows
The correlation heatmap reveals how strongly individual exam scores are tied to each other:
- **Reading and Writing (r = 0.95):** This is the strongest link in the entire dataset. A student who scores well in reading comprehension almost always scores equally well in writing.
- **Math and Reading (r = 0.82) & Math and Writing (r = 0.80):** While math requires distinct problem-solving skills, it is still strongly linked to language skills (comprehending questions, word problems).
- **Key Takeaway:** Academic performance is holistic. Weakness in one subject strongly signals risk in others, so remedial support should address fundamental learning habits rather than viewing math or writing in total isolation.

![Math Score vs Reading Score](images/maths_score_vs_readingscore.png)

---

### 6. What the Pairplot Shows (Multi-Subject Comparison by Gender)
The pairplot compares Math, Reading, and Writing scores against each other simultaneously, colored by gender:
- **Linear Positive Trends:** Every scatter plot shows an upward linear trend — higher scores in one subject consistently track higher scores in the others.
- **Reading vs. Writing Tight Cluster:** The scatter points between Reading and Writing form the narrowest, cleanest diagonal cluster, visually confirming their 0.95 correlation.
- **Gender Separation:**
  - Female points visibly cluster higher up on the Reading and Writing axes.
  - Male points visibly shift higher along the Math axis.
- **Diagonal Distribution Curves:** Show that Math scores are slightly more dispersed, while Reading and Writing follow very similar bell curves.

![Pairplot by Gender](images/pairplot.png)

---

### 7. What Multivariate Analysis Shows (Combining Factors)
By analyzing multiple variables together, several deeper patterns emerge:
- **Gender + Test Prep:** Completing test prep raises marks for both genders, but females who complete test prep achieve the highest overall average in the entire dataset (**74.4**).
- **Parental Education + Lunch Type:** Across every single parental education bracket, students with standard lunch outperform those with free/reduced lunch. Even for parents with college degrees, students with free lunch score lower, proving that nutritional security is an independent performance driver.
- **Ethnicity + Test Prep:** The positive effect of test preparation holds true across all 5 ethnic groups (Group A through Group E), showing that structured preparation benefits all student backgrounds equally.

---

### 8. Overall Score Distribution
- Overall student scores follow a clean bell-shaped normal distribution with an average of **67.8**.
- While most students comfortably pass, there is a clear bottom tail of students scoring below 50 who need early tutoring.

![Average Score Distribution](images/univariate_average_score.png)

---

### 9. Top 10% vs. Bottom 10% Student Profile

| Student Group | Common Profile | Average Score Range |
|---|---|---|
| **Top 10% Scorers** | Female + Standard Lunch + Completed Test Prep + College/Master's Educated Parents | 85 - 100 |
| **Bottom 10% Scorers (At-Risk)** | Free/Reduced Lunch + No Test Prep + High School Educated Parents | Below 50 (High fail rate) |

---

## 6. Project Takeaways at a Glance

When summarizing this project, these are the core numbers and conclusions to highlight:

- **Overall Average:** The average score across all 1,000 students is **67.8**.
- **#1 Determinant:** **Lunch type** created the largest score gap — standard lunch students scored **8.6 points higher overall** (and 11.1 points higher in Math).
- **Test Prep Impact:** Taking the prep course delivered an immediate **+7.6 to +10 point boost**, especially in writing.
- **Gender Dynamics:** Girls outscored boys in Reading (+7.1 pts) and Writing (+9.2 pts); Boys outscored girls in Math (+5.1 pts).
- **Strongest Correlation:** **Reading and Writing have an r = 0.95 link**, showing language skills develop together.
- **Parental Education Gap:** A **10.5 point gap** separates students of Master's degree parents (73.6) from High School parents (63.1).
- **At-Risk Target:** The most vulnerable students are those on free lunch who have not taken test prep and whose parents completed only high school.

---

## 7. Actionable Recommendations

| # | Recommendation | Target Cohort | Reason | Expected Impact |
|---|---|---|---|---|
| **1** | **Subsidized / Free Meal Programs** | Free/Reduced lunch students | 11-point math gap between lunch tiers. | Better nutrition and concentration in morning classes. |
| **2** | **Free In-School Test Prep Courses** | Students without test prep | Prep completion adds ~8 to 10 points across subjects. | Directly lifts overall passing rates without extra cost to families. |
| **3** | **Subject-Specific Support by Gender** | Girls (Math) & Boys (Writing) | Boys trail by 9.2 pts in writing; girls trail by 5.1 pts in math. | Bridges the subject-specific gender gaps. |
| **4** | **Extra After-School Tutoring for First-Gen Students** | Students of high-school educated parents | Score gap of 10.5 points compared to degree-holding families. | Provides study support that may not be available at home. |
| **5** | **Early Warning System for At-Risk Cohorts** | Free lunch + No prep cohort | Highest concentration of grades below 50. | Identifies struggling students before final exams. |

---

## 8. Repository Structure

```
STUDENT_Performance/
|-- data/
|   \-- StudentsPerformance.csv            # Original dataset (1,000 rows, 8 features)
|-- images/
|   |-- maths_score_vs_readingscore.png    # Scatter plot (Reading vs Math correlation)
|   |-- pairplot.png                       # Pairplot colored by gender
|   |-- univariate_average_score.png       # Score distribution histogram
|   \-- univariate_gender.png              # Gender distribution countplot
|-- notebooks/
|   \-- student_performance_analysis.ipynb # Complete analysis & visualizations notebook
\-- readme.md                              # Project documentation
```

---

## 9. Tools Used

- **Python** (Pandas, NumPy) — Data loading, cleaning, and feature engineering
- **Matplotlib & Seaborn** — Data visualizations, distribution plots, and correlation heatmaps
- **Jupyter Notebook** — Interactive analysis environment

---

## 10. Author

**Deepanshu Kapri**
