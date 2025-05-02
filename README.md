
**Comprehensive Market Analysis & Performance Monitoring of Google Play Store Apps**

## 📌 Project Overview:
This project aims to perform a comprehensive analysis of the Google Play Store dataset to uncover key trends and patterns related to app ratings, installs, revenue generation, and user engagement. The project involves a thorough data cleaning process using Python libraries and subsequent analysis using MySQL. The primary goal is to provide actionable insights to support app development decisions, including identifying the top-performing app categories, understanding app distribution across genres, and recognizing underperforming apps with untapped potential.

## 🏆 Key Achievements:
- ✅ Provided **actionable insights** for optimizing app ratings, revenue strategies, and marketing campaigns for developers.
- ✅ Delivered a **detailed report** with recommendations on which categories to target for new app launches, both for free and paid apps.

---

## 🧹 Data Cleaning and Preprocessing:

### 1. Data Cleaning (Python):
- Utilized **Pandas**, **NumPy**, and **datetime** to clean the dataset.
- Removed null values, unwanted columns, special characters (like `$`), and inconsistent date formats.
- Exported the cleaned dataset as a CSV for import into MySQL.

### 2. Preparing MySQL for Data Import:
- Enabled `local_infile=ON` in `my.ini` for both `[client]` and `[mysqld]` sections.
- Restarted the MySQL service to apply changes.

### 3. Data Import:
```sql
LOAD DATA INFILE '/path/to/cleaned_googleplaystore.csv'
INTO TABLE googleplaystore
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
```

---

## 📈 Data Analysis (SQL):

### 1. Top 5 Categories for Free Apps Based on Average Ratings:
```sql
SELECT category, ROUND(AVG(rating), 2) AS 'avg_rating'
FROM playstore
WHERE type = 'free'
GROUP BY category
ORDER BY avg_rating DESC
LIMIT 5;
```

### 2. Top 3 Categories Generating Most Revenue (Paid Apps):
```sql
SELECT category, ROUND(AVG(installs * price), 2) AS 'revenue'
FROM playstore
WHERE type = 'paid'
GROUP BY category
ORDER BY revenue DESC
LIMIT 3;
```

### 3. Percentage of Games in Each Category:
```sql
SELECT *, (cnt / (SELECT COUNT(*) FROM playstore)) * 100 AS 'percentage'
FROM (SELECT category, COUNT(app) AS 'cnt' FROM playstore GROUP BY category) t;
```

### 4. Avg Rating & Installs: Free vs Paid
```sql
SELECT Type,
       AVG(Rating) AS Avg_Rating,
       SUM(Installs) AS Total_Installs
FROM PlayStore
WHERE Rating IS NOT NULL AND Installs IS NOT NULL
GROUP BY Type;
```

### 5. App with Most Reviews in Each Category:
```sql
SELECT Category, App, MAX(Reviews) AS Max_Reviews
FROM PlayStore
GROUP BY Category, App
ORDER BY Category, Max_Reviews DESC;
```

### 6. Underrated Apps (High Reviews, Low Ratings):
```sql
SELECT app, rating, reviews
FROM playstore
WHERE rating < 3.5 AND reviews > 50000
ORDER BY reviews DESC;
```

### 7. Top 5 Categories with Most High-Rated Apps (≥ 4.5):
```sql
SELECT category, COUNT(*) AS high_rated_apps
FROM playstore
WHERE rating >= 4.5
GROUP BY category
ORDER BY high_rated_apps DESC
LIMIT 5;
```

### 8. Genres with Most Apps:
```sql
SELECT genres, COUNT(*) AS app_count
FROM playstore
GROUP BY genres
ORDER BY app_count DESC
LIMIT 10;
```

### 9. Most Installed Apps by Category:
```sql
SELECT category, app, MAX(installs) AS max_installs
FROM playstore
GROUP BY category, app
ORDER BY max_installs DESC;
```

### 10. Recommendation: Paid vs Free App Strategy by Category
```sql
WITH t1 AS (
    SELECT category, ROUND(AVG(rating), 2) AS 'avg_paid' FROM playstore WHERE type = 'paid' GROUP BY category
),
t2 AS (
    SELECT category, ROUND(AVG(rating), 2) AS 'avg_free' FROM playstore WHERE type = 'free' GROUP BY category
)
SELECT a.category, avg_paid, avg_free,
       IF(avg_paid > avg_free, 'Develop Paid Apps', 'Develop Free Apps') AS 'decision'
FROM t1 a
INNER JOIN t2 b ON a.category = b.category;
```

---

## ✅ Conclusion:
This project provides valuable insights into the mobile app market, revealing important trends related to app ratings, revenue, installs, and user engagement.

### Key takeaways:
- **Free Apps** with high ratings → Higher user engagement.
- **Paid Apps** in niche categories → Revenue potential.
- **Hidden Gems** (high ratings, low installs) → Growth opportunities.
- **Underrated Apps** → Need visibility & optimization.
- **Genre analysis** → Strategic category targeting for new app launches.


---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).

---

## 🙌 Acknowledgments

- Dataset by Kaggle community
- Tools: Python, MySQL


