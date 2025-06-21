# Mayank_30QUES_SQL_FIRST
create database rohan;
use rohan;
-- 1. Create employees table
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    salary DECIMAL(10,2),
    department_id INT,
    join_date DATE,
    FOREIGN KEY (department_id) REFERENCES departments(id)
);

-- 2. Create departments table
CREATE TABLE departments (
    id INT PRIMARY KEY,
    dept_name VARCHAR(50) UNIQUE NOT NULL
);

-- 3. Create projects table
CREATE TABLE projects (
    id INT PRIMARY KEY,
    project_name VARCHAR(100),
    employee_id INT,
    FOREIGN KEY (employee_id) REFERENCES employees(id)
);

-- 4. Create attendance table
CREATE TABLE attendance (
    id INT PRIMARY KEY,
    employee_id INT,
    date DATE,
    status VARCHAR(10),
    FOREIGN KEY (employee_id) REFERENCES employees(id)
);

-- Insert into departments
INSERT INTO departments (id, dept_name) VALUES
(1, 'HR'),
(2, 'IT'),
(3, 'Finance'),
(4, 'Sales');

-- Insert into employees
INSERT INTO employees (id, name, age, salary, department_id, join_date) VALUES
(101, 'John', 35, 55000.00, 2, '2023-01-15'),
(102, 'Alice', 28, 60000.00, 1, '2022-03-10'),
(103, 'Bob', 45, 75000.00, 3, '2021-08-20'),
(104, 'Diana', 32, 52000.00, 2, '2023-09-25'),
(105, 'Ethan', 25, 48000.00, 4, '2024-02-10'),
(106, 'Fiona', 38, 70000.00, 3, '2022-11-05');

-- Insert into projects
INSERT INTO projects (id, project_name, employee_id) VALUES
(1, 'Payroll System', 103),
(2, 'HRMS', 102),
(3, 'Inventory App', 104),
(4, 'Sales CRM', 105);

-- Insert into attendance
INSERT INTO attendance (id, employee_id, date, status) VALUES
(1, 101, '2024-06-20', 'Present'),
(2, 102, '2024-06-20', 'Absent'),
(3, 103, '2024-06-20', 'Present'),
(4, 104, '2024-06-20', 'Present'),
(5, 105, '2024-06-20', 'Present'),
(6, 106, '2024-06-20', 'Absent');

-- Ques1)Select all columns from the 'employees' tables.-- 
select * from employees;
-- Ques2)select names of employees who older than 30--
select name,age from employees
where age > 30; 
-- Ques3)select top 5 highest paid employees.--
select name,salary from employees
order by salary desc
limit 5;
-- Ques4)get a list of employees with names starting with 'A'.-- 
select id,name from employees
where name like "A%";
-- Ques5)select unique job title from the employees table-- 
alter table employees add column
job_title varchar(50);
update employees
set job_title="HR"
where id=102;

update employees
set job_title="IT"
where id=101;

update employees
set job_title="Finance"
where id=106;

update employees
set job_title="Sales"
where id=105;

select distinct job_title from employees;

-- Ques6)insert a new employee into the employees table-- 
insert into employees
values
(107, 'Piona', 38, 80000.00, 3, '2021-01-23',"Finance");
-- Ques7)update the salary of an employee with id =101.-- 
update employees
set salary=67000.00
where id =101;
-- Ques8)delete employees who have not joined yet.-- 
delete from employees
where join_date is null;
select * from employees;
-- Ques9)insert multiple rows into the 'departments' table.-- 
INSERT INTO departments (id, dept_name) VALUES
(5, 'Marketing'),
(6, 'Research & Development'),
(7, 'Operations'),
(8, 'Legal');
select * from departments;
-- Ques10) delete all records from 'departments' where dept_name='sales'.-- 
-- 1. Delete attendance records of employees in Sales:-- 
delete from attendance
where employee_id in (
        select id from employees where department_id=4);
