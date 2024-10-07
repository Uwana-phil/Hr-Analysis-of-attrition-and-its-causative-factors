# Hr-Analysis-of-attrition-and-its-causative-factors

## Table of Contents
- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools](#tools)
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Findings](#findings)
- [Recommendations](#recommendations)
- [Additional General Recommendation](#additional-general-recommendation)
- [Limitations](#limitations)


## Project Overview:
This analysis aims to uncover the key drivers behind employee attrition within an organisation. By carefully examining the workforce data, i identify patterns and trends that may contribute to factors like employee's geographic (gender, age, marital status, tenure), job role, work-life balance,  compensation, training, 

![hr_dashboard2](https://github.com/user-attachments/assets/b8f1801a-d735-4c0f-85ed-f42cb531b2cf)



### Data Source:
HR Data: The primary dataset used for this analysis is the "HR_df.csv" file containing information of a private organisation.

### Tools:
The tools used for this analysis are;
- Python - For Data cleaning and Analysis
  - [Download Here](https://www.python.org/downloads/)
- Tableau - For creating reports and Visualization
  - [Download Here](https://public.tableau.com/app/profile/philips.jeffrey/viz/HRanalysisandthecausativefactorsofattrition/Dashboard2)

### Data Cleaning and Preparation:
In the initial stage of the data preparation phase, we perform the following tasks:
1. Import Libraries for the Analysis
2. Load and Inspect the dataset
3. Check for null/missing values
4. Check the data structure and info
5. Check for duplicated values
6. Check the data types
7. inspect the columns
   - To expunge columns that won't be needed for the analysis

### Exploratory Data Analysis:
The EDA involves exploring the dataset to answer key questions, such as;
- What is the total number of employees?
- What is the attrition rate?
- Under which age range has the highest attrition rate?
- Which department has the highest attrition rate?
- Do employees' geographics (gender, age, marital status, tenure) contribute to a high attrition rate?
- Which job role has a high rate of attrition?
- Does inadequate training contribute to attrition?

### Data Analysis:
Include some interesting codes/features worked with

#### Importing Libraries for the analysis and Loading the Dataset
```Python
import pandas as pd
import numpy as np
import warnings
warnings = warnings.filterwarnings('ignore') #to ignore any warning output that may occur
```
```python
hrdata = pd.read_csv("C:/Users/PHILIPS/Documents/HR_df.csv")
hrdata.head(3) # viewing the top 3 rows
```
#### Inspect, Clean, and Preprocess the dataset for analysis
```python
# To know the size of the dataset
nRow,nCol = hrdata.shape
print(f'the dataset consist of {nRow} rows and {nCol} columns')
```

```python
#Check the dataset for duplucated values
hrdata.duplicated().sum()
```

```python
#To know the datatypes and uniqueness of the attributes
hrdata.info()
```

```python
#To check for null values in the dataset
hrdata.isnull().sum()
```
#### Data Cleaning:
```python
#Lets call the dataset to display only 3 rows
hrdata.head(3)
```
```python
# Listing all the columns of the datset
[hr.columns]
```
```python
#Lets drop the Unnamed column using the if statement
if 'Unnamed: 0' in hrdata.columns:
    hrdata = hrdata.drop(['Unnamed: 0'], axis=1)
```

```python
#we noticed 'nan'values in the columns, so we will remove them by using the def function to find the mean values of the corresponding columns
def hr(x):
    mean = round(x.mean(),1)
    meanhr = mean
    return meanhr
hrmean = hr(hrdata['NumCompaniesWorked'])
print(hrmean)
```

```python
hrdata['TotalWorkingYears'].mean().round(2)
```

```python
#fill the null colums with the mean value of the corresponding columns
hrdata['NumCompaniesWorked'] = hrdata['NumCompaniesWorked'].fillna(2.7)
hrdata['TotalWorkingYears'] = hrdata['TotalWorkingYears'].fillna(11.3)
```

#### Analyzing the Dataset
```python
#call the dataset
hrdata.head(3)
```

```python
# Let know the total number of employees in the organisation
hrdata['EmployeeID'].value_counts().sum()
```

```python
#Lets know the attributes of the attrition columns
hrdata['Attrition'].unique()
```

```python
# Seperate all attrited employees in the organisation from the active employees. 
attrition_yes = hrdata[(hrdata['Attrition'] == 'Yes')]
attrition_yes.head(3)
```

```python
attrition_no = hrdata[(hrdata['Attrition'] == 'No')]
attrition_no.head(3)
```

```python
# Lets know the count of employees that left the company
attrition_yes['Attrition'].value_counts()
```

```python
#count of active employees
attrition_no['Attrition'].value_counts()
```

```python
# Age range of employees
maxRange,minRange = attrition_yes['Age'].max(),attrition_yes['Age'].min()
print(f'Max age is {maxRange}, while the min age is  {minRange}')
```

```python
maxRange1,minRange1 = attrition_no['Age'].max(),attrition_No['Age'].min()
print(f'max age of employees still in the company is {maxRange1}, min age is {minRange1}')
```
- attrition based on employee age
```python
#Average age of attrited employees
employee_ave = attrition_yes['Age'].mean().round(2)
employee_ave
```

```python
# Average age of active employees
employee_ave1 = attrition_no['Age'].mean().round(2)
employee_ave1
```
- Gender-based attrition
```python
# Gender-based attrition
employee_Mgender = attrition_yes['Gender'].value_counts()
employee_Mgender
```
```python
# Attrition based on marital status
employee_Mstatus = attrition_yes['MaritalStatus'].value_counts()
employee_Mstatus
```
- Income-wise attrition
```python
# Average income of attrited employees based on their marital status
attrition_yes_income = attrition_yes.groupby('MaritalStatus')['MonthlyIncome'].mean().round(2)
attrition_yes_income = attrition_yes_income.sort_values(ascending = False)
attrition_yes_income
```
```python
# Average income of active employees based on their marital status
attrition_no_income = attrition_no.groupby('MaritalStatus')['MonthlyIncome'].mean().round(2)
attrition_no_income = attrition_no_income.sort_values(ascending = False)
attrition_no_income
```

```python
# Average Income of attrited employees based on Gender
income_gender = attrition_yes.groupby('Gender')['MonthlyIncome'].mean().round(2)
income_gender = income_gender.sort_values(ascending = False)
income_gender
```
```python
# Average Income of active employees based on Gender
income_gender1 = attrition_no.groupby('Gender')['MonthlyIncome'].mean().round(2)
income_gender1 = income_gender1.sort_values(ascending = False)
income_gender1
```
- Department-wise attrition
```python
# number of attrited employees in each department
attrition_yes['Department'].value_counts()
```
```python
# Gender of employees that left the company in each department
gender_in_each_dept = attrition_yes.groupby('Department')['Gender'].value_counts()
gender_in_each_dept
```

### Findings:
The analysis results are summarised as follows
- The total employee count of the organization is 2000 with 325 attrition count.
- The organization has a high attrition rate of 16.3%
- Employee's age range for high attrition is between 18  and 39 which are adults.
- Male employees have a higher attrition rate (63%)
- over 70% of employees between age 18 and 39 receive training less than 4 times out of 6 in a year.
- Over 50% of attrition are singles.
- Research Scientists, Lab Technicians, and Sales Executives are the employees with an increase in attrition.
- Attrited employees receive less income/compensation

### Recommendations:
Based on the analysis, we recommend the following actions;
- High Overall Attrition Rate (16.3%)
  - Develop a comprehensive employee retention strategy focused on both short-term and long-term solutions. This could include improving employee engagement, offering competitive compensation, and fostering a positive work environment.
  - Conduct employee surveys, focus groups, and exit interviews to understand why employees leave. Regularly review and adjust HR policies based on feedback.
- High Attrition Among Employees Aged 18-39
  - Implement targeted programs that address the specific needs of younger employees (aged 18-39).
  - Offer more career development opportunities, mentorship programs, and clear paths for promotion to make the roles more attractive to younger employees.
  - Assess whether these employees feel their skills are underutilized and if they perceive a lack of future growth.
- Male Employees Have a Higher Attrition Rate (63%)
  - Investigate the specific challenges faced by male employees, including work-life balance, job satisfaction, and compensation.
  - Create programs to address gender-specific concerns. Conduct surveys to understand what motivates male employees and identify specific workplace culture improvements that might help reduce attrition among males.
  - Break down attrition rates by job roles and seniority levels to see if there are specific departments or roles where male employees are more likely to leave.
- Lack of Training Among Employees Aged 18-39
  - Increase the frequency and quality of training programs, particularly for employees aged 18-39
  - Create personalized training plans and ensure that employees in this age group receive at least 6 training sessions per year. Introduce digital learning platforms to provide on-demand training opportunities that employees can take advantage of at their own pace.
- High Attrition Among Single Employees (Over 50%)
  - Implement policies that cater to the work-life balance and well-being of single employees.
  - Offer flexible working hours, remote work options, or social programs that help single employees feel more connected to the company. Consider benefits such as gym memberships, wellness programs, or social clubs that may enhance their work-life satisfaction.
- Job Roles with Increased Attrition: Research Scientists, Lab Technicians, and Sales Executives
  - Review the job satisfaction levels, workload, and career growth opportunities for these roles.
  - Conduct focused interviews with employees in these roles to understand the root causes of their dissatisfaction. Consider revising job responsibilities, offering more competitive compensation, or improving promotion opportunities within these roles.
  - Compare their compensation, workload, and benefits with industry standards to ensure competitiveness.
- Lower Compensation for Attrited Employees
  - Address income inequality and improve compensation structures, especially for employees in critical roles or high-risk groups (e.g., Research Scientists, Lab Technicians, Sales Executives)
  - Regularly conduct salary benchmarking to ensure that compensation is competitive within the industry. Consider implementing performance-based bonuses or incentives to motivate and retain employees.
### Additional General Recommendation:
  1. Employee Engagement Programs
     - Launch engagement programs to improve employee satisfaction, communication, and sense of belonging. Regularly monitor morale through engagement surveys
  2. Work-Life Balance Initiatives
     - Explore initiatives like flexible schedules, remote work options, or employee well-being programs to reduce stress and improve overall employee satisfaction.

### Limitations:
- I had to remove irrelevant columns in the dataset to arrive at an accurate result
- I also fill the null values because they may affect the accuracy of my findings.
- Attributes like 'job performance' are omitted from the dataset but then we still arrived at an accurate conclusion.

😙
😄
