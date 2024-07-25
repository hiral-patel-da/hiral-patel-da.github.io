---
title: "Data science job salary analysis"
excerpt: "<img src='/images/500x300.png'>"
collection: portfolio
---
Project Goal 
====

To gather and analyze salary information from around the world for jobs in Artificial Intelligence (AI), Machine Learning (ML), and Data Science. This dataset will show how much people in these fields earn based on their job, industry, location, and experience level. The goal is to provide clear, useful insights that help companies, employees, and policymakers understand and improve pay practices in AI, ML, and data science globally.

Objective
=====

* Objective 1: Clean and Prepare Data

    - Remove duplicate entries from the dataset.

    - Rename columns experience_level,
     employment_type, and company_size for clarity.

    - Format the salary_in_usd column to include commas and zero decimal places.

* Objective 2: Data Exploration

    - Analyze trends using pivot tables based on job role, industry, location, and experience.

    - Calculate and visualize descriptive statistics (mean, median, range) to identify salary patterns.

* Objective 3: Prepare Detailed Report

    - Provide actionable recommendations for companies, employees, and policymakers.

    - Present findings through clear and effective visualizations.

    - Share the report to facilitate understanding and improvement of pay practices in AI, ML, and data science globally.

Findings and Solution 
====

* Objective 1 - (EXCEL & SQL)

* Objective 2 - Data Exploration (EXCEL & SQL)

* Objective 2 - Exploratory Data Analysis (TABLEAU)

-  Findings for Objective 1: Clean and Prepare Data

 Duplicate Removal: After inspecting the dataset, duplicates were identified based on all fields. A total of 8,261 duplicate entries were found and successfully removed, ensuring each record is unique.

```sql
-- To remove duplicates from the original table we will first create a new table to avoid messing with the raw table. 
-- after generating the new table, we will rename the new table. 

CREATE TABLE [Job Salary Analysis ] .dbo.Data_Scientist_Salary_Analysis_cleaned (
	[work_year] [float] NULL,
	[experience_level] [nvarchar](255) NULL,
	[employment_type] [nvarchar](255) NULL,
	[job_title] [nvarchar](255) NULL,
	[salary] [float] NULL,
	[salary_currency] [nvarchar](255) NULL,
	[salary_in_usd] [float] NULL,
	[employee_residence] [nvarchar](255) NULL,
	[remote_ratio] [float] NULL,
	[company_location] [nvarchar](255) NULL,
	[company_size] [nvarchar](255) NULL
) ON [PRIMARY];

-- to remove duplicates, we will select distinct each column from the raw (old) table and insert it into new (genereated) table.

insert into [Job Salary Analysis ] .dbo.Data_Scientist_Salary_Analysis_cleaned
select distinct * from [Job Salary Analysis ] .dbo.[Data Scientist Salary Analysis];

```

 Column Renaming: The columns experience_level, employment_type, and company_size were improved for better readability. No changes were made to other columns to maintain consistency across the dataset.

