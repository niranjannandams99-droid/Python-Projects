<img width="1026" height="481" alt="Screenshot 2026-05-21 143741" src="https://github.com/user-attachments/assets/9c86f032-1f46-428e-8746-edd7316c8303" />





# 🍽️ Where Should You Open a Restaurant in Bangalore?
## A Data-Driven Market Analysis Using Zomato Data

![Python](https://img.shields.io/badge/Python-3.9-blue?style=for-the-badge&logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-black?style=for-the-badge&logo=pandas)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-orange?style=for-the-badge)
![Seaborn](https://img.shields.io/badge/Seaborn-Statistical%20Plots-green?style=for-the-badge)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?style=for-the-badge&logo=jupyter)

---

## 📌 Project Overview

This project explores the Bangalore restaurant market using data from Zomato to identify key factors that influence restaurant success. The analysis examines restaurant locations, cuisine trends, pricing patterns, and customer engagement to uncover actionable business insights.

> **Core Question: Where should a new restaurant open in Bangalore?**

---

## 🎯 Key Questions

- Which locations have the highest restaurant competition?
- Where is customer demand strongest?
- Which cuisines dominate the Bangalore food market?
- What pricing range is most common and performs best?
- Does higher pricing guarantee better ratings?
- Where can new restaurants find untapped opportunities?

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Python | Programming Language |
| Pandas | Data Cleaning & Analysis |
| NumPy | Numerical Operations |
| Matplotlib | Data Visualization |
| Seaborn | Statistical Visualization |
| Jupyter Notebook | Development Environment |

---

## 📂 Dataset

- **Source:** Zomato Bangalore Restaurants
- **Platform:** [Kaggle Dataset](https://www.kaggle.com/datasets/himanshupoddar/zomato-bangalore-restaurants)
- **Size:** 50,000+ restaurant records

**Key columns used:**

| Column | Description |
|---|---|
| `name` | Restaurant name |
| `location` | Area in Bangalore |
| `cuisines` | Cuisine types served |
| `rate` | Customer rating (out of 5) |
| `votes` | Number of customer votes (demand proxy) |
| `approx_cost(for two people)` | Approximate dining cost |
| `online_order` | Whether online ordering is available |
| `book_table` | Whether table booking is supported |

---

## 📦 Importing Libraries

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

---

## 📥 Loading the Dataset

```python
df = pd.read_csv("zomato_bangalore.csv")
df.head()
```

**Sample Output:**

| name | online_order | book_table | rate | votes | location |
|---|---|---|---|---|---|
| Jalsa | Yes | Yes | 4.1/5 | 775 | Banashankari |
| Spice Elephant | Yes | No | 4.1/5 | 787 | Banashankari |
| San Churro Cafe | Yes | No | 3.8/5 | 918 | Banashankari |

---

## 🧹 Data Cleaning

### Cleaning the Ratings Column

The `rate` column had values in `"4.1/5"` format. These were cleaned and converted to numeric:

```python
df['rate'] = df['rate'].astype(str)
df['rate'] = df['rate'].str.split('/').str[0]
df['rate'] = pd.to_numeric(df['rate'], errors='coerce')
```

### Cleaning the Cost Column

```python
df['approx_cost(for two people)'] = df['approx_cost(for two people)'].astype(str)
df['approx_cost(for two people)'] = df['approx_cost(for two people)'].str.replace(',', '')
df['approx_cost(for two people)'] = pd.to_numeric(df['approx_cost(for two people)'], errors='coerce')
```

### Working Subset

```python
rest = df[['name', 'location', 'cuisines', 'rate', 'votes', 'approx_cost(for two people)']]
rest.head()
```

---

## 📊 Exploratory Data Analysis (EDA)

---

### 1️⃣ Restaurant Competition Across Bangalore

Restaurant density by location reveals where competition is highest — and where gaps may exist.

```python
df['location'].value_counts().head(10).plot(kind='bar')
plt.title("Restaurant Density by Location")
plt.show()
```

<img width="561" height="602" alt="Screenshot 2026-05-21 143020" src="https://github.com/user-attachments/assets/fe274cbc-f3fc-4d31-96a7-e7e5555d4251" />




> **📈 Insight:** BTM and Koramangala have the highest restaurant density — strong food cultures but intense competition. Low-density areas represent untapped market opportunities.

---

### 2️⃣ Customer Demand by Location

Customer votes are used as a proxy for demand. Higher average votes indicate stronger, more engaged audiences.

```python
df.groupby('location')['votes'].mean().sort_values(ascending=False).head(10).plot(kind='bar')
plt.title("Average Customer Engagement by Location")
plt.show()
```

<img width="562" height="603" alt="Screenshot 2026-05-21 143122" src="https://github.com/user-attachments/assets/d908e9ea-cc3f-4b66-8029-27486bbe4da2" />




> **📈 Insight:** Church Street and Lavelle Road have fewer restaurants but extremely engaged customers — ideal for premium dining concepts.

---

### 3️⃣ Cuisine Market Trends

Understanding which cuisines dominate helps restaurant owners align menus with proven customer preferences.

```python
cuisines = df['cuisines'].dropna().str.split(',')
cuisines = cuisines.explode()
cuisines = cuisines.str.strip()
top_cuisines = cuisines.value_counts().head(10)

top_cuisines.plot(kind='bar')
plt.title("Most Popular Cuisines in Bangalore")
plt.xlabel("Cuisine")
plt.ylabel("Number of Restaurants")
plt.show()
```

<img width="589" height="529" alt="Screenshot 2026-05-21 143216" src="https://github.com/user-attachments/assets/8fd11af2-ac92-4dcc-b515-3403d384a91d" />




> **📈 Insight:** North Indian and Chinese cuisines dominate the Bangalore market. Offering niche or underrepresented cuisines could reduce direct competition.

---

### 4️⃣ Pricing Strategy — Boxplot

A boxplot reveals the spread and outliers in restaurant pricing across Bangalore.

```python
sns.boxplot(x=df['approx_cost(for two people)'])
plt.title("Restaurant Price Distribution (Boxplot)")
plt.show()
```

<img width="522" height="455" alt="Screenshot 2026-05-21 142756" src="https://github.com/user-attachments/assets/24e6dcad-0930-4e9f-a06b-bd555075cb58" />



> **📈 Insight:** The bulk of the market operates under ₹700 for two. Premium pricing above ₹1,500 is a niche strategy — high risk, high reward.
---

### 5️⃣ Pricing Strategy — Distribution Histogram

A histogram shows where the bulk of restaurant pricing is concentrated.

```python
df['approx_cost(for two people)'].plot(kind='hist', bins=30)
plt.title("Restaurant Pricing Distribution")
plt.xlabel("Cost for Two")
plt.ylabel("Frequency")
plt.show()
```

<img width="594" height="450" alt="Screenshot 2026-05-21 142604" src="https://github.com/user-attachments/assets/7c230333-cb4e-4298-bf1d-87c7b8578581" />





> **📈 Insight:** ₹300-₹600 is the most competitive price band. Positioning at ₹700-₹1,000 could be a sweet spot — above budget but below premium — less crowded.
---

### 6️⃣ Customer Satisfaction vs. Popularity

Does a higher rating translate to more customer engagement? This scatter plot explores the relationship.

```python
plt.scatter(df['rate'], df['votes'])
plt.xlabel("Rating")
plt.ylabel("Votes")
plt.title("Customer Satisfaction vs Popularity")
plt.show()
```

<img width="592" height="456" alt="Screenshot 2026-05-21 142419" src="https://github.com/user-attachments/assets/3fb860b4-3ebe-4fd6-afab-2163efb8292b" />



> **📈 Insight:** Restaurants with higher ratings generally accumulate more votes. Quality and popularity are closely linked — customer trust drives engagement.

---

### 7️⃣ Market Opportunity Analysis

An **Opportunity Score** is engineered to identify locations with strong demand relative to competition:

$$\text{Opportunity Score} = \frac{\text{Average Votes (Demand)}}{\text{Restaurant Count (Competition)}}$$

```python
location_demand = df.groupby('location')['votes'].mean()
location_competition = df['location'].value_counts()

market = pd.DataFrame({'demand': location_demand, 'competition': location_competition})
market['opportunity_score'] = market['demand'] / market['competition']

market.sort_values(by='opportunity_score', ascending=False).head(10)['opportunity_score'].plot(kind='bar')
plt.title("Best Locations for Opening a New Restaurant")
plt.show()
```

<img width="552" height="587" alt="Screenshot 2026-05-21 142140" src="https://github.com/user-attachments/assets/83aaa06e-a2c2-420d-b8bc-9f520d9fd48a" />



> **📈 Insight:** Rajarajeshwari Nagar is a goldmine — very high demand relative to competition. West Bangalore and Central Bangalore are secondary opportunities. These are the data-backed answers to "Where should you open a restaurant?"

---

## 🔥 Key Insights Summary

| # | Insight |
|---|---|
| 📍 Avoid | BTM and Koramangala are the most saturated locations |
| 📊 High demand | Church Street and similar areas show the strongest average customer demand |
| 🍜 Cuisine | North Indian and Chinese cuisines dominate — niche offerings can reduce competition |
| 💰 Price at | Mid-range pricing (₹300-₹700) is the sweet spot for most customers |
| ⭐ Target | Higher ratings strongly correlate with more votes and visibility |
| 🎯 Focus on | Emerging locations with high Opportunity Scores are ideal for new entrants |
---

## 💡 Business Recommendations

### 📍 Location Strategy
Avoid over-saturated hubs unless offering strong differentiation. Prioritize locations with a high Opportunity Score — areas where customer demand outpaces supply.

### 💰 Pricing Strategy
Mid-range pricing (₹300-₹700 for two) attracts the widest customer base. Premium pricing must be backed by exceptional experience and branding.

### 🍜 Cuisine Selection
Popular cuisines (North Indian, Chinese) guarantee a large audience but face fierce competition. Underrepresented cuisines can carve out a loyal niche.

### 📱 Digital Presence
Online ordering is essential in Bangalore's food ecosystem. Listing on Zomato with strong review management directly impacts votes and visibility.

---

## 🚀 Future Improvements

- Build a restaurant recommendation system
- Apply machine learning for rating prediction
- Create an interactive dashboard (Power BI / Tableau)
- Perform sentiment analysis on customer reviews
- Incorporate real-time Zomato API data

---

## 📌 Conclusion

This analysis demonstrates how data-driven thinking can guide strategic decisions in Bangalore's competitive restaurant market. Using Python and visualization libraries, the project surfaces clear patterns in customer behavior, pricing, location dynamics, and cuisine preferences.

Data alone doesn't guarantee success — but it significantly improves the odds of making the right call.

---

## 📁 Project Structure

```
📦 Bangalore-Restaurant-Market-Analysis
 ┣ 📜 Zomato_Bangalore_Analysis.ipynb
 ┣ 📜 README.md
 ┗ 📂 Dataset
     ┗ 📄 zomato_bangalore.csv
```

---

## 🔗 Links

| Resource | Link |
|---|---|
| 📓 GitHub Repository | [niranjannandams99-droid/Python-EDA](https://github.com/niranjannandams99-droid/Python-EDA) |
| 📊 Kaggle Dataset | [Zomato Bangalore Restaurants](https://www.kaggle.com/datasets/himanshupoddar/zomato-bangalore-restaurants) |
| 💼 LinkedIn | [niranjannandam](https://www.linkedin.com/in/niranjannandam) |

---

## 👨‍💻 Author

**Niranjan Nandam**
📍 Bangalore, India · 🎓 Electronics & Communication Engineering · 📊 Aspiring Data Analyst / Data Scientist

---


