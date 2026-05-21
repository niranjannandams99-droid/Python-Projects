<img width="1026" height="481" alt="Screenshot 2026-05-21 143741" src="https://github.com/user-attachments/assets/21db0d1c-5713-4578-86ee-2850ebcfcd22" />




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

<img width="561" height="602" alt="image" src="https://github.com/user-attachments/assets/5dae2e68-384e-4aa8-8c91-e4a38779414d" />


> **📈 Insight:** BTM, Koramangala, and Indiranagar have the highest restaurant density — strong food cultures but intense competition. Low-density areas represent untapped market opportunities.

---

### 2️⃣ Customer Demand by Location

Customer votes are used as a proxy for demand. Higher average votes indicate stronger, more engaged audiences.

```python
df.groupby('location')['votes'].mean().sort_values(ascending=False).head(10).plot(kind='bar')
plt.title("Average Customer Engagement by Location")
plt.show()
```

<img width="562" height="603" alt="image" src="https://github.com/user-attachments/assets/11094bbc-96d0-4d32-8a2a-419286f5fbe0" />



> **📈 Insight:** Locations like Church Street and MG Road show very high average votes, signalling strong customer demand regardless of restaurant count.

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

<img width="589" height="529" alt="image" src="https://github.com/user-attachments/assets/2ce14000-c09b-413c-9e87-8fd39d0b10df" />



> **📈 Insight:** North Indian and Chinese cuisines dominate the Bangalore market. Offering niche or underrepresented cuisines could reduce direct competition.

---

### 4️⃣ Pricing Strategy — Boxplot

A boxplot reveals the spread and outliers in restaurant pricing across Bangalore.

```python
sns.boxplot(x=df['approx_cost(for two people)'])
plt.title("Restaurant Price Distribution (Boxplot)")
plt.show()
```

<img width="522" height="455" alt="image" src="https://github.com/user-attachments/assets/d2017f60-d08e-46dd-95b9-b00552aaaf4f" />


> **📈 Insight:** The median cost sits in the ₹300–₹700 range, with a long right tail — a small number of premium restaurants skew the distribution significantly.

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

<img width="594" height="450" alt="image" src="https://github.com/user-attachments/assets/d478707f-662d-4aa7-8e48-2624dca23d82" />




> **📈 Insight:** Most restaurants fall in the **₹300–₹700** price range. Mid-range pricing is the dominant and most competitive strategy in Bangalore's food market.

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

<img width="592" height="456" alt="image" src="https://github.com/user-attachments/assets/e93633c1-3641-40ab-b305-6342fe786844" />


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

  <img width="552" height="587" alt="Screenshot 2026-05-21 142140" src="https://github.com/user-attachments/assets/efe78fbb-1370-4765-9ab6-f08722cbcac2" />


> **📈 Insight:** Several emerging and mid-density locations score high on the opportunity index — high demand with relatively fewer restaurants. These are prime candidates for new restaurant openings.

---

## 🔥 Key Insights Summary

| # | Insight |
|---|---|
| 📍 | BTM, Koramangala, and Indiranagar are the most saturated locations |
| 📊 | Church Street and similar areas show the strongest average customer demand |
| 🍜 | North Indian and Chinese cuisines dominate — niche offerings can reduce competition |
| 💰 | Mid-range pricing (₹300–₹700) is the sweet spot for most customers |
| ⭐ | Higher ratings strongly correlate with more votes and visibility |
| 🎯 | Emerging locations with high Opportunity Scores are ideal for new entrants |

---

## 💡 Business Recommendations

### 📍 Location Strategy
Avoid over-saturated hubs unless offering strong differentiation. Prioritize locations with a high Opportunity Score — areas where customer demand outpaces supply.

### 💰 Pricing Strategy
Mid-range pricing (₹300–₹700 for two) attracts the widest customer base. Premium pricing must be backed by exceptional experience and branding.

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

> ⭐ If you found this project useful, star the repo and share your feedback!