```sql
-- We will update the aliases in column experience_level, employment_type, and company_size.

--1) we will first select the column experience_level from the table and change the aliases using CASE, WHEN and THEN statement to better understand the dataset.

select experience_level, 
		(Case when experience_level ='SE' then 'Senior Level'
		when experience_level = 'EN' then 'Entry Level'
		when experience_level = 'MI' then ' Mid Level'
		When experience_level = 'EX' then 'Executive Level'
		END) experience_level
from [Job Salary Analysis ] .dbo.[Data Scientist Salary Analysis]

--- we will update the table by using update syntax for column experience_level.

UPDATE [Job Salary Analysis ] .dbo.[Data Scientist Salary Analysis]
SET experience_level = Case when experience_level ='SE' then 'Senior Level'
		when experience_level = 'EN' then 'Entry Level'
		when experience_level = 'MI' then ' Mid Level'
		When experience_level = 'EX' then 'Executive Level'
		END

--2) we will first select the column employment_type from the table and change the aliases using CASE, WHEN and THEN statement to bettwr understand the dataset.

Select employment_type, 
		(Case when employment_type  ='FT' then 'Full Time'
		when employment_type  = 'PT' then 'Part Time'
		when employment_type  = 'CT' then 'Contract Basis'
		When employment_type  = 'FL' then 'Freelance'
		END) employment_type
from [Job Salary Analysis ] .dbo.[Data Scientist Salary Analysis]

--- we will update the table by using update syntax for column employment_type.

UPDATE [Job Salary Analysis ] .dbo.[Data Scientist Salary Analysis]
SET  employment_type = Case when employment_type  ='FT' then 'Full Time'
		when employment_type  = 'PT' then 'Part Time'
		when employment_type  = 'CT' then 'Contract Basis'
		When employment_type  = 'FL' then 'Freelance'
		END

--3) we will first select the column company_size from the table and change the aliases using CASE, WHEN and THEN statement to bettwr understand the dataset.

Select company_size, 
	(Case when company_size   ='S' then 'Small'
		when company_size  = 'M' then 'Medium'
		when company_size  = 'L' then 'Large'
		END) company_size
From [Job Salary Analysis ] .dbo.[Data Scientist Salary Analysis]

--- we will update the table by using update syntax for column company_size.

UPDATE [Job Salary Analysis ] .dbo.[Data Scientist Salary Analysis]
SET company_size = Case when company_size   ='S' then 'Small'
		when company_size  = 'M' then 'Medium'
		when company_size  = 'L' then 'Large'
		END

```

 Salary Formatting: The salary_in_usd column was formatted to enhance readability. Salaries now display with commas for thousands separators and are rounded to zero decimal places throughout the dataset.

- Findings for Objective 2: Data Exploration

- Distribution of Top 10 Job Titles

The top 10 job titles in AI, ML, and data science roles include positions such as Data Scientist, Machine Learning Engineer, and AI Researcher, with Data Scientist being the most prevalent.

<a href='/images/top-10-job-titles.png' target='_blank'><image src='/images/top-10-job-titles.png' /></a>

- Salary by Employment Type

Full-time positions generally command higher salaries compared to part-time and contract roles in AI, ML, and data science fields.

<a href='/images/salary-by-employment-type.png' target='_blank'><image src='/images/salary-by-employment-type.png' /></a>

-  Salary vs Employee Residence

 Employees located in major tech hubs across countries such as the United States, Canada, and the United Kingdom typically earn higher salaries than those in Countries with smaller cities or rural areas.

 <a href='/images/Salary-vs-residence.png' target='_blank'><image src='/images/Salary-vs-residence.png' /></a>

- Top 10 Countries Data Science Employees Live In

 Data science professionals are predominantly located in the United States, India, and the United Kingdom, reflecting global centers for tech and data-driven industries.

<a href='/images/top-10-country-employee-resides-in.png' target='_blank'><image src='/images/top-10-country-employee-resides-in.png' /></a>

- Distribution of Data Science Jobs by Company Location

 Data science jobs are concentrated in cities with established tech industries, such as United states, United kingdom  and Canada, indicating regional clusters of opportunities.

 <a href='/images/company_location.png' target='_blank'><image src='/images/company_location.png' /></a>       

- Top 5 Highest Salary by Job Title

 Roles like Data Scientist, Data Engineer, Data Analyst, Machine Learning Engineer and Analytics Engineer consistently offer the highest salaries in the field.

<a href='/images/top-5-highest-salary-jobs.png' target='_blank'><image src='/images/top-5-highest-salary-jobs.png' /></a>        

- Most Common Job Titles and Employment Types

 Data Scientist and Machine Learning Engineer are among the most common job titles, predominantly found in full-time employment.

<a href='/images/common-job-title-emplyement-type.png' target='_blank'><image src='/images/common-job-title-emplyement-type.png' /></a>

