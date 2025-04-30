# Monthly-Cohort-Retention-Analysis

## Table of Content
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Preparation](data-preparation)
- [Exploratory Data Analysis](exploratory-data-analysis)
- [Data Analysis](data-analysis)
- [Results](results)
- [Recommendations](recommendations)
- [Limitations](limitations)
- [Limitations](#limitations)

### Project Overview
This data analysis project focuses on evaluating a company's customer retention performance over time, using the date of users’ first purchases as the starting point. The objective is to analyze and communicate monthly retention trends by cohort, specifically for the following product lines:

- Retail: Cohort-based monthly customer retention
- Restaurant: Cohort-based monthly customer retention

### Data Source
The data comprises two CSV files—first_purchases.csv and purchases.csv—which were merged to generate the extracted_purchases.csv file used for this project.

### Tools
- Excel- Data Cleaning
- SQL- Data Analysis
- Power BI- Visualization

### Data Prepartion
In the initial data preparation phase, we performed the following task;
1. Data loading and inspection
2. Handling missing values
3. Data formatting
4. Data extraction

### Exploratory Data Analysis
EDA involved exploring the extracted_purchases data to answer key question such as:

1. What is the cohort month?
2. What are the months since cohort?
3. What is the retention rate?

### Data Analysis
 - SQL
 - Dialect- (Transact-SQL).

- Import first_purchases.csv
- Import purchases.csv

- Used the query below to extract the needed columns from both datasets through inner join
   - SELECT 
   - first_purchases.first_purchase_date,
   - first_purchases.user_id AS first_purchase_user_id, 
   - [ purchases].user_id AS purchase_user_id,
   - [ purchases].purchase_date,
   - [ purchases].purchase_id,
  - [ purchases].product_line
   
FROM 
     first_purchases
INNER JOIN 
     [ purchases] 
     ON first_purchases.user_id = [ purchases].user_id;

- Saved the new dataset with the name " Extracted_Purchases"
- Confirm the new dataset
	-  select * from [Extracted_purchases]

- Used the new dataset to analyse customers retention "month after month based on when users made their first ever purchase".
  - WITH User_retention AS (
  - SELECT 
  - first_purchase_user_id,  
  - FORMAT(first_purchase_date, 'yyyy-MM') AS cohort_month,  
  - DATEDIFF(MONTH, first_purchase_date, purchase_date) AS months_after_first_purchase,  
  - purchase_user_id,  
  - purchase_id  

    FROM 
        [Extracted_Purchases] 
)

  - SELECT 
  - cohort_month,  
  - months_after_first_purchase,  
  - COUNT(DISTINCT purchase_user_id) AS retained_users,  
  - COUNT(DISTINCT CASE 
  - WHEN months_after_first_purchase = 0 THEN purchase_user_id 
  - ELSE NULL 
  - END) AS first_purchase_users  

    FROM 
    User_retention
GROUP BY 
    cohort_month, 
    months_after_first_purchase
ORDER BY 
    cohort_month, 
    months_after_first_purchase;

### Results
 - Retail Product Line: 

   Retention started relatively strong in April2020 (18.33%), peaked in June 2020 (22.75%), and then showed a sharp and steady decline to just 2.71% by September 2020.
   This suggests early cohorts were better retained, possibly due to stronger product-market fit or effective initial campaigns. However, retention performance deteriorated rapidly, 
   indicating potential issues with customer satisfaction, experience, or follow-up engagement.



 - Restaurant Product Line: 

   Retention was lower overall compared to retail, starting at 6.99% in April, gradually increasing to a modest peak of 8.68% in July, and then declining slightly to 2.98% in September. 
   Although less volatile than retail, the restaurant line also shows signs of waning engagement over time, but its initial retention was already modest.

### Recommendations
Based on the analysis,
- Retail products attracted higher initial retention, but saw a steep fall-off, raising concerns about long-term engagement or customer churn after early success.

- Restaurant products had lower but more stable retention, suggesting a consistently low engagement rather than a sudden drop.

We recommend that,
- Both product lines would benefit from targeted retention strategies, with retail needing long-term engagement improvements and restaurant needing enhanced onboarding and initial value 
  delivery to lift early retention rates.

### Limitations
I consistently matched first_purchases.user_id as first_purchase_user_id and purchases.user_id as purchase_user_id to ensure accurate associations in the merged results, as demonstrated in the script under the Data Analysis section.


    
    

