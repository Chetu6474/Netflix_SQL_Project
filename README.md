![Alt text](https://github.com/Chetu6474/Netflix_SQL_Project/blob/main/netflix_project/Netflix_LinkdinHeader_N_Texture_5.png?raw=true)
# **Netflix Data Analysis Project**  

## **📖 Project Overview**  
This project aims to analyze Netflix data using **PostgreSQL** and **Excel**, with additional **data visualizations** to gain insights.  
The dataset contains over **8,500 records**, and various business problems are addressed using SQL queries and analytical techniques.  

---

## **🛠 Tech Stack Used**  
- **Database**: PostgreSQL  
- **Data Processing**: Excel   

---

## **📌 Project Workflow**  

### **Step 1: Database Creation & Data Import**  
1. Created a PostgreSQL database named **`Netflix_db`**.  
2. Defined table names and assigned appropriate **data types** to store values.  
3. Imported data from a CSV file into the database.  
4. Performed initial exploration to understand the dataset before diving into business problems.  

---

## **📊 Business Problems & Solutions**  

### **Problem 1: Count the number of Movies vs TV Shows**  
#### **📜 Code Implementation**  
```sql
SELECT type,
 	COUNT(*) as total
FROM netflix
GROUP BY type;
```

### **Problem 2: Find the most common rating for movies and TV shows**  
#### **📜 Code Implementation**  
```sql
WITH RatingCounts AS (
    SELECT 
        type,
        rating,
        COUNT(*) AS rating_count
    FROM netflix
    GROUP BY type, rating
),
RankedRatings AS (
    SELECT 
        type,
        rating,
        rating_count,
        RANK() OVER (PARTITION BY type ORDER BY rating_count DESC) AS rank
    FROM RatingCounts
)
SELECT 
    type,
    rating AS most_frequent_rating
FROM RankedRatings
WHERE rank = 1;
```

### **Problem 3: List all movies released in a specific year (e.g., 2020)**  
#### **📜 Code Implementation**  
```sql
SELECT * 
FROM netflix
WHERE release_year = 2020;
```

### **Problem 4: Find the top 5 countries with the most content on Netflix**  
#### **📜 Code Implementation**  
```sql
SELECT * 
FROM
(
    SELECT 
        UNNEST(STRING_TO_ARRAY(country, ',')) AS country,
        COUNT(*) AS total_content
    FROM netflix
    GROUP BY 1
) AS t1
WHERE country IS NOT NULL
ORDER BY total_content DESC
LIMIT 5;
```

### **Problem 5: Identify the longest movie**  
#### **📜 Code Implementation**  
```sql
SELECT 
    *
FROM netflix
WHERE type = 'Movie'
ORDER BY SPLIT_PART(duration, ' ', 1)::INT DESC;
```

### **Problem 6: Find content added in the last 5 years**  
#### **📜 Code Implementation**  
```sql
SELECT *
FROM netflix
WHERE TO_DATE(date_added, 'Month DD, YYYY') >= CURRENT_DATE - INTERVAL '5 years';
```

### **Problem 7: Find all the movies/TV shows by director 'Rajiv Chilaka'**  
#### **📜 Code Implementation**  
```sql
SELECT *
FROM (
    SELECT 
        *,
        UNNEST(STRING_TO_ARRAY(director, ',')) AS director_name
    FROM netflix
) AS t
WHERE director_name = 'Rajiv Chilaka';
```

### **Problem 8: List all TV shows with more than 5 seasons**  
#### **📜 Code Implementation**  
```sql
SELECT *
FROM netflix
WHERE type = 'TV Show'
  AND SPLIT_PART(duration, ' ', 1)::INT > 5;
```

### **Problem 9: Count the number of content items in each genre**  
#### **📜 Code Implementation**  
```sql
SELECT 
    UNNEST(STRING_TO_ARRAY(listed_in, ',')) AS genre,
    COUNT(*) AS total_content
FROM netflix
GROUP BY 1;
```

### Problem 10: Find top 5 years with highest average content release in India**  
#### **📜 Code Implementation**  
```sql
SELECT 
    country,
    release_year,
    COUNT(show_id) AS total_release,
    ROUND(
        COUNT(show_id)::numeric /
        (SELECT COUNT(show_id) FROM netflix WHERE country = 'India')::numeric * 100, 2
    ) AS avg_release
FROM netflix
WHERE country = 'India'
GROUP BY country, release_year
ORDER BY avg_release DESC
LIMIT 5;
```

### **Problem 11: List all movies that are documentaries**  
#### **📜 Code Implementation**  
```sql
SELECT * 
FROM netflix
WHERE listed_in LIKE '%Documentaries';
```

### **Problem 12: Find all content without a director**  
#### **📜 Code Implementation**  
```sql
SELECT * 
FROM netflix
WHERE director IS NULL;
```

### **Problem 13: Find how many movies actor 'Salman Khan' appeared in last 10 years!**  
#### **📜 Code Implementation**  
```sql
SELECT * 
FROM netflix
WHERE casts LIKE '%Salman Khan%'
  AND release_year > EXTRACT(YEAR FROM CURRENT_DATE) - 10;
```

### **Problem 14: Find the top 10 actors who have appeared in the highest number of movies produced in India.**  
#### **📜 Code Implementation**  
```sql
SELECT 
    UNNEST(STRING_TO_ARRAY(casts, ',')) AS actor,
    COUNT(*)
FROM netflix
WHERE country = 'India'
GROUP BY actor
ORDER BY COUNT(*) DESC
LIMIT 10;
```

### **Problem 15: Categorize the content based on the presence of the keywords 'kill' and 'violence' in the description field. Label content containing these keywords as 'Bad' and all other content as 'Good'. Count how many items fall into each category.**  
#### **📜 Code Implementation**  
```sql
SELECT 
    category,
    COUNT(*) AS content_count
FROM (
    SELECT 
        CASE 
            WHEN description ILIKE '%kill%' OR description ILIKE '%violence%' THEN 'Bad'
            ELSE 'Good'
        END AS category
    FROM netflix
) AS categorized_content
GROUP BY category;
```
