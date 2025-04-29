Comprehensive Market Analysis & Performance Monitoring of Google Play Store Apps

📖 Project Overview:

This project presents a comprehensive SQL-based analysis of the Google Play Store dataset. The goal is to identify key insights related to app ratings, downloads, revenue generation, and user engagement, using SQL queries for business-driven recommendations. The data is cleaned using Python and analyzed in MySQL.

📊 Key Achievements:

Delivered actionable SQL-based insights to optimize app strategies.

Identified high-performing app categories for targeted launches.

Pinpointed hidden growth opportunities by analyzing underperforming and underrated apps.

Efficient handling of large datasets using scalable SQL queries.

📂 Data Cleaning and Preprocessing:

Python Libraries Used: pandas, numpy, datetime

Steps:

Removed nulls, special characters, duplicates, and inconsistent formats.

Exported cleaned data to CSV for MySQL ingestion.

Loaded the dataset into MySQL using:

LOAD DATA INFILE '/path/to/cleaned_googleplaystore.csv'
INTO TABLE googleplaystore
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

📊 SQL Data Analysis Queries:

1. Top 5 Categories for Free Apps Based on Average Ratings:

SELECT category, ROUND(AVG(rating), 2) AS avg_rating
FROM playstore
WHERE type = 'free'
GROUP BY category
ORDER BY avg_rating DESC
LIMIT 5;

2. Top 3 Categories Generating Most Revenue from Paid Apps:

SELECT category, ROUND(AVG(installs * price), 2) AS revenue
FROM playstore
WHERE type = 'paid'
GROUP BY category
ORDER BY revenue DESC
LIMIT 3;

3. Percentage of Games Within Each Category:

SELECT category,
       ROUND(SUM(CASE WHEN genres LIKE '%Game%' THEN 1 ELSE 0 END) / COUNT(*) * 100, 2) AS game_percentage
FROM playstore
GROUP BY category;

4. Free vs Paid: Average Rating and Total Installs:

SELECT type,
       ROUND(AVG(rating), 2) AS avg_rating,
       SUM(installs) AS total_installs
FROM playstore
WHERE rating IS NOT NULL AND installs IS NOT NULL
GROUP BY type;

5. App with the Most Reviews in Each Category:

SELECT category, app, MAX(reviews) AS max_reviews
FROM playstore
GROUP BY category, app
ORDER BY category, max_reviews DESC;

6. Underrated Apps: High Reviews but Low Ratings:

SELECT app, rating, reviews
FROM playstore
WHERE rating < 3.5 AND reviews > 50000
ORDER BY reviews DESC;

7. Top 5 Categories with Most High-Rated Apps (Rating ≥ 4.5):

SELECT category, COUNT(*) AS high_rated_apps
FROM playstore
WHERE rating >= 4.5
GROUP BY category
ORDER BY high_rated_apps DESC
LIMIT 5;

8. Genres with the Most Apps:

SELECT genres, COUNT(*) AS app_count
FROM playstore
GROUP BY genres
ORDER BY app_count DESC
LIMIT 10;

9. Most Installed App in Each Category:

SELECT category, app, MAX(installs) AS max_installs
FROM playstore
GROUP BY category, app
ORDER BY max_installs DESC;

10. Recommendation: Paid or Free Apps by Category (Based on Ratings):

WITH paid AS (
    SELECT category, ROUND(AVG(rating), 2) AS avg_paid
    FROM playstore
    WHERE type = 'paid'
    GROUP BY category
),
free AS (
    SELECT category, ROUND(AVG(rating), 2) AS avg_free
    FROM playstore
    WHERE type = 'free'
    GROUP BY category
)
SELECT paid.category, avg_paid, avg_free,
       IF(avg_paid > avg_free, 'Develop Paid Apps', 'Develop Free Apps') AS decision
FROM paid
JOIN free ON paid.category = free.category;

11. Hidden Gems: High Ratings but Low Installs

SELECT app, rating, installs
FROM playstore
WHERE rating >= 4.5 AND installs < 50000
ORDER BY rating DESC, installs ASC;

12. Clean Genres Column: Split Multi-Genre Entries

SELECT
  app,
  TRIM(SUBSTRING_INDEX(genres, ';', 1)) AS genre_primary,
  TRIM(SUBSTRING_INDEX(genres, ';', -1)) AS genre_secondary
FROM playstore
WHERE genres LIKE '%;%';

🔧 Conclusion:

This SQL project revealed important insights from the Google Play Store dataset:

✅ Free apps with high ratings attract more users.

✅ Paid apps in certain categories yield high average revenue.

⚡ Hidden gems and underrated apps highlight untapped market opportunities.

🔹 Genres and categories segmentation is critical for growth and targeting.

These findings can support marketing strategy, app development decisions, and investor pitches.

🎓 Technologies Used:

Python (Data Cleaning)

MySQL (Data Analysis)

Excel / Power BI (for Visualization, not shown here)
