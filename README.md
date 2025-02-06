![Alt text](https://github.com/Chetu6474/Netflix_SQL_Project/blob/main/netflix_project/Netflix_LinkdinHeader_N_Texture_5.png?raw=true)
# **Netflix Data Analysis Project**  

# **Netflix Data Analysis Project**  

## **📖 Project Overview**  
This project aims to analyze Netflix data using **PostgreSQL** and **Excel**, with additional **data visualizations** to gain insights.  
The dataset contains over **8,500 records**, and various business problems are addressed using SQL queries and analytical techniques.  

---

## **🛠 Tech Stack Used**  
- **Database**: PostgreSQL  
- **Data Processing**: Excel  
- **Visualization**: Power BI / Tableau  

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
