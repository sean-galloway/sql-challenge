# sql-challenge
## Data Modeling
Inspect the CSVs and sketch out an ERD of the tables. Feel free to use a tool like [http://www.quickdatabasediagrams.com](http://www.quickdatabasediagrams.com).

![Database Diagram](Images/Diagram.png)
## Data Engineering
1. Use the information you have to create a table [schema](./schema.sql) for each of the six CSV files, specifying data types, primary keys, foreign keys, and other constraints.
2. Import each CSV file into the corresponding SQL table. Import in this order:
   1. titles
   2. departments
   3. employees
   4. salaries
   5. dept_emp
   6. dep_manager
#### Data Analysis

Once you have a complete database, do the following:

1. List the following details of each employee: employee number, last name, first name, sex, and salary.
```sql
SELECT e.emp_no, e.last_name, e.first_name, e.sex, s.salary
FROM employees AS e
INNER JOIN salaries AS s
    ON s.emp_no = e.emp_no;
```

2. List first name, last name, and hire date for employees who were hired in 1986.
```sql
SELECT first_name, last_name, hire_date
FROM employees
WHERE hire_date >= '1986-01-01' AND hire_date <= '1986-12-31';
```
3. List the manager of each department with the following information: department number, department name, the manager's employee number, last name, first name.
```sql
SELECT dm.dept_no, d.dept_name, e.emp_no, e.first_name, e.last_name
FROM dept_manager AS dm
INNER JOIN departments AS d
	ON d.dept_no = dm.dept_no
INNER JOIN employees AS e
	ON e.emp_no = dm.emp_no;
```
4. List the department of each employee with the following information: employee number, last name, first name, and department name.
```sql
SELECT e.emp_no, e.last_name, e.first_name, d.dept_name
FROM dept_emp AS de
INNER JOIN departments AS d
	ON d.dept_no = de.dept_no
INNER JOIN employees AS e
	ON e.emp_no = de.emp_no;
```
5. List first name, last name, and sex for employees whose first name is "Hercules" and last names begin with "B."
```sql
SELECT first_name, last_name, sex
FROM employees
WHERE first_name = 'Hercules' and LEFT(last_name, 1) = 'B';
```
6. List all employees in the Sales department, including their employee number, last name, first name, and department name.
```sql
SELECT e.emp_no, e.last_name, e.first_name, d.dept_name
FROM employees AS e
INNER JOIN dept_emp AS de
	ON e.emp_no = de.emp_no
INNER JOIN departments AS d
	ON de.dept_no = d.dept_no
WHERE d.dept_name = 'Sales';
```
7. List all employees in the Sales and Development departments, including their employee number, last name, first name, and department name.
```sql
SELECT e.emp_no, e.last_name, e.first_name, d.dept_name
FROM employees AS e
INNER JOIN dept_emp AS de
	ON e.emp_no = de.emp_no
INNER JOIN departments AS d
	ON de.dept_no = d.dept_no
WHERE d.dept_name = 'Sales' OR d.dept_name = 'Development';
```
8. In descending order, list the frequency count of employee last names, i.e., how many employees share each last name.
```sql
SELECT last_name, COUNT(last_name) AS "Name Count"
FROM employees
GROUP BY last_name
ORDER BY "Name Count" DESC;
```
## Bonus (Optional)

As you examine the data, you are overcome with a creeping suspicion that the dataset is fake. You surmise that your boss handed you spurious data in order to test the data engineering skills of a new employee. To confirm your hunch, you decide to take the following steps to generate a visualization of the data, with which you will confront your boss:

1. Import the SQL database into Pandas. (Yes, you could read the CSVs directly in Pandas, but you are, after all, trying to prove your technical mettle.) This step may require some research. Feel free to use the code below to get started. Be sure to make any necessary modifications for your username, password, host, port, and database name:

   ```python
   from sqlalchemy import create_engine
   DATABASE_URI = f"postgres://{username}:{password}@localhost/{database}"
   # Connect to the database
   engine = create_engine(DATABASE_URI)
   connection = engine.connect()
   ```

* Consult [SQLAlchemy documentation](https://docs.sqlalchemy.org/en/latest/core/engines.html#postgresql) for more information.

* If using a password, do not upload your password to your GitHub repository. See [https://www.youtube.com/watch?v=2uaTPmNvH0I](https://www.youtube.com/watch?v=2uaTPmNvH0I) and [https://help.github.com/en/github/using-git/ignoring-files](https://help.github.com/en/github/using-git/ignoring-files) for more information.

2. Create a histogram to visualize the most common salary ranges for employees.
  
<img src="Images/SalaryPerEmployee.png" width=410>

3. Create a bar chart of average salary by title.  
  
<img src="Images/AverageSalaryByJobTitle.png" width=410>