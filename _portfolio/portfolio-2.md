---
title: "Data Cleaning in SQL"
excerpt: "<img src='/images/data-cleaning-housing-profile.png'>"
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

 For standardizing date format, I used `CONVERT` syntax to change the datetime format to date format of the column "SaleDate" and then Altered the table to add the converted datetime format into date and then updated the table. 

``` sql
--Standardize Date format

select SaleDate, CONVERT(Date,SaleDate)
from [Portfolio Project 1].dbo.NashvilleHousing

Update [Portfolio Project 1].dbo.NashvilleHousing
SET SaleDate = CONVERT(Date,SaleDate)

-- it didn't update the table so, we will work on altering the table first

Alter Table [Portfolio Project 1].dbo.NashvilleHousing
Add SaleDateConverted Date;

UPDATE [Portfolio Project 1].dbo.NashvilleHousing
SET SaleDateConverted = CONVERT(Date,SaleDate)

select SaleDateConverted, CONVERT(Date,SaleDate)
from [Portfolio Project 1].dbo.NashvilleHousing

```

 For filling in missing property address, First, I tried understanding where these missing values are located by listing all data and ordering it by "ParcelID". Then, I found other records "(b)" that have valid "PropertyAddress" values for the same "ParcelID", which can be used to fill in the missing ones in records "(a)" where "PropertyAddress" is null with `ISNULL` syntax. Finally, in the third query I updated the dataset by actually replacing the null "PropertyAddress" values in records "(a)" with the corresponding values from records "(b)". 

``` sql
-- populate property address data

select *  
from [Portfolio Project 1].dbo.NashvilleHousing
--where PropertyAddress is null
order by ParcelID 

select a.ParcelID, a.PropertyAddress, b.ParcelID, b.PropertyAddress, ISNULL(a.PropertyAddress,b.PropertyAddress)  
from [Portfolio Project 1].dbo.NashvilleHousing a
join [Portfolio Project 1].dbo.NashvilleHousing b
	on a.ParcelID = b.ParcelID 
	and a.[UniqueID ] <> b.[UniqueID ] 
where a.PropertyAddress is null

Update a 
SET PropertyAddress = ISNULL(a.PropertyAddress,b.PropertyAddress)
from [Portfolio Project 1].dbo.NashvilleHousing a
join [Portfolio Project 1].dbo.NashvilleHousing b
	on a.ParcelID = b.ParcelID 
	and a.[UniqueID ] <> b.[UniqueID ] 
where a.PropertyAddress is null

```

 For splitting address components into separate columns, I used string functions like `SUBSTRING` syntax returns the substring from the "PropertyAddress" as per the position returned by the `CHARINDEX`, Then I used `CHARINDEX` to return the substring position of delimeter from the "PropertyAddress", Then I used `LEN` syntax to calculate the length of column "PropertyAddress" and finally I altered the table and update the table and repeated the same steps for the column "OwnerAddress".

