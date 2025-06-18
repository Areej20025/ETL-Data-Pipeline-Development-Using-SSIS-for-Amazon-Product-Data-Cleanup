# ETL Data Pipeline Development Using SSIS for Amazon Product Data Cleanup

This project demonstrates how to clean Amazon product data using **SQL Server Integration Services (SSIS)**. Below is the step-by-step breakdown of the process, including before and after images for each phase.

---

## **Project Overview**

The **ETL pipeline** was developed using **SSIS** to clean Amazon product data. This pipeline handled several key operations:
- **Removing duplicates** in the **ASIN** column.
- **Replacing missing values (NULL)** in key columns such as **reviews**, **price**, **rating**, etc.
- **Removing irrelevant columns** that were not necessary for analysis, like **fastest_delivery_date** and **free_delivery_date**.
- **Exporting data** with missing prices to an **Excel file** and inserting data with valid prices into a **SQL database**.

---

## **Problem Statement**

The dataset provided by Amazon contained several issues that made it unsuitable for analysis:

### 1. **Duplicate ASIN values**:
- **ASIN**, which should be a unique identifier for each product, contained duplicate values.
- This caused multiple entries for the same product.

### 2. **Missing (NULL) values in multiple columns**:
- Columns like **price**, **list_price**, **rating**, **reviews**, **sold_past_month**, **brand**, **free_delivery_date**, and **fastest_delivery_date** contained many **NULL** values.

### 3. **Irrelevant columns**:
- Due to the age of the data, columns like **free_delivery_date** and **fastest_delivery_date** were identified as irrelevant and should be removed.

---

## **Solution Using SSIS**

### **1. Removing Duplicates in ASIN Column**
We used the **Sort** component in the **Data Flow Task** to sort data by **ASIN** and remove duplicates, ensuring only one unique record for each **ASIN**.

### **2. Replacing NULL Values**
We used the **Conditional Split** and **Derived Column** transformations to handle **NULL** values in the following columns:

- **reviews**: Replaced `NULL` with `0`.
- **sold_past_month**: Replaced `NULL` with `"0"`.
- **available_offers**: Replaced `NULL` with `1`.
- **amazon_choice_type**: Replaced `NULL` with `"None"`.
- **brand**: Replaced `NULL` with `"Unknown"`.
- **list_price**: Replaced `NULL` with the value from the **price** column.
- **rating**: Replaced `NULL` with `"N/A"`.

### **3. Removing Irrelevant Columns**
We used **Execute SQL Task** to remove the unnecessary columns **free_delivery_date** and **fastest_delivery_date**. Below is the SQL code used:

```sql
DECLARE @sql AS NVARCHAR(MAX);

-- Check if the 'fastest_delivery_date' column exists and drop it if it does
IF EXISTS (SELECT * FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'NEW' AND COLUMN_NAME = 'fastest_delivery_date')
BEGIN
    SET @sql = 'ALTER TABLE NEW DROP COLUMN fastest_delivery_date';
    EXEC sp_executesql @sql;
END;

-- Check if the 'free_delivery_date' column exists and drop it if it does
IF EXISTS (SELECT * FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'NEW' AND COLUMN_NAME = 'free_delivery_date')
BEGIN
    SET @sql = 'ALTER TABLE NEW DROP COLUMN free_delivery_date';
    EXEC sp_executesql @sql;
END;
