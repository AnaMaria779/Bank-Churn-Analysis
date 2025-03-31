# 🏦 Bank Churn Analysis - SQL Project  

## 📌 Overview  
This project focuses on analyzing bank customer behavior, financial health, and risk assessment using **MS SQL Server**. It examines **credit utilization, income patterns, spending habits, and churn prediction** to help banks optimize retention strategies, credit risk management, and personalized financial offerings.  

---

## 🚀 Features  
- 📊 **Key Performance Indicators (KPIs):**  
  - **Total Customers**  
  - **Average Utilization Ratio**  
  - **Average Credit Limit**  
  - **Average Age**  
- 📉 **Granular Customer Analysis:**  
  - Segmentation by **card category, age group, and marital status**  
  - **High credit limit but low balance customers**  
  - **Top customers with longest banking relationships**  
  - **High-risk credit utilization segmentation**  

---

## 📂 Dataset  
The dataset contains information about **bank customers**, including:  
- **Demographics:** Age, Gender, Marital Status, Education Level  
- **Financial Status:** Income, Balance, Credit Limit, Utilization Ratio  
- **Banking Behavior:** Card Category, Months on Book, Dependent Count  
- **Churn Indicator:** Identifies whether a customer has left the bank  

---

## 📊 Key Metrics & Visualizations  

### 🔹 Customer Segmentation  
- **Total Customers** (COUNT of unique client numbers)  
- **Customer Distribution by Card Category**  
- **Customers Segmentation by Age Group**  
- **Customers by Marital Status**  

### 🔹 Credit & Financial Insights  
- **Average Credit Limit** (Determining spending power and risk level)  
- **Credit Utilization Analysis** (Identifying high-risk customers)  
- **Customers with High Credit Limit but Low Balance**  
- **Credit Utilization Index (High-Risk vs. Low-Risk Customers)**  

### 🔹 Churn & Retention Analysis  
- **Total Churned Customers** (Percentage of customers who left)  
- **Customers with Longest Relationship (VIP Segmentation)**  
- **Customers with High Credit Utilization (Top 10 Risky Clients)**  

---

## ⚠️ Key Challenges  

1. **Data Quality Issues**  
   - Handling missing or inconsistent customer records.  
   - Standardizing income and financial indicators.  

2. **Churn Prediction Complexity**  
   - Identifying early churn indicators for proactive intervention.  
   - Differentiating between voluntary and involuntary churn.  

3. **Risk Segmentation**  
   - Defining high-risk vs. low-risk customers based on utilization.  
   - Creating actionable retention strategies based on insights.  

---

## 🎯 Expected Outcomes  

✅ **Improved Churn Prediction**: Identifying patterns in customer retention and churn.  
✅ **Better Credit Risk Management**: Segmenting customers based on financial health.  
✅ **Enhanced Customer Retention Strategies**: Detecting dormant accounts and high-value customers.  
✅ **Optimized Marketing Campaigns**: Identifying high-value customers for targeted promotions.  

---

## 🛠️ SQL Queries Used  

### 🏦 1. KPI Metrics  

```sql
-- Total Customers
SELECT COUNT(clientnum) AS Total_Customers FROM bank_churn_data;

-- Average Utilization Ratio
SELECT ROUND(AVG(utilization_ratio), 2) AS Avg_Utilization FROM bank_churn_data;

-- Average Credit Limit
SELECT ROUND(AVG(credit_limit), 2) AS Avg_Credit_Limit FROM bank_churn_data;

-- Average Age
SELECT AVG(customer_age) AS Avg_Age FROM bank_churn_data;
-- Customer Distribution by Card Category
SELECT card_category, COUNT(clientnum) AS Customer_Count 
FROM bank_churn_data 
GROUP BY card_category 
ORDER BY Customer_Count DESC;

-- Customers Segmentation by Age Group
SELECT 
    CASE 
        WHEN customer_age BETWEEN 18 AND 25 THEN '18-25'
        WHEN customer_age BETWEEN 26 AND 35 THEN '26-35'
        WHEN customer_age BETWEEN 36 AND 45 THEN '36-45'
        WHEN customer_age BETWEEN 46 AND 55 THEN '46-55'
        ELSE '56+' 
    END AS Age_Group,
    COUNT(clientnum) AS Total_Customers
FROM bank_churn_data
GROUP BY CASE 
        WHEN customer_age BETWEEN 18 AND 25 THEN '18-25'
        WHEN customer_age BETWEEN 26 AND 35 THEN '26-35'
        WHEN customer_age BETWEEN 36 AND 45 THEN '36-45'
        WHEN customer_age BETWEEN 46 AND 55 THEN '46-55'
        ELSE '56+' 
    END
ORDER BY Total_Customers DESC;
-- Highest Credit Utilization Customers (Top 10)
SELECT TOP 10 
    clientnum, 
    customer_age, 
    income, 
    credit_limit, 
    utilization_ratio
FROM bank_churn_data
ORDER BY utilization_ratio DESC;

-- Customers with High Credit Limit but Low Balance (Top 10)
SELECT TOP 10
    clientnum, 
    customer_age, 
    income, 
    credit_limit, 
    balance, 
    ROUND((balance * 100.0 / credit_limit), 2) AS Balance_Percentage
FROM bank_churn_data
WHERE balance < (0.2 * credit_limit)
ORDER BY Balance_Percentage ASC;
-- Customers with the Longest Relationship (Top 10 by Tenure)
SELECT TOP 10 
    clientnum, 
    customer_age, 
    months_on_book, 
    income, 
    balance
FROM bank_churn_data
ORDER BY months_on_book DESC;

-- Credit Utilization Index (High-Risk Customers)
SELECT 
    clientnum, 
    customer_age, 
    income, 
    credit_limit, 
    utilization_ratio,
    ROUND((utilization_ratio * 100.0), 2) AS Utilization_Percentage,
    CASE 
        WHEN utilization_ratio > 0.8 THEN 'High Risk'
        WHEN utilization_ratio BETWEEN 0.5 AND 0.8 THEN 'Moderate Risk'
        ELSE 'Low Risk' 
    END AS Risk_Level
FROM bank_churn_data
ORDER BY utilization_ratio DESC;

---
 
##📌 Usage  
1. **Load the Spotify dataset into Power BI.**  
2. **Use the interactive slicers** to filter by platform, artist, or track name.  
3. **Explore key insights** in the pre-built dashboards.  
4. **Analyze listening trends and engagement patterns.**  

---

##📚 Contributions  
Contributions are welcome! Feel free to submit issues or pull requests.  

---

##📜 License  
This project is licensed under the **MIT License**.  
