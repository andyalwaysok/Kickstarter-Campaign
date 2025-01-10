# 🏢 Data Analysis for Start-up Comapny using SQL

## 📌 Introduction
The product team is considering launching a campaign on Kickstarter to test the viability of some offerings. 

I've been asked to pull data that will help the team understand what might influence the success of a campaign.

## Database structure
<img width="400" alt="image" src="https://github.com/andyalwaysok/Kickstarter-Campaign/blob/947005f2202e0b1b9a1e7fcfae3e127ce8b92872/Kickstarter%201.png">

## 🔑 Questions and answers

### 1. Listing names and data types for each table in the database
````sql
PRAGMA table_info(ksprojects)
````
<img width="400" alt="image" src="https://github.com/andyalwaysok/Kickstarter-Campaign/blob/e1fc966796c6990a3f45486f1e7421fcaf541e1b/sql1.png">

### 2. Want to see records where the project state is either 'failed', 'canceled', or 'suspended'
````sql
SELECT main_category, backers, pledged, goal
FROM ksprojects
WHERE state IN ('failed', 'canceled', 'suspended')
LIMIT 10
````
<img width="400" alt="image" src=https://github.com/andyalwaysok/Kickstarter-Campaign/blob/e1fc966796c6990a3f45486f1e7421fcaf541e1b/sql2.png>

### 3. Expanding query to find which of these projects had at least 100 backers and at least $20,000 pledged
````sql
SELECT main_category, backers, pledged, goal
FROM ksprojects
WHERE state IN ('failed', 'canceled', 'suspended') 
AND backers >= 100 
AND pledged >= 20000
LIMIT 10
````
<img width="400" alt="image" src=https://github.com/andyalwaysok/Kickstarter-Campaign/blob/e1fc966796c6990a3f45486f1e7421fcaf541e1b/sql3.png>


### 4. Product team would like to view projects by categories, along with the percentage of the goal that was funded
````sql
SELECT main_category, backers, pledged, goal, pledged/goal as pct_pledged
FROM ksprojects
WHERE state IN ('failed')
AND backers >= 100 AND pledged >= 20000
ORDER BY 1, pct_pledged DESC
LIMIT 10
````
<img width="400" alt="image" src=https://github.com/andyalwaysok/Kickstarter-Campaign/blob/e1fc966796c6990a3f45486f1e7421fcaf541e1b/sql4.png>

### 5. Create a field funding_status that applies the following logic based on the percentage of amount pledged to campaign goal
- If the percentage pledged is greater than or equal to 1, then the project is "Fully funded"
- If the percentage pledged is between 75% and 100%, then the project is "Nearly funded"
- If the percentage pledged is less than 75%, then the project is "Not nearly funded"

````sql
SELECT main_category, backers, pledged, goal, pledged / goal AS pct_pledged,
      CASE WHEN (pledged / goal) >= 1 THEN 'Fully funded'
      WHEN (pledged / goal) < 1 AND (pledged / goal) > .75 THEN 'Nearly funded'
      ELSE 'Not nearly funded' END AS funding_status
FROM ksprojects
WHERE state IN ('failed')
AND backers >= 100 AND pledged >= 20000
ORDER BY main_category, pct_pledged DESC
LIMIT 10
````
<img width="400" alt="image" src=https://github.com/andyalwaysok/Kickstarter-Campaign/blob/e1fc966796c6990a3f45486f1e7421fcaf541e1b/sql5.png>
