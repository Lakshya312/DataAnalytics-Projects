# Netflix Data Analysis Portfolio

Welcome to my data analytics portfolio! This repository documents my exploration of the Netflix content library. It showcases my ability to process real-world entertainment data, uncover production trends, and visualize viewer preferences using Python.

# 1️⃣ Project: Netflix Movies & TV Shows Analysis 🍿

## Overview
This project focuses on the vast catalog of titles available on Netflix. I analyzed a dataset of movies and TV shows to understand:

1.  **Ratings Breakdown:** Which target audiences (e.g., TV-MA, PG-13) are most catered to?
2.  **Top Contributors:** Who are the most prolific directors, actors, and producing countries?
3.  **Production Trends:** How has the volume of content changed over the years, distinguishing between Movies and TV Shows?
4.  **Sentiment Analysis:** What is the emotional tone of the content descriptions?

The goal is to provide a data-driven look into Netflix's content strategy and library composition.

## 🛠️ Tools I Used
To perform this analysis, I utilized the following tools and libraries:
* **Python 3:** The core programming language for the analysis.
* **Pandas:** For data manipulation, cleaning, and aggregation.
* **Matplotlib & Seaborn:** For creating static statistical visualizations.
* **Plotly:** For creating interactive and dynamic charts.
* **TextBlob:** For performing natural language processing (sentiment analysis).
* **Jupyter Notebooks:** For interactive coding and documentation.

## 📂 Data Preparation & Cleanup
Before diving into the questions, I cleaned and standardized the data to ensure accuracy.

**Key Steps:**
1.  **Loading:** Imported the dataset using Pandas.
2.  **Handling Missing Values:** Filled missing values for critical columns like `director` (with "Director Not Specified"), `cast` (with "No Cast Specified"), and `country`.
3.  **Data Transformation:** The `director` and `cast` columns contained comma-separated lists. I split these into individual rows to analyze contributors separately.
4.  **Date Parsing:** Converted `date_added` to datetime objects for accurate time-based analysis.

```python
# Standard Import & Cleaning Code
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import plotly.express as px

# 1. Load Data
df = pd.read_csv("netflix_titles.csv")

# 2. Handle Missing Values
df['director'] = df['director'].fillna('Director Not Specified')
df['cast'] = df['cast'].fillna('No Cast Specified')
df['country'] = df['country'].fillna('country_not_specified')

# 3. Extract Year and Type for Trends
df['date_added'] = pd.to_datetime(df['date_added'])
df['year_added'] = df['date_added'].dt.year
```

# The Analysis

## 1. Content Ratings Distribution

Goal: To understand the target audience of Netflix's library. I grouped the content by rating to see which age groups are most represented.

### Visualize Data

```python
# Ratings Distribution
x = df.groupby(['rating']).size().reset_index(name='counts')

# Pie Chart
pieChart = px.pie(x, values='counts', names='rating', title='Distribution of Content Ratings')
pieChart.show()

# Bar Chart
plt.bar(x['rating'], x['counts'])
plt.title('Distribution of Content on Netflix')
plt.xlabel('Ratings')
plt.ylabel('Counts')
plt.xticks(rotation=75)
plt.show()
```
## Results
![Visualizatoion of top skills for data nerds](/4_Netflix_Project/images/content_rating.png)

## Insight
* Target Audience: The library is heavily skewed towards mature audiences, with TV-MA being the most common rating.

* Family Content: There is a significant portion of content rated TV-14 and PG-13, catering to teens and general audiences.

## 2. Top Contributors (Directors, Actors, Countries)
Goal: To identify the key players driving Netflix content. I processed the director, cast, and country columns to count individual contributions and see regional dominance.

### Visualize Data
```python
# Selecting top5 directors
top_five_directors = directors.head()
top_five_directors

# Plotting the top5 directors
top_five_directors = top_five_directors.sort_values(by='total_counts', ascending=True)
plt.barh(top_five_directors['Directors'], top_five_directors['total_counts'])
plt.title('Top 5 Directors on Netflix')
plt.ylabel('Directors')
plt.xlabel('Movies And TV Shows')
```
## Results
### Top 5 Directors
![Visualizatoion of top skills for data nerds](/4_Netflix_Project/images/top5_directors.png)

