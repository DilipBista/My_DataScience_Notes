## TODAY I LEARNED 
10-05-24

Today I learned about functions. 
- default argument
Multiple arguments 
Lamdas 



10- 04- 21 
This morning we reviewed Fridays assignment. Instructor showed us ways to tackle and solve problems even though most of the contents were covered in lecture latter today. 
 Today we learned about control structure. 
	control structure is a method to control the flow of execution using conditionals and loops 




10-01-21
- Today we learned data types, operators and variables. 



09-29-30

 “If you can't solve a problem, then there is an easier problem you can solve: find it.” - Polya

Today I leaned about CASE in SQL. We reviewed temporary table exercises which I copied and pasted below. 
Ryan explained how to break complex problem to simpler form. 
	-- Step 1: blow off whatever I can afford to blow off, first. (temp table)
	-- Step 2: Plan out in text or comments what tables you need to talk to
	-- What db? employees
	-- What tables? employees table for names, departments table dept_name, dept_emp to join
	-- step 3: start small w/ one table, run your query to prove 
	-- step 3.5: figure out the columns later 
	-- "reduce cognitive overhead"

TEMPORARY TABLE review 

use employees;

create temporary table ryan.employees_with_departments AS (
	select first_name, last_name, dept_name
	from employees
	join dept_emp using(emp_no)
	join departments using(dept_no)
	where to_date > curdate()
);

use ryan;
select *
from employees_with_departments;

-- describe employees.employees;

-- add the full name column to our temp_table
alter table employees_with_departments add full_name VARCHAR(31);

select * from employees_with_departments;

-- to change values in a column, use update:
update employees_with_departments
set full_name = concat(first_name, " ", last_name);

select * from employees_with_departments;



-- alter table employees_with_departments drop column first_name;
-- alter table employees_with_departments drop column last_name;
alter table employees_with_departments drop column first_name, drop column last_name;

use ryan;
-- drop table employees_with_departments; 

-- show tables;
use employees;
create temporary table ryan.employees_with_departments AS (
	select concat(first_name, ' ', last_name) as full_name, dept_name
	from employees
	join dept_emp using(emp_no)
	join departments using(dept_no)
	where to_date > curdate()
);

select * from ryan.employees_with_departments;
describe ryan.employees_with_departments;


-- Create a temporary table based on the payment table from the sakila database.
-- Write the SQL necessary to transform the amount column 
-- such that it is stored as an integer representing the number of cents of the payment. 
-- For example, 1.99 should become 199.

use sakila;

create temporary table ryan.payments as (
	select * from payment
);

-- double check it worked
select * from ryan.payments;

-- check the data types w/ describe
describe ryan.payments;

-- time to add a new column
alter table ryan.payments add column cents INT UNSIGNED;

-- set the values for that new column
update ryan.payments
set cents = ryan.payments.amount * 100;

-- double check your work ran
select * from ryan.payments;


-- Find out how the current average pay in each department 
-- compares to the overall, historical average pay. 
-- In order to make the comparison easier, you should use the Z-score for salaries. 
-- In terms of salary, what is the best department right now to work for? 
-- The worst?

-- how can we simplify?
-- blow off what we can, where we can (reducing cognitive overhead)
-- zscore = (x - avg(x)) / stddev(x)

-- step 1: find historic average pay
-- average historic salary is 63811
use employees;
select round(avg(salary)) from salaries;

-- step 2: find historic standard deviation of salaries
-- historic standard deviation is 16905
select round(stddev(salary)) from salaries;

-- step 3: Find current average pay in each department
-- if you see "each" or "for each" in a problem statement for SQL
-- You're probably going to be grouping by the noun specified by "for each"
-- I need departments for dept_name
-- I need salaries to get $$
-- I need something that associates salary w/ department (emp_no to dept_no) (dept_emp)
select dept_name, round(avg(salary)) as current_dept_avg_salary
from departments
join dept_emp using(dept_no)
join salaries using(emp_no)
where salaries.to_date > curdate()
and dept_emp.to_date > curdate()
group by dept_name;

