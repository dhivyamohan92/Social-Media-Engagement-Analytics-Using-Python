# Social-Media-Engagement-Analytics-Using-Python



## 📌 Project Overview

Social media platforms generate large volumes of engagement data such as **likes, comments, shares, impressions, watch time, follower count, and engagement rate**.

This project analyzes a social media dataset containing **5,000 records and 19 columns** to identify patterns in content performance, user behavior, posting time, device usage, and sentiment.

The project demonstrates an end-to-end **Python Data Analytics workflow**, including data cleaning, data wrangling, exploratory data analysis, statistical analysis, visualization, and insight generation.

---

## 🎯 Project Objectives

The main objectives of this project are to:

* Import and inspect the social media dataset.
* Identify and handle missing values.
* Detect duplicate records.
* Correct and standardize data types and categorical values.
* Extract hashtag counts as a new feature.
* Explore numerical and categorical variables.
* Analyze correlations between numerical variables.
* Perform `groupby()` analysis.
* Combine DataFrames using `merge()`.
* Calculate descriptive statistics.
* Create visualizations using Matplotlib and Seaborn.
* Create interactive visualizations using Plotly.
* Identify meaningful patterns in social media engagement.

---

## 📂 Dataset

**Dataset:** `social_media_engagement_5000.csv`

**Size:** 5,000 rows × 19 columns

### Dataset Columns

| Column             | Description                       |
| ------------------ | --------------------------------- |
| `index`            | Record index                      |
| `user_id`          | Unique user identifier            |
| `age`              | User age                          |
| `gender`           | User gender                       |
| `country`          | User country                      |
| `post_id`          | Unique post identifier            |
| `post_type`        | Type of post                      |
| `post_category`    | Content category                  |
| `likes`            | Number of likes                   |
| `comments`         | Number of comments                |
| `shares`           | Number of shares                  |
| `watch_time_sec`   | Watch time in seconds             |
| `impression_count` | Number of impressions             |
| `posted_at`        | Date and time of posting          |
| `follower_count`   | Number of followers               |
| `is_verified`      | Account verification status       |
| `device_type`      | Device used                       |
| `sentiment`        | Post sentiment                    |
| `hashtags`         | Hashtags associated with the post |
| `engagement_rate`  | Post engagement rate              |

---

## 🛠️ Technologies Used

* **Python**
* **Pandas**
* **NumPy**
* **Matplotlib**
* **Seaborn**
* **Plotly**
* **Jupyter Notebook / Google Colab**

---

# 🔹 Project Workflow

```text
Data Import
     ↓
Data Inspection
     ↓
Data Cleaning
     ↓
Data Transformation
     ↓
Exploratory Data Analysis
     ↓
Data Wrangling
     ↓
Statistical Analysis
     ↓
Visualization
     ↓
Insight Generation
```

---

# 🧹 Task 1 – Data Import & Setup

The CSV dataset was imported using Pandas.

```python
import pandas as pd
import numpy as np

df = pd.read_csv("social_media_engagement_5000.csv")
```

Initial dataset inspection was performed using:

```python
df.head()
df.tail()
df.shape
df.columns
df.info()
df.dtypes
```

The `posted_at` column was converted into datetime format:

```python
df["posted_at"] = pd.to_datetime(df["posted_at"])
```

---

# 🧹 Task 2 – Data Cleaning

## Missing Value Detection

Missing values were identified using:

```python
df.isnull().sum()
```

Initial missing values were found in:

* `age`
* `gender`
* `likes`
* `comments`
* `shares`
* `sentiment`

## Missing Value Treatment

Numerical columns were filled using the median:

```python
for col in ["age", "likes", "comments", "shares"]:
    df[col] = df[col].fillna(df[col].median())
```

Categorical columns were filled using the mode:

```python
for col in ["gender", "sentiment"]:
    df[col] = df[col].fillna(df[col].mode()[0])
```

## Duplicate Handling

Duplicate records were checked using:

```python
df.duplicated().sum()
```

**Result:** `0` duplicate records were found.

## Data Formatting

Numerical columns were converted to integer format:

```python
for col in ["age", "likes", "comments", "shares"]:
    df[col] = df[col].astype(int)
```

Sentiment values were standardized:

```python
df["sentiment"] = df["sentiment"].str.strip().str.lower()
```

## Feature Cleaning – Hashtag Count

A new `hashtag_count` feature was created:

```python
df["hashtag_count"] = df["hashtags"].fillna("").apply(
    lambda x: len(x.split())
)
```

