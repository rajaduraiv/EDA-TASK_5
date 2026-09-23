# Exploratory Data Analysis (EDA) – Healthcare Dataset

## 1. Project Overview

This project performs Exploratory Data Analysis (EDA) on a healthcare dataset containing patient information, medical conditions, admission details, dates, and billing amounts.

The main purpose of this task is to understand the structure of the dataset, identify missing values, clean the data, perform statistical analysis, analyze relationships between different columns, and visualize important information.

## 2. Dataset Description

The dataset contains **1000 patient records** and initially has **10 columns**.

### Columns

| Column            | Description                                |
| ----------------- | ------------------------------------------ |
| Patient_ID        | Unique patient identification number       |
| Age               | Age of the patient                         |
| Gender            | Gender of the patient                      |
| Blood_Type        | Blood group of the patient                 |
| Medical_Condition | Medical condition of the patient           |
| Medical_Code      | Code associated with the medical condition |
| Date_of_Admission | Date on which the patient was admitted     |
| Discharge_Date    | Date on which the patient was discharged   |
| Admission_Type    | Type of admission                          |
| Billing_Amount    | Amount billed for the patient's treatment  |

After feature engineering, one additional column named `Stay_Date` was created.

## 3. Technologies Used

* Python
* Jupyter Notebook / Google Colab
* Pandas
* NumPy
* Matplotlib
* Seaborn

## 4. Libraries Used

### Code

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

### Purpose

* **Pandas** – Data loading, cleaning, manipulation and analysis.
* **NumPy** – Numerical operations.
* **Matplotlib** – Data visualization.
* **Seaborn** – Statistical data visualization.

## 5. Loading the Dataset

### Code

```python
df = pd.read_csv("/content/healthcare_data_for_task.csv")
```

The healthcare CSV file is loaded into a Pandas DataFrame named `df`.

## 6. Viewing the Dataset

### Code

```python
df.head()
```

### Output

The first five records contain information such as:

```text
Patient_ID  Age  Gender  Blood_Type  Medical_Condition  Medical_Code
P0001       47   Male    AB+         Heart Disease      HEA004
P0002       62   Female  AB-         Diabetes           DIA001
P0003       36   Male    AB-         Heart Disease      HEA004
P0004       51   Male    A+          Heart Disease      HEA004
P0005       30   Male    A+          Diabetes           DIA001
```

The remaining columns include admission date, discharge date, admission type and billing amount.

## 7. Checking Dataset Shape

### Code

```python
df.shape
```

### Output

```text
(1000, 10)
```

This means the dataset contains **1000 rows and 10 columns** before feature engineering.

## 8. Checking Column Names

### Code

```python
df.columns
```

### Output

```text
Index([
'Patient_ID',
'Age',
'Gender',
'Blood_Type',
'Medical_Condition',
'Medical_Code',
'Date_of_Admission',
'Discharge_Date',
'Admission_Type',
'Billing_Amount'
], dtype='object')
```

## 9. Checking Dataset Information

### Code

```python
df.info()
```

### Output Summary

```text
1000 entries
10 columns

Patient_ID          1000 non-null
Age                 1000 non-null
Gender              1000 non-null
Blood_Type          1000 non-null
Medical_Condition   1000 non-null
Medical_Code         970 non-null
Date_of_Admission   1000 non-null
Discharge_Date      1000 non-null
Admission_Type      1000 non-null
Billing_Amount      1000 non-null
```

The `Medical_Code` column contains missing values, while the other columns do not contain missing values.

## 10. Selecting Individual Columns

### Age

```python
df['Age']
```

This displays the age of all patients.

### Medical Code

```python
df['Medical_Code']
```

This displays the medical code associated with each patient.

## 11. Checking Missing Values

### Code

```python
df.isnull().sum()
```

### Output

```text
Patient_ID            0
Age                   0
Gender                0
Blood_Type            0
Medical_Condition     0
Medical_Code         30
Date_of_Admission     0
Discharge_Date        0
Admission_Type        0
Billing_Amount        0
```

There are **30 missing values** in the `Medical_Code` column.

## 12. Handling Missing Values

### Code

```python
df['Medical_Code'] = df['Medical_Code'].fillna('Unknown')
```

The missing values in `Medical_Code` are replaced with the value **`Unknown`**.

This makes the column complete without removing the corresponding patient records.

## 13. Cleaning Admission Type

### Code

```python
df['Admission_Type'] = (
    df['Admission_Type']
    .str.strip()
    .str.title()
)
```

This operation:

* Removes unnecessary spaces using `str.strip()`.
* Converts the values into title case using `str.title()`.

## 14. Checking Unique Admission Types

### Code

```python
df['Admission_Type'].unique()
```

### Output

```text
['Emergency', 'Elective', 'Urgent']
```

There are three admission types in the dataset:

* Emergency
* Elective
* Urgent

## 15. Counting Admission Types

### Code

```python
df['Admission_Type'].value_counts()
```

### Output

```text
Emergency    381
Elective     368
Urgent       251
```

The dataset contains:

* **381 Emergency admissions**
* **368 Elective admissions**
* **251 Urgent admissions**

## 16. Converting Date Columns

### Code

