# **Power BI Salary Analysis**

## **Overview of the Dataset**

This dataset contains detailed information about employees, their experience levels, salaries, and other professional and demographic attributes. The dataset aims to provide insights into various factors that influence salaries in the data science field, such as experience level, employment type, company size, region, and remote work flexibility.

### **Key Columns and Their Definitions**:

1. **Work Year (work year)**:  
   - **Definition**: The calendar year in which the salary data was recorded.
   - **Examples**: 2020, 2021, 2022, 2023.

2. **Experience Level (experience level)**:  
   - **Definition**: The level of experience of an employee.
   - **Categories**:
     - **EN**: Entry-level (junior employees).
     - **MI**: Mid-level (moderately experienced employees).
     - **SE**: Senior-level (experienced professionals).
     - **EX**: Executive-level (leaders or veterans in the field).

3. **Employment Type (employment type)**:  
   - **Definition**: The type of employment contract or working condition.
   - **Categories**:
     - **FT**: Full-time
     - **PT**: Part-time
     - **CT**: Contract-based
     - **FL**: Freelance

4. **Job Title (job title)**:  
   - **Definition**: The specific role of the employee within the organization.
   - **Examples**: Data Scientist, Product Manager, etc.

5. **Salary (salary)**:  
   - **Definition**: A numeric column representing the annual salary of employees, in the currency specified in the `Salary Currency` column.

6. **Salary Currency (salary currency)**:  
   - **Definition**: The currency in which the salary value was recorded.
   
7. **Salary in USD (salary usd)**:  
   - **Definition**: A numeric column representing the annual salary of employees, converted to USD for consistency.

8. **Employee Residence (employee residence)**:  
   - **Definition**: The country in which the employee resides, which may differ from the company's location.
   - **Examples**: USA, India, Spain, Brazil, etc.

9. **Remote Ratio (remote ratio)**:  
   - **Definition**: Specifies the percentage of work that can be done remotely for the position. It indicates the degree of remote work flexibility allowed by the employer.
   - **Possible Values**:
     - **0**: Fully on-site work.
     - **50**: Hybrid work setup (partial remote and on-site).
     - **100**: Fully remote work.

10. **Company Location (company location)**:  
   - **Definition**: The country where the employing company is located.
   - **Examples**: USA, UK, Germany, Canada, etc.

11. **Company Size (company size)**:  
   - **Definition**: Refers to the size of the company based on the number of employees, providing insight into the scale of the organization.
   - **Possible Values**:
     - **S (Small)**: Less than 50 employees.
     - **M (Medium)**: Between 50 and 250 employees.
     - **L (Large)**: More than 250 employees.

---

 **Country Abbreviations**  
The dataset also includes a list of country abbreviations for easy reference. Below are some examples:  
- **CL**: Chile  
- **JP**: Japan  
- **US**: United States  
- **IN**: India  
- **FR**: France  
- **DE**: Germany  
- **GB**: United Kingdom  
- **AU**: Australia  
- **CA**: Canada  
- **BR**: Brazil  
- (and many others)

---

## **Key Questions Addressed by the Analysis**

The analysis explores key questions that help in understanding the factors influencing salaries and remote work dynamics in the data science field:

1. **Which country has the most remote workers?**
2. **Is remote work a choice/preference or an obligation due to the employee's location?**
3. **Which companies are more likely to offer opportunities to entry-level employees (based on company size)?**
4. **What are the top countries that encourage remote work?**
5. **How do salaries differ across work types (hybrid, remote, onsite)?**
6. **How does experience level impact salary trends?**
7. **Are there significant salary variations across regions and job titles?**

---

## **Data Transformations in Power BI**

### **1. Removed Unnecessary Columns**
Two columns were removed from the dataset as they were irrelevant for our analysis:
- **Salary**: This column was removed because the `Salary in USD` column already provides the salary values in a consistent currency (USD).
- **Salary Currency**: This column was removed as it no longer added value after converting all salaries to USD.

---

### **2. Experience Level Column Transformation**
To make the dataset more readable and consistent, we replaced the abbreviated experience levels with their full descriptions:
- **EN** was replaced with **Entry-level**.
- **MI** was replaced with **Mid-level**.
- **SE** was replaced with **Senior-level**.
- **EX** was replaced with **Executive-level**.

This transformation was done by using the **Replace Values** function in Power Query. Here’s the query for this transformation:

```M
= Table.ReplaceValue(#"Previous Step", "EN", "Entry-level", Replacer.ReplaceText, {"experience_level"})
= Table.ReplaceValue(#"Previous Step", "MI", "Mid-level", Replacer.ReplaceText, {"experience_level"})
= Table.ReplaceValue(#"Previous Step", "SE", "Senior-level", Replacer.ReplaceText, {"experience_level"})
= Table.ReplaceValue(#"Previous Step", "EX", "Executive-level", Replacer.ReplaceText, {"experience_level"})
```

