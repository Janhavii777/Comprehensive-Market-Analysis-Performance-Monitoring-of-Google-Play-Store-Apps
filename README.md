# 📊 Comprehensive Market Analysis & Performance Monitoring of Google Play Store Apps

## 🔍 Project Overview
This project performs an in-depth analysis of the Google Play Store dataset to uncover key insights related to:
- App ratings
- Installs
- Monetization strategies
- User engagement

It utilizes **Python (Pandas, NumPy)** for data cleaning and **MySQL** for querying large datasets to provide recommendations that help:
- Identify high-performing categories
- Spot hidden gems
- Decide between free vs. paid app strategies

---

## 🌟 Key Achievements
- ✅ Actionable insights for app performance and revenue optimization
- ✅ Efficient pipelines for large-scale data (millions of rows)
- ✅ Strategic recommendations for launching new apps
- ✅ Identification of underperforming apps with growth potential

---

## 📂 Data Cleaning & Preprocessing

### 1. Clean Dataset using Python
- Removed nulls, duplicates, special characters like `$`
- Standardized inconsistent date formats
- Saved clean dataset to CSV

### 2. Configure MySQL for Import
- Enabled `local_infile=ON` in `my.ini`
- Restarted MySQL server

### 3. Load CSV into MySQL
```sql
LOAD DATA INFILE '/path/to/cleaned_googleplaystore.csv'
INTO TABLE googleplaystore
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

📊 Data Analysis Queries
1. Top 5 Categories for Free Apps by Avg Rating
SELECT category, ROUND(AVG(rating), 2) AS avg_rating
FROM playstore
WHERE type = 'free'
GROUP BY category
ORDER BY avg_rating DESC
LIMIT 5;

2. Top 3 Revenue Generating Categories (Paid)
SELECT category, ROUND(AVG(installs * price), 2) AS revenue
FROM playstore
WHERE type = 'paid'
GROUP BY category
ORDER BY revenue DESC
LIMIT 3;

3. % of Games in Each Category
SELECT *, (cnt / (SELECT COUNT(*) FROM playstore)) * 100 AS percentage
FROM (
  SELECT category, COUNT(app) AS cnt
  FROM playstore
  GROUP BY category
) t;

4. Avg Rating & Installs for Free vs Paid Apps
SELECT Type, AVG(Rating) AS Avg_Rating, SUM(Installs) AS Total_Installs
FROM PlayStore
WHERE Rating IS NOT NULL AND Installs IS NOT NULL
GROUP BY Type;

5. Most Reviewed App in Each Category
SELECT Category, App, MAX(Reviews) AS Max_Reviews
FROM PlayStore
GROUP BY Category, App
ORDER BY Category, Max_Reviews DESC;

6. Underrated Apps (High Reviews, Low Ratings)
SELECT app, rating, reviews
FROM playstore
WHERE rating < 3.5 AND reviews > 50000
ORDER BY reviews DESC;

7. Top 5 Categories with Most High-Rated Apps (≥ 4.5)
SELECT category, COUNT(*) AS high_rated_apps
FROM playstore
WHERE rating >= 4.5
GROUP BY category
ORDER BY high_rated_apps DESC
LIMIT 5;

8. Top Genres by App Count
SELECT genres, COUNT(*) AS app_count
FROM playstore
GROUP BY genres
ORDER BY app_count DESC
LIMIT 10;

9. Most Installed Apps by Category
SELECT category, app, MAX(installs) AS max_installs
FROM playstore
GROUP BY category, app
ORDER BY max_installs DESC;

10. Should You Build Paid or Free Apps?
WITH t1 AS (
  SELECT category, ROUND(AVG(rating), 2) AS avg_paid
  FROM playstore
  WHERE type = 'paid'
  GROUP BY category
),
t2 AS (
  SELECT category, ROUND(AVG(rating), 2) AS avg_free
  FROM playstore
  WHERE type = 'free'
  GROUP BY category
)
SELECT a.category, avg_paid, avg_free,
       IF(avg_paid > avg_free, 'Develop Paid Apps', 'Develop Free Apps') AS decision
FROM t1 a
JOIN t2 b ON a.category = b.category;

🧠 Key Takeaways
Free apps with high ratings attract strong engagement.

Paid apps in niche categories can drive high revenue.

Hidden gems (high rating, low installs) are growth opportunities.

Underrated apps (high reviews, low ratings) may need better user experience.

Genre insights uncover market gaps and competitive advantages.

📁 Technologies Used
Python (Pandas, NumPy)

MySQL

Data Visualization Tools (optionally Power BI, Tableau)