```python
df['Date_of_Admission'] = pd.to_datetime(df['Date_of_Admission'])

df['Discharge_Date'] = pd.to_datetime(df['Discharge_Date'])
```

The admission and discharge columns are converted from object/string format into proper datetime format.

## 17. Extracting Year, Month and Day

### Year

```python
df['Date_of_Admission'].dt.year
```

This extracts the admission year.

### Month

```python
df['Date_of_Admission'].dt.month
```

This extracts the admission month.

### Day

```python
df['Date_of_Admission'].dt.day
```

This extracts the admission day.

These operations help in performing time-based analysis.

## 18. Creating Stay Duration

### Code

```python
df['Stay_Date'] = (
    df['Discharge_Date'] - df['Date_of_Admission']
).dt.days
```

A new column called `Stay_Date` is created.

It represents the number of days between the patient's admission and discharge dates.

## 19. Checking Updated Dataset Information

### Code

```python
df.info()
```

### Output Summary

```text
1000 entries
11 columns

Patient_ID          1000 non-null
Age                 1000 non-null
Gender              1000 non-null
Blood_Type          1000 non-null
Medical_Condition   1000 non-null
Medical_Code        1000 non-null
Date_of_Admission   1000 non-null
Discharge_Date      1000 non-null
Admission_Type      1000 non-null
Billing_Amount      1000 non-null
Stay_Date           1000 non-null
```

After cleaning and feature engineering, the dataset contains **11 columns**.

## 20. Statistical Analysis of Billing Amount

### Code

```python
df['Billing_Amount'].describe()
```

### Output

```text
count      1000.000000
mean      77117.515720
std       41955.242269
min        5568.160000
25%       40318.785000
50%       77293.900000
75%      113089.267500
max      149921.800000
```

This provides the descriptive statistics of the billing amount, including count, mean, standard deviation, minimum, quartiles and maximum.

## 21. Average Age by Medical Condition

### Code

```python
df.groupby('Medical_Condition')['Age'].mean()
```

### Output

```text
Arthritis          52.674641
Asthma             49.919598
Diabetes           53.130653
Heart Disease      52.180995
Hypertension       51.395349
```

This operation calculates the average patient age for each medical condition.

## 22. Cross Tabulation of Medical Condition and Gender

### Code

```python
pd.crosstab(
    df['Medical_Condition'],
    df['Gender']
)
```

### Output

```text
Gender             Female  Male
Medical_Condition

Arthritis              95   114
Asthma                101    98
Diabetes              110    89
Heart Disease         104   117
Hypertension           80    92
```

This cross-tabulation shows the number of male and female patients for each medical condition.

## 23. Data Visualization

### Bar Chart – Admission Type

### Code

```python
df['Admission_Type'].value_counts().plot(kind='bar')
```

### Output

<img width="552" height="502" alt="image" src="https://github.com/user-attachments/assets/1cd93b30-a88c-4556-ae54-6edb3317b29c" />


A bar chart is generated showing the number of patients under each admission type:

```text
Emergency → 381
Elective  → 368
Urgent    → 251
```

The chart provides a visual comparison of the different admission types.

## 24. EDA Operations Performed

The following operations were performed in this EDA task:

1. Imported required Python libraries.
2. Loaded the healthcare CSV dataset.
3. Displayed the first five records.
4. Checked the dataset shape.
5. Checked column names.
6. Examined dataset information and data types.
7. Selected individual columns.
8. Checked missing values.
9. Handled missing values in `Medical_Code`.
10. Cleaned `Admission_Type`.
11. Checked unique admission types.
12. Counted admission types.
13. Converted admission dates to datetime format.
14. Converted discharge dates to datetime format.
15. Extracted year from admission date.
16. Extracted month from admission date.
17. Extracted day from admission date.
18. Created the `Stay_Date` feature.
19. Checked the updated dataset information.
20. Performed descriptive statistical analysis on billing amounts.
21. Calculated average age for each medical condition.
22. Created a cross-tabulation between medical condition and gender.
23. Created a bar chart for admission types.

## 25. Key Findings

* The dataset contains **1000 patient records**.
* Initially, the dataset contains **10 columns**.
* `Medical_Code` had **30 missing values**.
* Missing medical codes were replaced with `Unknown`.
* There are three admission types: **Emergency, Elective and Urgent**.
* Emergency admissions: **381**
* Elective admissions: **368**
* Urgent admissions: **251**
* The average billing amount is approximately **77,117.52**.
* The minimum billing amount is approximately **5,568.16**.
* The maximum billing amount is approximately **149,921.80**.
* A new `Stay_Date` column was created to represent the duration of the patient's stay.
* The final dataset contains **11 columns** after feature engineering.

## 26. Conclusion

This EDA task demonstrates how Python can be used to explore and prepare healthcare data for further analysis. The dataset was inspected, cleaned, missing values were handled, date columns were converted, a new stay-duration feature was created, and statistical and categorical analyses were performed. Visualization was also used to understand admission patterns.

The cleaned dataset can be used for further data analysis, visualization, reporting, or machine learning applications.

## 27. File Structure

```text
EDA_TASK_5/
│
├── EDA_TASK_5.ipynb
├── healthcare_data_for_task.csv
└── README.md
```
