# sql_challenge

# Data Analysis with SQL: Employee Database

This document outlines the SQL queries used to analyze and extract meaningful information from an employee database. Each query addresses specific analytical goals, showcasing key database concepts such as joins, filtering, grouping, and subqueries.

---

## 1. **Employee Details with Salary**
   - **Query Objective:** Retrieve the employee number, last name, first name, sex, and salary for all employees.  
   - **Key Concept:** Use of `JOIN` to combine employee and salary data.  
   - **Query:**
     ```sql
     SELECT 
         e.emp_no AS "Employee Number",
         e.last_name AS "Last Name",
         e.first_name AS "First Name",
         e.sex AS "Sex",
         s.salary AS "Salary"
     FROM 
         employees e
     JOIN 
         salaries s ON e.emp_no = s.emp_no;
     ```

---

## 2. **Employees Hired in 1986**
   - **Query Objective:** List the first name, last name, and hire date for employees hired in 1986.  
   - **Key Concept:** Use of `EXTRACT` function to filter by year.  
   - **Query:**
     ```sql
     SELECT first_name, last_name, hire_date 
     FROM employees
     WHERE EXTRACT(YEAR FROM hire_date) = 1986;
     ```

---

## 3. **Department Managers**
   - **Query Objective:** Retrieve the manager details for each department, including the department number and name.  
   - **Key Concept:** Multi-table join across `departments`, `dept_manager`, and `employees`.  
   - **Query:**
     ```sql
     SELECT 
         d.dept_no AS "Department Number",
         d.dept_name AS "Department Name",
         e.emp_no AS "Employee Number",
         e.last_name AS "Last Name",
         e.first_name AS "First Name"
     FROM 
         departments d
     JOIN 
         dept_manager dm ON d.dept_no = dm.dept_no
     JOIN 
         employees e ON dm.emp_no = e.emp_no;
     ```

---

## 4. **Employee Department Details**
   - **Query Objective:** List department details for each employee.  
   - **Key Concept:** `JOIN` to connect `employees`, `dept_emp`, and `departments` tables.  
   - **Query:**
     ```sql
     SELECT 
         de.dept_no AS "Department Number",
         e.emp_no AS "Employee Number",
         e.last_name AS "Last Name",
         e.first_name AS "First Name",
         d.dept_name AS "Department Name"
     FROM 
         employees e
     JOIN 
         dept_emp de ON e.emp_no = de.emp_no
     JOIN 
         departments d ON de.dept_no = d.dept_no;
     ```

---

## 5. **Employees Named Hercules with Last Name Starting with B**
   - **Query Objective:** Retrieve specific employee details based on name patterns.  
   - **Key Concept:** Use of `WHERE` clause with `LIKE` for pattern matching.  
   - **Query:**
     ```sql
     SELECT 
         first_name AS "First Name",
         last_name AS "Last Name",
         sex AS "Sex"
     FROM 
         employees
     WHERE 
         first_name = 'Hercules'
     AND 
         last_name LIKE 'B%';
     ```

---

## 6. **Employees in Sales Department**
   - **Query Objective:** List all employees in the Sales department.  
   - **Key Concept:** Two query methods: direct `JOIN` or subquery.  
   - **Query (Direct `JOIN`):**
     ```sql
     SELECT 
         e.emp_no AS "Employee Number",
         e.last_name AS "Last Name",
         e.first_name AS "First Name"
     FROM 
         employees e
     JOIN 
         dept_emp de ON e.emp_no = de.emp_no
     JOIN 
         departments d ON de.dept_no = d.dept_no
     WHERE 
         d.dept_name = 'Sales'
     ORDER BY 
         e.emp_no DESC;
     ```
   - **Query (Subquery):**
     ```sql
     SELECT 
         e.emp_no AS "Employee Number",
         e.last_name AS "Last Name",
         e.first_name AS "First Name"
     FROM 
         employees e
     JOIN 
         dept_emp de ON e.emp_no = de.emp_no
     WHERE 
         de.dept_no IN (
             SELECT dept_no
             FROM departments
             WHERE dept_name = 'Sales'
         )
     ORDER BY 
         e.emp_no DESC;
     ```

---

## 7. **Employees in Sales and Development Departments**
   - **Query Objective:** Retrieve employees from the Sales and Development departments.  
   - **Key Concept:** Use of `IN` operator with a subquery or direct filtering.  
   - **Query (Subquery):**
     ```sql
     SELECT 
         e.emp_no AS "Employee Number",
         e.last_name AS "Last Name",
         e.first_name AS "First Name",
         d.dept_name AS "Department Name"
     FROM 
         employees e
     JOIN 
         dept_emp de ON e.emp_no = de.emp_no
     JOIN
         departments d ON de.dept_no = d.dept_no
     WHERE 
         d.dept_no IN (
             SELECT dept_no 
             FROM departments
             WHERE dept_name IN ('Sales', 'Development')
         );
     ```
   - **Query (Direct Filtering):**
     ```sql
     SELECT 
         e.emp_no AS "Employee Number",
         e.last_name AS "Last Name",
         e.first_name AS "First Name",
         d.dept_name AS "Department Name"
     FROM 
         employees e
     JOIN 
         dept_emp de ON e.emp_no = de.emp_no
     JOIN
         departments d ON de.dept_no = d.dept_no
     WHERE 
         d.dept_name IN ('Sales', 'Development');
     ```

---

## 8. **Frequency Count of Last Names**
   - **Query Objective:** Calculate the frequency of each last name in descending order.  
   - **Key Concept:** Use of `GROUP BY` and `ORDER BY`.  
   - **Query:**
     ```sql
     SELECT 
         e.last_name AS "Last Name",
         COUNT(e.emp_no) AS "Frequency"
     FROM 
         employees e
     GROUP BY 
         e.last_name
     ORDER BY 
         "Frequency" DESC, "Last Name";
     ```

---

### Notes
- Ensure data integrity when working with external files. For example, missing rows in the PyBank Challenge initially caused discrepancies in calculations. Always verify data accuracy before analysis.  
- These queries demonstrate foundational SQL skills such as joining tables, filtering records, using aggregate functions, and working with subqueries to solve real-world data analysis problems.

Reference 

Matthew Werth - Supported me in perfecting the table joins