- Impact of Experience Level and Employment Type on Salary

Experience level significantly influences salary, with senior-level positions commanding higher compensation, particularly in full-time roles compared to part-time or contract positions.

<a href='/images/el-et-impacting-salary.png' target='_blank'><image src='/images/el-et-impacting-salary.png' /></a>

- Salary Changes Over Time (2020-2024)

Salaries in AI, ML, and data science have shown a steady increase from 2020 to 2024, driven by growing demand and technological advancements.

<a href='/images/salary-change-overtime.png' target='_blank'><image src='/images/salary-change-overtime.png' /></a>

- Differences in Salary Trends Between Company Locations or Sizes

 Companies based in high-cost-of-living areas or large corporations generally offer higher salaries compared to startups or companies in lower-cost regions.

<a href='/images/difference-in-salary-by-company-size.png' target='_blank'><image src='/images/difference-in-salary-by-company-size.png' /></a>

- Impact of Remote Work on Salary

 Remote work has led to a shift in salary dynamics, with some professionals earning higher salaries due to global talent competition and others seeing adjustments based on local market conditions.

<a href='/images/remote-work-impacting-salary.png' target='_blank'><image src='/images/remote-work-impacting-salary.png' /></a>

* Exploratory Data Analysis (Tableau)

   1. Distribution of Experience Level

        - The distribution of experience levels among AI, ML, and data science professionals shows that 8.28% are entry-level (<2 years), 25.99% are mid-level (2-5 years), and 62.98% are senior-level (>5 years).

<a href='/' target='_blank'><image src='/' /></a>

    2. Distribution of Work Type

        - The distribution of work types reveals that 99.58% of professionals work full-time, 0.20% work part-time, 0.15% work on a contract basis, and 0.07% are freelancers or consultants.

<a href='/' target='_blank'><image src='/' /></a>

    3. Distribution of Company Size

        - In terms of company size, 0.98% of professionals work in startups, 93.27% in medium-sized companies, and 5.75% in large corporations.

<a href='/' target='_blank'><image src='/' /></a>

    4. Median Salary vs Experience Level

        - The median salary increases with experience level: entry-level professionals earn approximately $80,000 mid-level professionals earn $1,10,707.5 senior-level professionals earn $1,52,500 and Executive-level professionals earn $1,96,000.

<a href='/' target='_blank'><image src='/' /></a>

    5. Median Salary vs Employment Type

        - Median salaries vary by employment type: full-time employees earn $1,33,275 part-time employees earn $58,400 contract workers earn $93,856 and freelancers/consultants earn $36,007.

<a href='/' target='_blank'><image src='/' /></a>

    6. Median Salary vs Experience Level Based on Company Size

        - Startups (1-50 employees):
    
            * Entry-Level: Entry-level professionals in startups earn a median salary of $58,268.

            * Mid-Level: Mid-level professionals in startups earn a median salary of $62,146.

            * Senior-Level: Senior-level professionals in startups earn a median salary of $1,04,080.

            * Executive-level: Executive-level professionals in startups earn a median salary of $1,15,222.

        - Medium-Sized Companies (51-500 employees):

            * Entry-Level: Entry-level professionals in medium-sized companies earn a median salary of $84,086.5.

            * Mid-Level: Mid-level professionals in medium-sized companies earn a median salary of $1,16,259.

            * Senior-Level: Senior-level professionals in medium-sized companies earn a median salary of $1,54,500.

            * Executive-level: Executive-level professionals in medium-sized earn a median salary of $1,51,200.

        - Large Corporations (>500 employees):

            * Entry-Level: Entry-level professionals in large corporations earn a median salary of $59,102.

            * Mid-Level: Mid-level professionals in large corporations earn a median salary of $83,171.

            * Senior-Level: Senior-level professionals in large corporations earn a median salary of $1,30,000.

            * Executive-level: Executive-level professionals in large corporation earn a median salary of $1,53,667.

