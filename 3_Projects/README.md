# Data Analytics Portfolio
Welcome to my data analytics portfolio! This repository documents my journey in analyzing the data job market and streaming content trends. It showcases my ability to process real-world data, uncover insights, and visualize findings using Python.

# 1️⃣ Project: Data Job Market Analysis 📊
## Overview

This project focuses on the data analyst job market in the United States. I analyzed a dataset of job postings to understand:
1.  **Skill Demand:** Which technical skills are most requested?
2.  **Market Trends:** How demand for these skills changed throughout 2023.
3.  **Salary Insights:** How much these roles and skills actually pay.
4.  **Optimal Skills:** Identifying the "sweet spot" skills that are both high-demand and high-paying.

The goal is to provide a data-driven roadmap for anyone (including myself!) looking to enter or advance in the data field.

## 🛠️ Tools I Used
To perform this analysis, I utilized the following tools and libraries:
* **Python 3:** The core programming language for the analysis.
* **Pandas:** For cleaning, filtering, and aggregating the raw data.
* **Matplotlib & Seaborn:** For creating static and statistical visualizations.
* **Jupyter Notebooks:** For interactive coding and documentation.
* **VS Code:** My primary environment for writing and managing code.
* **Git & GitHub:** For version control and sharing this portfolio.

## 📂 Data Preparation & Cleanup
Before diving into the questions, I standardized the data to ensure accuracy. This setup process is shared across all the notebooks in this project.

**Key Steps:**
1.  **Loading:** Imported the dataset using the `datasets` library.
2.  **Date Parsing:** Converted `job_posted_date` to datetime objects for trend analysis.
3.  **Skill Parsing:** The `job_skills` column was stored as strings (e.g., `"['SQL', 'Python']"`). I used `ast.literal_eval` to convert them into usable Python lists.
4.  **Filtering:** Restricted the dataset to `United States` job postings to keep the analysis focused.

```python
# Standard Import & Cleaning Code Used Across Notebooks
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# 1. Load Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# 2. Clean Data (Dates & Skills)
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)

# 3. Filter for US Market
df_US = df[df['job_country'] == 'United States']
```

# The Analysis

## 1. Skill Demand Analysis

## What are the most demanded skills for the top 3 most popular data roles?

**Goal:** To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.

**Notebook:** [2_Skills_Demand](/3_Projects/3_Skill_Demand.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1, figsize=(7,6))

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0, 78)
```

## Results
![Visualizatoion of top skills for data nerds](/3_Projects/images/skill_demand.png)

## Insights

* SQL is the most requested skill for Data Analysts and Data Scientists, appearing in a vast majority of job postings.

* Python is the most versatile skill, highly demanded across all three roles, but it is the undisputed #1 requirement for Data Scientists and Data Engineers.

* Data Engineers require more specialized technical infrastructure skills (AWS, Azure, Spark) compared to Data Analysts, who focus more on general data manipulation tools (Excel, Tableau).

## 2. Skill Trend Analysis
## How are in-demand skills trending for Data Analysts?

**Goal:** To analyze how the demand for top data analyst skills evolved throughout 2023. I filtered for Data Analyst positions and grouped the skills by the month of the job postings to identify trends.

**Notebook:** [3_Skills_Trend](/3_Projects/4_Skills_Trend.ipynb)

### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```

## Results
![Visualizatoion of trending skills](/3_Projects/images/skill_trend.png)

## Insights

* SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand.

* Excel experienced a significant increase in demand starting around September, surpassing both Python and Tableau by the end of the year.

* Python and Tableau show relatively stable demand throughout the year with some fluctuations but remain essential skills for data analysts.

## 3. Salary Analysis
##  How well do jobs and skills pay for Data Analysts?
**Goal:** To determine which roles and skills pay the most. I analyzed the salary distributions for common data roles and identified the highest-paying skills for Data Analysts.

**Notebook:** [4_Skills_Analysis](/3_Projects/5_Salary_Analysis.ipynb)

### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```

## Results
![Analysing salary](/3_Projects/images/box_plot.png)

## Insights

* Senior Roles Pay More: Unsurprisingly, Senior Data Scientists and Senior Data Engineers command the highest median salaries, often exceeding $150k.

* Data Analyst Salaries: The median salary for Data Analysts is lower and has a tighter distribution, suggesting less variability in pay compared to engineering roles.

* Outliers: There are significant high-salary outliers in Data Engineering, indicating that specialized expertise in this field can lead to exceptional compensation.

![Analysing salary](/3_Projects/images/salary_analysis.png)

## 4. Optimal Skills Analysis
## What are the most optimal skills to learn for Data Analysts?

**Goal:** To identify the "optimal" skills to learn—those that are both high demand (easy to get a job) and high paying (good salary). I visualized this by plotting median salary against the percentage of job postings requiring the skill.

**Notebook:** [5_Optimal_Skills](/3_Projects/6_Optimal_Skills.ipynb.ipynb)

### Visualize Data

```python
# Create scatter plot for Optimal Skills
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()
```

## Results
![Visualizatoion of trending skills](/3_Projects/images/optimal_skills.png)

## Insights

* The "Sweet Spot": Skills like Python and Tableau fall into the optimal quadrant—they are relatively high in demand (appearing in 20-30% of postings) and offer competitive salaries.

* High Pay, Niche Demand: Skills like Oracle and SQL Server offer high salaries but are less frequently requested, making them specialized "niche" skills.

* Foundational Skills: Excel and SQL are ubiquitous (high demand) but offer lower median salaries, reinforcing their role as entry-level necessities rather than salary drivers.