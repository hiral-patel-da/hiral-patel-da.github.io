---
title: "Data Cleaning in SQL"
excerpt: "Short description of portfolio item number 1<br/><img src='/images/500x300.png'>"
collection: portfolio
---

Project Goal
=====

In this project, our goal is to refine the raw Nashville housing data using SQL Server. By transforming the data, we aim to enhance its usability and improve its readability, thereby facilitating more insightful analysis.

Objective
=====

* In this project, I focused on Nashville housing data, refining it through meticulous cleaning to enhance precision and readability, thus optimizing it for detailed analysis.

    - I meticulously cleaned and formatted the raw data using SQL. This included tasks such as standardizing date formats, filling in missing property addresses, splitting address components into separate columns for clarity, converting "Y" and "N" to "Yes" and "No" in the "sold as vacant" column for easier interpretation, removing duplicates to save storage space and improve query speed, and deleting unused columns to streamline query performance. Through these thorough data cleaning steps, I ensured the dataset was accurate and reliable. This allows me to analyze the data confidently, knowing the results will be both meaningful and precise.

Findings
=====

*   In reviewing the Nashville housing data, I've identified areas where the data could benefit from clarification and refinement to ensure its relevance and accuracy.

    - For standardizing date format, I found that column "SaleDate" is in `DATETIME` format where time is of no use.

    - For filling in missing property address, I found that column "ParcelID" is same as column "PropertyAdress" some of the rows in column "PropertyAddress" is null. To fill in the missing property address I performed self- join with column "parcelID" is `EQUAL`(=) and column "UniqueID" is `NOT EQUAL` (<>).
 
    - For splitting address components into separate columns, I found column "PropertyAdrress" and Column "OwnerAddress" has Address, city, and state in one column seperated with a ` delimeter (,) comma`.

    - For converting "Y" and "N" to "Yes" and "No" in the "sold as vacant" column, I found that "Sold as vacant" column has "Y" and "N" as data and thought to make the data more meaningful. 

    - For removing duplicates, I found that dataset has repeated columns which slow down the query process and occupy storage space. 

Solution
=====

* To clean and Format the raw data in SQL, I used the following functions to make data more precised and improve its query performance. 

    - For standardizing date format, I used `CONVERT` syntax to change the datetime format to date format of the column "SaleDate" and then Altered the table to add the converted datetime format into date and then updated the table. 

    - For filling in missing property address, First, I tried understanding where these missing values are located by listing all data and ordering it by "ParcelID". Then, I found other records "(b)" that have valid "PropertyAddress" values for the same "ParcelID", which can be used to fill in the missing ones in records "(a)" where "PropertyAddress" is null with `ISNULL` syntax. Finally, in the third query I updated the dataset by actually replacing the null "PropertyAddress" values in records "(a)" with the corresponding values from records "(b)". 

    - For splitting address components into separate columns, I used string functions like `SUBSTRING` syntax returns the substring from the "PropertyAddress" as per the position returned by the `CHARINDEX`, Then I used `CHARINDEX` to return the substring position of delimeter from the "PropertyAddress", Then I used `LEN` syntax to calculate the length of column "PropertyAddress" and finally I altered the table and update the table and repeated the same steps for the column "OwnerAddress".

    - For converting "Y" and "N" to "Yes" and "No" in the "sold as vacant" column, I used `CASE STATEMENT` to replace "Y" to "Yes" and "N" to "No" to make data more clear anf meaningful.

    - For removing duplicates, I created a `COMMON TABLE EXPRESSION` and gave a `ROW NUMBER` to dataset to figure out how many duplicates are there by using `PARTITION BY` and selecting each columns. After `CTE` is created select all column and use `WHERE` syntax to figure `ROW NUMBER` greater than 1. and remove them using `DELETE` syntax.  