``` sql
-- Breaking out Address into Individual Columns (Address, City, State)

select PropertyAddress  
from [Portfolio Project 1].dbo.NashvilleHousing
--where PropertyAddress is null
--order by ParcelID 

SELECT 
SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress)-1) as Address,
SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1, LEN(PropertyAddress)) as Address
FROM [Portfolio Project 1].dbo.NashvilleHousing

Alter Table [Portfolio Project 1].dbo.NashvilleHousing
Add PropertySplitAddress Nvarchar(255);

UPDATE [Portfolio Project 1].dbo.NashvilleHousing
SET PropertySplitAddress = SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress)-1)

Alter Table [Portfolio Project 1].dbo.NashvilleHousing
Add PropertySplitCity Nvarchar(255);

UPDATE [Portfolio Project 1].dbo.NashvilleHousing
SET PropertySplitCity = SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1, LEN(PropertyAddress)) 

Select * 
from [Portfolio Project 1].dbo.NashvilleHousing


Select OwnerAddress  
from [Portfolio Project 1].dbo.NashvilleHousing

Select 
PARSENAME(REPLACE(OwnerAddress,',','.') ,3),
PARSENAME(REPLACE(OwnerAddress,',','.') ,2),
PARSENAME(REPLACE(OwnerAddress,',','.') ,1)
from [Portfolio Project 1].dbo.NashvilleHousing

Alter Table [Portfolio Project 1].dbo.NashvilleHousing
Add OwnerSplitAddress Nvarchar(255);

UPDATE [Portfolio Project 1].dbo.NashvilleHousing
SET OwnerSplitAddress = PARSENAME(REPLACE(OwnerAddress,',','.') ,3)
 
Alter Table [Portfolio Project 1].dbo.NashvilleHousing
Add OwnerSplitCity Nvarchar(255);

UPDATE [Portfolio Project 1].dbo.NashvilleHousing
SET OwnerSplitCity = PARSENAME(REPLACE(OwnerAddress,',','.') ,2) 

Alter Table [Portfolio Project 1].dbo.NashvilleHousing
Add OwnerSplitState Nvarchar(255);

UPDATE [Portfolio Project 1].dbo.NashvilleHousing
SET OwnerSplitState = PARSENAME(REPLACE(OwnerAddress,',','.') ,1)

Select * 
from [Portfolio Project 1].dbo.NashvilleHousing

```

 For converting "Y" and "N" to "Yes" and "No" in the "sold as vacant" column, I used `CASE STATEMENT` to replace "Y" to "Yes" and "N" to "No" to make data more clear anf meaningful.

```sql
-- Change Y and N to Yes and No in "Sold as Vacant" field

select Distinct(SoldAsVacant), COUNT(SoldAsVacant)
from [Portfolio Project 1].dbo.NashvilleHousing
Group by SoldAsVacant 
order by 2


--Case Statement
Select SoldAsVacant,
Case when SoldAsVacant = 'Y' THEN 'Yes'
	when SoldAsVacant = 'N' THEN 'No'
	Else SoldAsVacant 
	END
from [Portfolio Project 1].dbo.NashvilleHousing

UPDATE [Portfolio Project 1].dbo.NashvilleHousing
SET SoldAsVacant  = Case when SoldAsVacant = 'Y' THEN 'Yes'
					when SoldAsVacant = 'N' THEN 'No'
					Else SoldAsVacant 
					END
```

For removing duplicates, I created a `COMMON TABLE EXPRESSION` and gave a `ROW NUMBER` to dataset to figure out how many duplicates are there by using `PARTITION BY` and selecting each columns. After `CTE` is created select all column and use `WHERE` syntax to figure `ROW NUMBER` greater than 1. and remove them using `DELETE` syntax. 

``` sql
 -- Remove Duplicates

With RowNumCTE AS(
Select *,
	ROW_NUMBER() OVER(PARTITION BY ParcelID, PropertyAddress, SalePrice, SaleDate, LegalReference ORDER BY UniqueID) row_num
from [Portfolio Project 1].dbo.NashvilleHousing
--Order by ParcelID 
)
Select *
From RowNumCTE 
Where row_num > 1
Order by PropertyAddress 

 -- to remove Duplicate found 

With RowNumCTE AS(
Select *,
	ROW_NUMBER() OVER(PARTITION BY ParcelID, PropertyAddress, SalePrice, SaleDate, LegalReference ORDER BY UniqueID) row_num
from [Portfolio Project 1].dbo.NashvilleHousing
--Order by ParcelID 
)
Delete
From RowNumCTE 
Where row_num > 1

```

# References

- <https://www.youtube.com/watch?v=8rO7ztF4NtU&list=PLUaB-1hjhk8H48Pj32z4GZgGWyylqv85f&index=3>

- <https://www.kaggle.com/datasets/tmthyjames/nashville-housing-data>

 