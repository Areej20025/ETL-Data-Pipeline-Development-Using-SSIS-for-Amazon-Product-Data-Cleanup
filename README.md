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

The data provided by Amazon contained several issues that made it unsuitable for analysis:
1. **Duplicate ASIN values**: The **ASIN** (Amazon Standard Identification Number) column, which should uniquely identify each product, contained duplicate entries.
2. **Missing values (NULL)**: Key columns like **price**, **rating**, and **reviews** had missing values, making it difficult to perform meaningful analysis.
3. **Irrelevant columns**: Certain columns like **fastest_delivery_date** and **free_delivery_date** were not needed for the analysis and were unnecessary for the final dataset.
4. **Data inconsistency**: The data had inconsistencies, such as different formats for price, which needed to be standardized.

---

## **Solution**

The solution involved using **SSIS** to clean and transform the data through the following steps:

1. **Data Import**: Data was imported from an **Excel file**.
2. **Data Cleaning**:
   - **Removing duplicates** based on the **ASIN** column.
   - **Replacing missing values** in important columns with default values (e.g., `0` for **reviews**, `"N/A"` for **rating**).
   - **Removing unnecessary columns** that did not contribute to the analysis.
3. **Data Transformation**: The data was split into two groups:
   - Rows with valid **price** values were inserted into the database.
   - Rows with missing **price** values were exported to an **Excel** file for further review.
4. **Data Loading**: Cleaned data was loaded into a **SQL database** for future use.

---

## **1. Data Before Cleaning in Excel**

The data before cleaning contained several issues that needed to be addressed, such as missing values, duplicate rows, and irrelevant columns.

![Before Cleaning Excel Data](./images/Before_Cleaning_Excel_Data.png)

**Explanation:**
- **Missing values** in **price**, **list_price**, **reviews**, **sold_past_month**, and **brand**.
- **Duplicate entries** for the same product identified by **ASIN**.
- **Irrelevant columns** like **fastest_delivery_date** and **free_delivery_date**.

---

## **2. Data After Cleaning in Database**

After applying cleaning techniques, the data became structured and consistent in the database. Below is a snapshot of the cleaned data.

![After Cleaning Database Data](./images/After_Cleaning_Database_Data.png)

**Explanation:**
- **Duplicate rows** have been removed.
- **Missing values** have been replaced with default values like `0` or `"N/A"`.
- **Irrelevant columns** such as **fastest_delivery_date** and **free_delivery_date** were removed.

---

## **3. SSIS Process Overview**

### 3.1 **SSIS Control Flow After Running**

This image shows the **Control Flow** after running the SSIS package. The **Control Flow** orchestrates the overall data cleaning and transformation process.

![SSIS Control Flow After Running](./images/SSIS_Control_Flow_After_Running.png)

**Explanation:**
- **Delete Data from Products Table**: Removes all data from the **products** table before inserting new data.
- **ETL Process for Amazon Product Data**: The main ETL process that extracts, transforms, and loads data into the database.
- **Remove Unnecessary Columns from Products**: Removes irrelevant columns from the **products** table, such as **fastest_delivery_date** and **free_delivery_date**.

---

### 3.2 **SSIS Data Flow Process**

This image shows the detailed **Data Flow** process in SSIS, which defines the transformations and data routing steps.

![SSIS Data Flow Process](./images/SSIS_Data_Flow_Process.png)

**Explanation:**
- **Import Data from Excel**: Import the product data from an Excel file using **Excel Source**.
- **Sort Data by ASIN and Remove Duplicates**: Use the **Sort** transformation to remove duplicate rows based on the **ASIN** column.
- **Replace Null Values**: Replace null values in key columns like **reviews**, **sold_past_month**, and **rating** using the **Derived Column** transformation.
- **Split Rows Based on Null or Non-Null Price Values**: Split the data into two paths, one for rows with **NULL** in **price** and another for rows with non-null **price** values using the **Conditional Split** transformation.
- **Export Rows with Null Price to Excel**: Export rows with null **price** to an Excel file using **Excel Destination**.
- **Insert Data into Database Table**: Insert rows with non-null **price** into the database using **OLE DB Destination**.

---

### 3.3 **SSIS Import Data from Excel**

This image shows the **Excel Source** configuration for importing data from the Excel file.

![SSIS Import Data from Excel](./images/SSIS_Import_Data_Excel.png)

**Explanation:**
- **Excel Connection Manager** is used to connect to the Excel file and specify the sheet (`'Amazon_products - Copy$'`) from which the data is imported.

---

### 3.4 **SSIS Replace Null Values**

This image shows the **Derived Column Transformation** used to replace **NULL** values in columns.

![SSIS Replace Null Values](./images/SSIS_Replace_Null_Values.png)

**Explanation:**
- **NULL** values in columns like **reviews**, **sold_past_month**, and **rating** are replaced with default values like `0` or `"N/A"` using **Derived Column Transformation**.

---

### 3.5 **SSIS Sort and Remove Duplicates**

This image shows the **Sort Transformation** used to sort data by **ASIN** and remove duplicates.

![SSIS Sort and Remove Duplicates](./images/SSIS_Sort_And_Remove_Duplicates.png)

**Explanation:**
- The **Sort** transformation is applied to the **ASIN** column, ensuring that each product is unique by removing duplicate rows.

---

### 3.6 **SSIS Split Rows Conditional Split**

This image shows the **Conditional Split** transformation, which splits the data based on **NULL** or **Non-NULL** values in the **price** column.

![SSIS Split Rows Conditional Split](./images/SSIS_Split_Rows_Conditional_Split.png)

**Explanation:**
- The data is split into two paths: one for rows where **price** is **NULL** and another for rows where **price** is **Non-NULL**.

---

## **4. Running the SSIS Project**

This image shows what happens when the **Run** button is clicked in SSIS.

![SSIS Run Project](./images/SSIS_Run_Project.png)

**Explanation:**
- This is the final step where the SSIS package runs, executing all transformations and loading the cleaned data into the database.

---

## **5. Duplicate ASIN Issue in Data**

This image shows the issue with duplicate **ASIN** values before cleaning.

![Duplicate ASIN Issue in Data](./images/Duplicate_ASIN_Issue_in_Data.png)

**Explanation:**
- **ASIN** should be unique for each product, but the data contains multiple rows for the same **ASIN**. This issue is resolved during the SSIS process by removing duplicates.

---

## **Conclusion**

This project demonstrates how to clean and transform Amazon product data using **SSIS**. The **ETL process** ensures that the data is cleaned, transformed, and loaded correctly into the database for further analysis.