-- 2. Delete project records of employees in Sales:-- 
delete from projects
where employee_id in (
        select id from employees where department_id=4);
-- 3. Delete employees in Sales department:--
delete from employees
where department_id=4; 
-- 4. Now delete the Sales department:-- 
delete from departments
where dept_name="Sales";
-- Ques11)find total number of employees.-- 
select count(id) as TOTAL_STUDENTS from employees;
-- Ques12)Calculate average salary by department. --
select departments.dept_name,avg(employees.salary) as AVERAGE_SALARY from departments
join employees on departments.id=employees.department_id
group by departments.id;
-- Ques13)Find the minimum and maximum salary--  
select min(salary) as Minimum_Salary,max(salary) as Maximum_Salary from employees;
-- Ques14)Count how many employees are in each department-- 
select count(employees.id) as No_of_employees, departments.dept_name,departments.id from departments
join employees on departments.id=employees.department_id
group by departments.id;
-- Ques15)Get departments having more than 5 employees.-- 
select count(employees.id) as No_of_employees, departments.dept_name,departments.id from departments
join employees on departments.id=employees.department_id
group by departments.id
having No_of_employees>5;
-- Ques16)get employee names along with their departments names-- 
select employees.name,departments.dept_name from departments
join employees on employees.department_id=departments.id;
-- Ques17)List all employees even if they don't belong to any department-- 
select e.name,e.id,e.department_id,d.dept_name from employees as e
join departments as d on e.department_id=d.id;
-- Ques18)list all the departemnts even if no employees are in them.-- 
select d.dept_name,d.id,e.name from departments as d
join employees as e on e.department_id=d.id;
-- Ques19)Find employees who work in the same department as 'john'-- 
select e2.name,d.dept_name from employees e1
join employees e2 on e1.department_id=e2.department_id
join departments as d on e1.department_id=d.id
where e1.name="John" AND e2.name <> 'John';
-- Ques20)Combine data from 'employees' and 'projects' using INNER JOIN--
select p.project_name, p.id as project_id , e.name from projects p
inner join employees as e on p.employee_id=e.id;

-- Ques21)create a table with not null and unique constraints-- 
create table projets(
     id int primary key auto_increment,
     projects_name varchar(50) unique not null);
select * from projets;
-- Ques22)get employees who earn more than the average salary.-- 
select name,salary from employees
where salary>(select avg(salary) from employees);
-- Ques23)find departments where no employee is assigned.-- 
select d.id,d.dept_name from departments as d
left join employees e on d.id=e.department_id;
-- Quest24)list employees whose salary matches with someone from another dept-- 
select distinct e2.name,e2.salary,e2.department_id from employees e2
join employees e1 on e1.department_id<>e2.department_id and e1.salary=e2.salary; 
-- Ques25)select all employee names using union from two different offices-- 
select e.name,e.department_id,d.dept_name from employees e
join departments as d on d.id=e.department_id
where d.dept_name="HR"
union
select e.name,e.department_id,d.dept_name from employees e
join departments as d on d.id=e.department_id
where d.dept_name="IT";
-- Ques26)Use a subquery to fetch top 3 salaries-- 
select  e.salary , e.name from employees as e
join (select distinct salary from employees
                  order by salary
                  limit 3) as top_salaries on e.salary=top_salaries.salary;
-- Ques27)add a check constraint to allow only salaries >10000-- 
alter table employees
add constraint check_kro
check (salary>10000);
INSERT INTO employees (id, name, age, salary, department_id, join_date)
VALUES (999, 'TestUser', 30, 9000, 1, '2024-01-01');
-- Ques28)create a forign key between employees and departments-- 
create table emp(
          id int primary key,
          name varchar(50),
          department_id int,
          foreign key(department_id) references department(id));
	
create table department(
             id int primary key,
             d_name varchar(50));
-- Ques29)alter the table to add a default value to join_date-- 
alter table employees
modify join_date date default CURRENT_DATE;
-- Ques30)drop the projects table if its exist-- 
drop table if exists projects;
