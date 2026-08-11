# YouTube Trending Videos EDA

## Overview
This project presents an end-to-end exploratory data analysis (EDA) of the India subset of the YouTube Trending Videos dataset from Kaggle.

The analysis focuses on understanding trending video behavior, cleaning the raw dataset, engineering useful features, and identifying patterns related to views, likes, comments, upload timing, and trend speed.

## Objective
The main goals of this project are to:

- understand the dataset structure and quality
- clean and prepare the data for analysis
- engineer useful engagement and timing features
- analyze patterns behind trending videos
- summarize insights that can support content strategy decisions

## Dataset Information
- **Dataset Name:** YouTube Trending Videos Dataset
- **Source:** [Kaggle - YouTube Trending Videos](https://www.kaggle.com/datasnaek/youtube-new)
- **Region Used in This Project:** India
- **File Used:** `INvideos.csv`

The dataset includes information such as:
- video title
- channel title
- category ID
- publish time
- trending date
- views
- likes
- dislikes
- comment count
- tags
- description
- comments disabled flag
- ratings disabled flag
- video removed/error flag

## Tools and Libraries
- Python
- pandas
- numpy
- matplotlib
- seaborn
- Jupyter Notebook

## Project Workflow
1. Defined the analysis objective and business questions
2. Imported libraries and configured notebook display settings
3. Loaded the dataset and cleaned column names
4. Reviewed dataset structure using `head()`, `shape`, `info()`, and `describe()`
5. Checked and handled missing values
6. Removed duplicate rows
7. Converted date columns into datetime format
8. Created new features such as:
   - `has_description`
   - `days_to_trend`
   - `publish_hour`
   - `publish_weekday`
   - `comments_status`
   - `ratings_status`
   - `video_status`
   - `tag_use_level`
   - `trend_speed_band`
   - `views_quartile`
   - `title_length`
   - `description_length`
   - `tag_count`
   - `like_rate`
   - `comment_rate`
9. Capped outliers in engagement columns using percentile-based clipping
10. Performed univariate, bivariate, and multivariate analysis
11. Summarized findings and recommendations

## Key Insights
- **4,263 duplicate rows were removed during cleaning.**
- **Only `description` had missing values, and those rows were preserved using a placeholder value.**
- **The median time to trend is 2 days, and 75% of videos trend within 3 days.**
- **Friday has the highest number of published trending videos, while Sunday has the lowest.**
- **Most trending videos have ratings enabled.**
- **High Tag Use is more common than Medium Tag Use among trending videos.**
- **Views, likes, dislikes, and comment counts show strong positive relationships.**
- **Higher-view quartiles receive much stronger average engagement than lower-view quartiles.**

## Recommendations
- Focus on the first 48 to 72 hours after upload because most videos trend within 3 days.
- Test Thursday, Friday, and Saturday uploads for stronger trending potential.
- Keep ratings enabled by default.
- Use complete metadata, including strong titles, descriptions, and relevant tags.
- Track engagement quality using `like_rate` and `comment_rate`, not only raw views.


----------------------



## Visuals 
### 1. Distribution of Views
![Distribution of Views](images/views_distribution.png)

### 2. Distribution of Days to Trend
![Distribution of Days to Trend](images/07_04_days_to_trend_distribution.png)

### 3. Average Views by Trend Speed Band
![Average Views by Trend Speed Band](images/08_02_views_by_trend_speed.png)

### 4. Average Views by Publish Weekday
![Average Views by Publish Weekday](images/08_05_views_by_weekday.png)

### 5. Correlation Heatmap
![Correlation Heatmap](images/09_01_correlation_heatmap.png)