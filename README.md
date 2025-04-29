# Comprehensive-Market-Analysis-Performance-Monitoring-of-Google-Play-Store-Apps
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