<a href='/' target='_blank'><image src='/' /></a>

    7. Mean and Median Salary Across the World

        - Globally, the mean salary for AI, ML, and data science professionals in United states is approximately $157,734, with a median salary of $148,500. Salaries vary significantly based on regional economic factors and cost of living.

 <a href='/' target='_blank'><image src='/' /></a>       

    8. Median Salary Based on Experience Level

        - When segmented by experience level, the median salary for entry-level professionals is $83,300, Executive-level professionals is $197,689,  mid-level professionals earn $120,000, and senior-level professionals earn $156,400.

 <a href='/' target='_blank'><image src='/' /></a>       
 <a href='/' target='_blank'><image src='/' /></a>
 <a href='/' target='_blank'><image src='/' /></a>
 <a href='/' target='_blank'><image src='/' /></a>

    9. Top 5 Jobs Based on Median Salary

        - The top-paying job titles based on median salary include AI Research Scientist, Machine Learning Engineer, Data Scientist, Data Engineer and Data Analyst, with median salaries ranging from $176,850 to $101,000.

 <a href='/' target='_blank'><image src='/' /></a>       

    10. Top 10 Job Titles

        - The top 10 most common job titles in AI, ML, and data science include Data Scientist, Data Engineer, Data Analyst,  Machine Learning Engineer, Research Scientist, Applied Scientist, Data Architect, Analytics Engineer, Research Engineer and Business Intelligence Engineer, among others.

<a href='/' target='_blank'><image src='/' /></a>

    11. Company Location Based on Count of Job Titles

        - The distribution of job titles across company locations shows that 88.70% of positions are based in tech hubs such as United states, reflecting global centers for tech and innovation.

 <a href='/' target='_blank'><image src='/' /></a>       

    12. Mean Salary as a Function of Currency

        - Mean salaries converted into different currencies reveal significant variations: professionals earning in USD have a mean salary of $156,711, while those earning in EUR have $64,680, and those in GBP earn $77,758, reflecting currency exchange rates and local economic conditions.

 <a href='/' target='_blank'><image src='/' /></a>       

    13. Mean Salary as a Function of Company Location

        - Mean salaries vary by company location: professionals working in high-cost-of-living countries like United states earn $157,734 , while those in emerging tech markets like India earn $46,298.

<a href='/' target='_blank'><image src='/' /></a>        

- Findings of Objective 3 - Detailed Report

    * For Companies:

        - Competitive Salary Structures: Align salaries with industry standards and regional benchmarks to attract and retain top talent.

        - Remote Work Policies: Implement flexible work arrangements to expand recruitment strategies and access diverse talent pools.

    * For Employees:

        - Continuous Skill Development: Invest in ongoing education and certifications to enhance expertise and qualify for higher-paying roles.

        - Negotiation Strategies: Use salary data insights to negotiate fair compensation packages based on skills and experience.

    * For Policymakers:

        - Policy Advocacy: Support policies that promote salary transparency and equitable pay practices across the tech sector.

        - Education Initiatives: Fund programs that address skill gaps and foster innovation in AI, ML, and data science education.

    * Visualizations

        - Utilize visualizations such as bar charts, line graphs, and geographical maps to:

            1. Illustrate regional salary distributions and variations.

            2. Compare median salaries across job titles and experience levels.

            3. Visualize salary trends over time (2020-2024) and by company size or location.

 <a href='/' target='_blank'><image src='/' /></a>           
<a href='/' target='_blank'><image src='/' /></a>

    * Conclusion

        - This report provides comprehensive insights into salary trends in AI, ML, and data science fields, offering practical recommendations for companies, employees, and policymakers to enhance pay practices and foster a competitive industry landscape. By leveraging these insights, stakeholders can navigate salary negotiations, talent acquisition, and policy development effectively in the evolving tech industry.