### Top 5 Actors
![Visualizatoion of top skills for data nerds](/4_Netflix_Project/images/top5_actors.png)

### Top 5 Content Producing Countries
![Visualizatoion of top skills for data nerds](/4_Netflix_Project/images/top5_country_content.png)

## Insight
* Top Directors: Rajiv Chilaka is the top director, known for a high volume of animated content.

* Top Actors: Anupam Kher leads in appearances, highlighting the strong presence of Indian cinema on the platform.

* Regional Dominance: The United States is by far the largest producer, followed by India and the United Kingdom.

## 3. Production Trends (Movies vs. TV Shows)
Goal: To visualize the growth of Netflix's library over time and compare the production volume of Movies versus TV Shows.

```python
# Group data by Release Year and Type
counts = df.groupby(['release_year', 'type']).size().reset_index(name='Total_Counts')
counts.rename(columns={'release_year': 'Release_Year', 'type': 'Type'}, inplace=True)

# Filter for recent years (e.g., >= 2000)
trend = counts[counts['Release_Year'] >= 2000]

# Separate data
movies_trend = trend[trend['Type'] == 'Movie']
tv_trend = trend[trend['Type'] == 'TV Show']

# Plot Line Chart
plt.figure(figsize=(12, 5))
plt.plot(movies_trend['Release_Year'], movies_trend['Total_Counts'], label='Movies', marker='o')
plt.plot(tv_trend['Release_Year'], tv_trend['Total_Counts'], label='TV Shows', marker='o')
plt.xlabel("Release Year")
plt.ylabel("Total Titles")
plt.title("Trend of Movies vs TV Shows Over the Years")
plt.legend()
plt.grid(True)
plt.show()
```
## Results
### Content Produced on netflix based on years
![Visualizatoion of top skills for data nerds](/4_Netflix_Project/images/netflix_content.png)

## Insights
* Explosive Growth: Content production saw a massive spike starting around 2015, coinciding with Netflix's aggressive push into original programming.

* Format Battle: While Movies have historically dominated, the gap between Movies and TV Shows has narrowed significantly in recent years.

* Recent Dip: A decline in numbers for 2020-2021 reflects the global impact of the pandemic on production schedules.

## 4. Sentiment Analysis
Goal: To determine the emotional tone of the descriptions for Netflix titles. I categorized them as Positive, Negative, or Neutral using Natural Language Processing.

### Visualize Data
```python
# Calculate Sentiment
sentiment_analysis = df[['release_year', 'description']].rename(columns={'release_year': 'Release Year', 'description': 'Description'})

for index, row in sentiment_analysis.iterrows():
    d = row['Description']
    testimonial = TextBlob(d)
    p = testimonial.sentiment.polarity
    if p == 0:
        sent = 'Neutral'
    elif p > 0:
        sent = 'Positive'
    else:
        sent = 'Negative'
    sentiment_analysis.loc[index, 'Sentiment'] = sent

# Group and Plot
sentiment_analysis = sentiment_analysis.groupby(['Release Year', 'Sentiment']).size().reset_index(name='Total Count')
sentiment_analysis = sentiment_analysis[sentiment_analysis['Release Year'] > 2005]

barGraph = px.bar(sentiment_analysis, x="Release Year", y="Total Count", color="Sentiment", title="Sentiment Analysis of Content on Netflix")
barGraph.show()
```
## Insights
* Positive Tone: The majority of content descriptions have a Positive or Neutral sentiment, likely to attract viewers.

* Content Correlation: The rise in all sentiment categories correlates perfectly with the overall increase in content production.

* Negative Descriptions: A distinct portion of descriptions is "Negative," often associated with genres like Crime, Thriller, and Drama where conflict is central to the plot.
#
Data Source: [Kaggle - Netflix Movies and TV Shows](https://www.kaggle.com/datasets/shivamb/netflix-shows).

My Jupyter Notebook: [Netflix Titles EDA](/4_Netflix_Project/Netflix_Titles_EDA_Report.ipynb)