create temporary table ryan.salaries_by_dept as (
	select dept_name, round(avg(salary)) as current_dept_avg_salary
	from departments
	join dept_emp using(dept_no)
	join salaries using(emp_no)
	where salaries.to_date > curdate()
	and dept_emp.to_date > curdate()
	group by dept_name
);

select * from ryan.salaries_by_dept;

use ryan;

-- add our historic avg(salary) column
-- add our historic std(salary) column
-- add our zscore column

alter table salaries_by_dept add column company_avg_salary FLOAT(10, 2);
alter table salaries_by_dept add column company_std_salary FLOAT(10, 2);
alter table salaries_by_dept add column zscore FLOAT(10, 2);

select * from ryan.salaries_by_dept;

-- let's rely on our previously determined avg(salary) and std(salary)
update salaries_by_dept 
set company_avg_salary = (select avg(salary) from employees.salaries);

select * from ryan.salaries_by_dept;

update salaries_by_dept
set company_std_salary = (select stddev(salary) from employees.salaries);

-- double check our work
select * from ryan.salaries_by_dept;

-- now we can calculate the zscore!
update salaries_by_dept
set zscore = (current_dept_avg_salary - company_avg_salary) / company_std_salary;

 -- double check our work
select * from ryan.salaries_by_dept
order by zscore desc;






09-28-21

Subqueries: Need to practice more to  understand more. I feel like I am lost or confused but I don't even know what I am confused about.  
Temporary tables: IT is very straight forward and easy topic in SQL  


09-27-21

Today we will start class on subqueries. 
Until last week I have learned basic statements in SQL such as SELECT, FROM DISTINCT and using clauses such as where, order by, group by, limit 
Functions such as date and time (CURDATE(), NOW(), CURTIME(), Numerical functions such as (AVG, MIN, MAX.. etc) 


09-23-21
- Today we learned about JOIN. There are different types of join such as Inner join, right join, left join, full join etc. 
Join helps us to join 2 or more table where they have common items in one or many different tables. 

SYNTAX for joining is as below; 

Lets say we have 3 different table as table name as follows 
 Employee, Manager, and Departments 
Employee tables has following rows such as .. emp_no, name, DOB, date_hire etc 
Manager table has following rows such as ... emp_no, dep_no 
Departments has following table such as .... dep_no, dep_name 


FIND what is common then JOIN 


09-20-21: Today we learned how to appropriately use Aggregate functions like GROUP BY and adding conditions to GROUP BY with HAVING . 

09-20-20: I we learned few things in SQL, they are Where command, Order By and Limit 



09-21-20: 

I learned about functions such as CONCAT, LIKE / NOT LIKE/ SUBSTR ( these are important top create username and further break down months, and so on).   

Try to think of your results as batches, sets, or pages. The first five results are your first page. The five after that would be your second page, etc. Update the query to find the tenth page of results.

LIMIT and OFFSET can be used to create multiple pages of data. What is the relationship between OFFSET (number of results to skip), LIMIT (number of results per page), and the page number?

SELECT  first_name, last_name, emp_no
FROM employees
WHERE hire_date LIKE '199%' 
AND birth_date LIKE '%-12-25'
ORDER BY hire_date
LIMIT 5 OFFSET 45;

###    GIT Clone 

1. Make a new repository on github called "now_i_know" or "my_data_science_notes"
- copy the SSH clone address
2. `cd ~/codeup-data-science` 
3. Clone your new repo with git git 
`git clone ` then paste that git clone address
4. `cd now_i_know` and do:
	- `git status` to double check that this is a repo
	- `git remote -v` to show the remotes
5. Move your `today_i_learned.txt` document that you made earlier this morning (or right now..)
6. `git add` the new file
7. `git commit` this new work
8. `git push` to push this work
- Oh btw: if you ever get a message about "there's no remote setup" and then the message gives you the code to push and set up the remote, copy + paste that code snippet