---

# 🔍 Task 3 – Exploratory Data Analysis

## Categorical Analysis

The following Pandas functions were used:

```python
df["gender"].unique()
df["gender"].nunique()
df["gender"].value_counts()
```

Similar analysis was performed for:

* Country
* Post type
* Post category
* Device type
* Sentiment
* Verification status

## Correlation Analysis

A correlation matrix was generated for numerical variables:

```python
df.select_dtypes(include="number").corr()
```

This helped identify relationships between variables such as:

* Likes
* Comments
* Shares
* Watch time
* Impressions
* Followers
* Engagement rate

---

# 📊 Task 4 – Data Wrangling

## Age Group Feature

Age groups were created using `pd.cut()`:

```python
bins = [0, 18, 25, 35, 45, 55, 65, 100]

labels = [
    "<18",
    "18-25",
    "26-35",
    "36-45",
    "46-55",
    "56-65",
    "65+"
]

df["age_group"] = pd.cut(
    df["age"],
    bins=bins,
    labels=labels
)
```

## Groupby Analysis

Average engagement was calculated by different dimensions.

### Post Type

```python
df.groupby("post_type")["engagement_rate"].mean()
```

### Country

```python
df.groupby("country")["engagement_rate"].mean()
```

### Content Category

```python
df.groupby("post_category")["engagement_rate"].mean()
```

### Sentiment

```python
df.groupby("sentiment")["engagement_rate"].mean()
```

### Device

```python
df.groupby("device_type")["watch_time_sec"].mean()
```

---

# 🔗 DataFrame Combining

`merge()` was used to combine analytical summaries:

```python
likes_summary = df.groupby(
    "post_type"
)["likes"].mean().reset_index(name="avg_likes")

engagement_summary = df.groupby(
    "post_type"
)["engagement_rate"].mean().reset_index(
    name="avg_engagement"
)

final_summary = pd.merge(
    likes_summary,
    engagement_summary,
    on="post_type"
)
```

---

# 📈 Task 5 – Statistical Analysis

Descriptive statistics were calculated for:

* Likes
* Comments
* Shares
* Watch time
* Engagement rate
* Follower count

The analysis included:

* Mean
* Median
* Mode
* Standard deviation
* Variance
* 25th percentile
* 50th percentile
* 75th percentile
* 90th percentile
* Skewness
* Kurtosis

Example:

```python
columns = [
    "likes",
    "comments",
    "shares",
    "watch_time_sec",
    "engagement_rate",
    "follower_count"
]

stats = pd.DataFrame({
    "Mean": df[columns].mean(),
    "Median": df[columns].median(),
    "Mode": df[columns].mode().iloc[0],
    "Standard Deviation": df[columns].std(),
    "Variance": df[columns].var(),
    "25th Percentile": df[columns].quantile(0.25),
    "50th Percentile": df[columns].quantile(0.50),
    "75th Percentile": df[columns].quantile(0.75),
    "90th Percentile": df[columns].quantile(0.90),
    "Skewness": df[columns].skew(),
    "Kurtosis": df[columns].kurt()
})

stats.round(2)
```

---

# 📊 Task 6 – Data Visualization

## Matplotlib

The project includes the following Matplotlib visualizations:

1. **Scatter Plot** – Likes vs Impressions
2. **Line Chart** – Daily Engagement Trend
3. **Bar Chart** – Posts by Category
4. **Pie Chart** – Gender Distribution
5. **Histogram** – Age Distribution
6. **Box Plot** – Engagement Rate Distribution
7. **Bar Chart** – Average Engagement by Post Type
8. **Bar Chart** – Average Watch Time by Device

---

## Seaborn

The following Seaborn visualizations were created:

1. **Count Plot** – Post Type
2. **Bar Plot** – Average Likes by Category
3. **Violin Plot** – Followers vs Sentiment
4. **Pair Plot** – Numerical Features
5. **Heatmap** – Correlation Matrix
6. **Swarm Plot** – Engagement vs Device

---

## Plotly

An interactive daily engagement visualization was created using Plotly.

```python
daily_engagement = (
    df.groupby(df["posted_at"].dt.date)["engagement_rate"]
    .mean()
    .reset_index()
)

daily_engagement.columns = [
    "date",
    "avg_engagement"
]

fig = px.line(
    daily_engagement,
    x="date",
    y="avg_engagement",
    title="Interactive Daily Engagement Trend",
    markers=True
)

fig.show()
```