---

### **3. Employment Type Column Transformation**
Similar to the `Experience Level` column, the abbreviations in the **Employment Type** column were replaced with their full descriptions:
- **FL** was replaced with **Freelance**.
- **FT** was replaced with **Full-time**.
- **PT** was replaced with **Part-time**.
- **CT** was replaced with **Contract-based**.

Here’s the query for this transformation:

```M
= Table.ReplaceValue(#"Previous Step", "FL", "Freelance", Replacer.ReplaceText, {"employment_type"})
= Table.ReplaceValue(#"Previous Step", "FT", "Full-time", Replacer.ReplaceText, {"employment_type"})
= Table.ReplaceValue(#"Previous Step", "PT", "Part-time", Replacer.ReplaceText, {"employment_type"})
= Table.ReplaceValue(#"Previous Step", "CT", "Contract-based", Replacer.ReplaceText, {"employment_type"})
```

---

### **4. Job Title Column Transformation**
For the **Job Title** column, we identified and handled duplicate job titles, such as:
- **ML Engineer** and **Machine Learning Engineer** were considered the same job title.

Here’s the query for this transformation:

```M
= Table.ReplaceValue(#"Previous Step", "ML Engineer", "Machine Learning Engineer", Replacer.ReplaceText, {"job_title"})
```

---

### **5. Employee Residence Column Transformation**
For the **Employee Residence** column, we found that **IL** (Israel) was listed as a country while it's not a country, so we removed all rows where the employee was recorded as residing in **IL**.

Here’s the query for this transformation:

```M
= Table.SelectRows(#"Previous Step", each [employee_residence] <> "IL")
```

---

## **Key Indicators**

1. **Average Salary by Experience Level (USD)**  
   This indicator analyzes how salaries vary across different experience levels. It provides insights into how experience impacts salary progression within the data science field.  
   - **Experience Levels**: Entry-level, Mid-level, Senior-level, Executive-level.

2. **Average Salary by Employment Type**  
   This metric compares salaries across different employment types, such as full-time, part-time, contract-based, and freelance. It helps understand how the type of employment influences salary expectations.  
   - **Employment Types**: Full-time, Part-time, Contract-based, Freelance.

3. **Top 5 Countries with the Highest Salaries**  
   This indicator highlights the countries offering the highest salaries for data science professionals. By identifying the top-paying regions, this helps in understanding the global market for data science roles.  
   - **Top Countries**: Includes countries like the United States, Switzerland, and others with high salary offerings.

4. **Ratio of International Employees**  
   This metric compares the employee’s residence with the company’s location to determine the proportion of international employees in the dataset. This helps understand how global the workforce is and the role of remote work in creating international teams.  
   - **International Employees**: Employees residing in a different country than their company.

5. **Employee Distribution by Company Size (S, M, L)**  
   This indicator examines how employees are distributed across companies of varying sizes. It shows which company sizes (small, medium, large) employ the most data science professionals and helps in understanding workforce distribution.  
   - **Company Sizes**: Small (less than 50 employees), Medium (50 to 250 employees), Large (more than 250 employees).

---

## **Using DAX to Create Measures and Calculated Columns**

### **Measures Created with DAX**

1. **Average Salary (Measure)**
   - **Description**: This measure calculates the average salary of

 employees across the entire dataset, taking into account various filters (e.g., by experience level or employment type).
   - **DAX Formula**:
   ```DAX
   Average Salary = AVERAGE([salary_in_usd])
   ```

2. **Employee Count (Measure)**
   - **Description**: This measure counts the total number of employees in the dataset. It can be filtered by different dimensions such as country, experience level, or employment type.
   - **DAX Formula**:
   ```DAX
   Employee Count = COUNTROWS(DataScience_salaries_2024)
   ```

3. **International Ratio (Measure)**
   - **Description**: This measure calculates the percentage of international employees in the dataset, meaning employees whose residence country is different from the company's location.
   - **DAX Formula**:
   ```DAX
   International Ratio = 
   DIVIDE(
       CALCULATE(COUNTROWS(DataScience_salaries_2024), [Is International] = "International"),
       COUNTROWS(Data),
       0
   )
   ```

---

#### **Calculated Columns Created with DAX**

1. **Is International (Calculated Column)**
   - **Description**: This column indicates whether an employee is international, i.e., if the employee's residence country differs from the company's country.
   - **DAX Formula**:
   ```DAX
   Is International = 
   IF(
       [employee_residence] = [company_location], 
       "National", 
       "International"
   )
   ```

---

