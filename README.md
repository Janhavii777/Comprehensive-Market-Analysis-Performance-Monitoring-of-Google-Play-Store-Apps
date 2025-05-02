
# 📊 Google Play Store Data Analysis & Performance Monitoring

## 🚀 Project Overview

This project focuses on performing a **comprehensive SQL-based analysis** of the Google Play Store dataset to extract insights around app ratings, installs, revenue, user engagement, and app categorization. The goal is to identify growth opportunities, optimize app strategies, and uncover hidden patterns to guide better app development decisions.

---

## 🎯 Objectives

- Analyze app performance by category and type (free vs paid)
- Identify high-potential categories for new app launches
- Calculate revenue insights and app distribution
- Detect hidden gems and underrated apps
- Generate actionable business recommendations using SQL

---

## 🛠️ Tools & Technologies

- **SQL (MySQL)** – for querying and data analysis
- **Python (Pandas, NumPy)** – for data cleaning and preprocessing
- **Excel** – for early inspection
- **Power BI** – for visualization (optional)

---

## 📂 Dataset

- **Source**: Google Play Store Dataset (publicly available)
- **Features**: App name, Category, Rating, Reviews, Installs, Type, Price, Genre, and more.

---

## 🧹 Data Cleaning (Python)

- Removed duplicates, nulls, and formatting issues
- Converted data types (e.g., price, installs)
- Split multi-genre entries
- Exported cleaned CSV for SQL import

---

## 🧮 SQL Analysis Highlights

### 1. Top 5 Categories for Launching New Free Apps
```sql
SELECT category, ROUND(AVG(rating), 2) AS avg_rating
FROM playstore
WHERE type = 'free'
GROUP BY category
ORDER BY avg_rating DESC
LIMIT 5;
```

### 2. Top 3 Categories by Paid App Revenue (price × installs)
```sql
SELECT category, ROUND(AVG(installs * price), 2) AS revenue
FROM playstore
WHERE type = 'paid'
GROUP BY category
ORDER BY revenue DESC
LIMIT 3;
```

### 3. Game Percentage by Category
```sql
SELECT category,
       ROUND(SUM(CASE WHEN genres LIKE '%Game%' THEN 1 ELSE 0 END) * 100 / COUNT(*), 2) AS game_percentage
FROM playstore
GROUP BY category;
```

### 4. Average Rating and Total Installs (Free vs Paid)
```sql
SELECT type, AVG(rating) AS avg_rating, SUM(installs) AS total_installs
FROM playstore
WHERE rating IS NOT NULL AND installs IS NOT NULL
GROUP BY type;
```

### 5. Highest Reviewed App in Each Category
```sql
SELECT category, app, MAX(reviews) AS max_reviews
FROM playstore
GROUP BY category;
```

### 6. Underrated Apps (High Reviews, Low Rating)
```sql
SELECT app, rating, reviews
FROM playstore
WHERE rating < 3.5 AND reviews > 50000
ORDER BY reviews DESC;
```

### 7. Top 5 Categories with Most High-Rated Apps (≥ 4.5)
```sql
SELECT category, COUNT(*) AS high_rated_apps
FROM playstore
WHERE rating >= 4.5
GROUP BY category
ORDER BY high_rated_apps DESC
LIMIT 5;
```

### 8. Most Common Genres
```sql
SELECT genres, COUNT(*) AS app_count
FROM playstore
GROUP BY genres
ORDER BY app_count DESC
LIMIT 10;
```

### 9. Most Installed Apps by Category
```sql
SELECT category, app, MAX(installs) AS max_installs
FROM playstore
GROUP BY category, app
ORDER BY max_installs DESC;
```

### 10. Recommendation: Free or Paid App by Category
```sql
WITH paid_avg AS (
  SELECT category, ROUND(AVG(rating), 2) AS avg_paid
  FROM playstore
  WHERE type = 'paid'
  GROUP BY category
),
free_avg AS (
  SELECT category, ROUND(AVG(rating), 2) AS avg_free
  FROM playstore
  WHERE type = 'free'
  GROUP BY category
)
SELECT paid_avg.category, avg_paid, avg_free,
       CASE WHEN avg_paid > avg_free THEN 'Develop Paid Apps'
            ELSE 'Develop Free Apps' END AS recommendation
FROM paid_avg
JOIN free_avg ON paid_avg.category = free_avg.category;
```

---

## 📈 Key Insights

- **Free apps** generally receive more installs and better average ratings.
- **Paid apps** in certain categories (e.g., Productivity, Finance) generate higher revenue.
- **Game apps** dominate in many categories.
- **Hidden gems** and **underrated apps** present opportunities for improvement and investment.
- Recommendations suggest category-specific app monetization strategies.

---

## 📌 Conclusion

This project provided valuable insights into app market trends, enabling smarter app strategy and market positioning using SQL. The structured approach also ensures this analysis is scalable and adaptable to large datasets.

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).

---

## 🙌 Acknowledgments

- Dataset by Kaggle community
- Tools: Python, MySQL