---

# 💡 Final Insights

## Content Performance

### Post Type

**Video posts** recorded the highest average engagement rate at approximately **1.122**, followed by text posts (1.065), image posts (0.896), and reel posts (0.783).

### Content Category

The **food category** recorded the highest average engagement rate at approximately **1.359**, followed by technology (1.160) and lifestyle (1.091).

### Country

**Brazil** recorded the highest average engagement rate at approximately **1.541**, followed by Australia (1.324), France (1.146), and UAE (1.112).

---

## User Trends

### Age

The **46–55** and **18–25** age groups recorded relatively higher average engagement, while the **56–65** age group recorded the lowest average engagement among the observed groups.

### Verified Accounts

Verified accounts recorded an average engagement rate of approximately **1.054**, compared with **0.955** for unverified accounts.

The difference was approximately **0.099 engagement-rate points**.

---

## Behavioral Insights

### Best Time for Impressions

The highest observed average impression count occurred around **12:00 AM (midnight)**, with approximately **50,014 impressions**.

### Device Type

Mobile devices recorded the highest average watch time:

| Device  | Average Watch Time |
| ------- | -----------------: |
| Mobile  |       4,087.83 sec |
| Tablet  |       3,979.74 sec |
| Desktop |       3,974.79 sec |

This indicates that mobile users spent slightly more time watching content compared with tablet and desktop users in this dataset.

---

## Sentiment Analysis

### Sentiment Performance

Negative sentiment posts recorded the highest average engagement rate:

| Sentiment | Average Engagement Rate |
| --------- | ----------------------: |
| Negative  |                   1.038 |
| Neutral   |                   0.991 |
| Positive  |                   0.919 |

### Negative vs Neutral Posts

| Metric           | Negative | Neutral |
| ---------------- | -------: | ------: |
| Average Likes    |   10,208 |   9,923 |
| Average Comments |    1,516 |   1,528 |
| Engagement Rate  |    1.038 |   0.991 |

Negative sentiment posts recorded slightly higher average likes and engagement rates, while neutral posts recorded slightly more average comments. Both sentiment types showed similar average shares and impression counts.

---

# 📌 Key Findings at a Glance

| Analysis Area                 | Key Finding     |
| ----------------------------- | --------------- |
| Highest-performing post type  | Video           |
| Highest-performing category   | Food            |
| Highest country engagement    | Brazil          |
| Higher-engagement age groups  | 18–25 and 46–55 |
| Verified account engagement   | 1.054           |
| Unverified account engagement | 0.955           |
| Highest impression hour       | 12:00 AM        |
| Highest watch-time device     | Mobile          |
| Highest sentiment engagement  | Negative        |
| Duplicate records             | 0               |

---

# 🎓 Learning Outcomes

Through this project, I gained practical experience in:

* Python programming for data analysis
* Pandas DataFrame manipulation
* NumPy numerical operations
* Missing-value treatment
* Duplicate detection
* Data type conversion
* Feature engineering
* Exploratory Data Analysis
* Groupby analysis
* DataFrame merging
* Descriptive statistics
* Correlation analysis
* Matplotlib visualization
* Seaborn visualization
* Interactive Plotly visualization
* Data interpretation
* Analytical insight generation

---

# 📁 Project Structure

```text
Social-Media-Engagement-Analytics/
│
├── social_media_engagement_5000.csv
│
├── Social_Media_Engagement_Analytics.ipynb
│
├── README.md
│
├── Project_Document.docx
│
└── visualizations/
    ├── likes_vs_impressions.png
    ├── daily_engagement.png
    ├── posts_by_category.png
    ├── gender_distribution.png
    ├── age_distribution.png
    ├── engagement_rate_boxplot.png
    ├── post_type_engagement.png
    └── device_watch_time.png
```

---

# 🚀 Conclusion

This project demonstrates an end-to-end **Social Media Engagement Analytics workflow using Python**.

The analysis transformed raw social media data into meaningful insights by applying data cleaning, transformation, exploratory analysis, statistical techniques, and visualization.

The project provides an understanding of how engagement varies across **content types, categories, countries, age groups, account verification status, posting times, devices, and sentiment**.

This project showcases practical skills in **Python, Pandas, NumPy, Matplotlib, Seaborn, Plotly, data cleaning, EDA, statistical analysis, visualization, and insight generation**.

---

## 👩‍💻 Author

**Dhivya M**

**Aspiring Data Analyst | Python | SQL | Power BI | Excel**

---
