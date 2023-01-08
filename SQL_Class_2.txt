Create database class2_db;

use class2_db;

create table if not exists employee(
    id int,
    name VARCHAR(50),
    address VARCHAR(50),
    city VARCHAR(50)
);

insert into employee values(1, 'Shashank', 'RJPM', 'Lucknow');

select * from employee;

--- add new column named DOB in the TABLE
alter table employee add DOB date;

select * from employee;


--- modify existing column in a TABLE or change datatype of name column or increase lenght of name column
alter table employee modify column name varchar(100);

--- delete existing column from given TABLE or remove city column from employee table
alter table employee drop column city;

select * from employee;


--- rename the column name to full_name
alter table employee rename column name to full_name;


create table if not exists employee(
    id int,
    name VARCHAR(50),
    age int,
    hiring_date date,
    salary int,
    city varchar(50)
);

insert into employee values(1,'Shashank', 24, '2021-08-10', 10000, 'Lucknow');

insert into employee values(2,'Rahul', 25, '2021-08-10', 20000, 'Khajuraho');

insert into employee values(3,'Sunny', 22, '2021-08-11', 11000, 'Banaglore');

insert into employee values(5,'Amit', 25, '2021-08-11', 12000, 'Noida');

insert into employee values(6,'Puneet', 26, '2021-08-12', 50000, 'Gurgaon');

--- add unique integrity constraint on id COLUMN

alter table employee add constraint id_unique UNIQUE(id);

insert into employee values(1,'XYZ', 25, '2021-08-10', 50000, 'Gurgaon');

--- drop constraint from existing TABLE
alter table employee drop constraint id_unique;

insert into employee values(1,'XYZ', 25, '2021-08-10', 50000, 'Gurgaon');

--- create table with Primary_Key

Create table persons
(
    id int, 
    name varchar(50), 
    age int,
    ---Primary Key (id) 
    constraint pk Primary Key (id) 
);

insert into persons values(1,'Shashank',29);

--- Try to insert duplicate value for primary key COLUMN
insert into persons values(1,'Rahul',28);

--- Try to insert null value for primary key COLUMN
insert into persons values(null,'Rahul',28);

--- To check difference between Primary Key and Unique
alter table persons add constraint age_unq UNIQUE(age); 

select * from persons;


insert into persons values(2,'Rahul',28);


insert into persons values(3,'Amit',28);

insert into persons values(3,'Amit',null);

select * from persons;

insert into persons values(4,'Charan',null);

insert into persons values(5,'Deepak',null);


--- create tables for Foreign Key demo
create table customer
(
    cust_id int,
    name VARCHAR(50), 
    age int,
    constraint pk Primary Key (cust_id) 
);

create table orders
(
    order_id int,
    order_num int,
    customer_id int,
    constraint pk Primary Key (order_id),
    constraint fk Foreign Key (customer_id) REFERENCES customer(cust_id)
);

--- Differen between Drop & Truncate Command

select * from persons;
truncate table persons;

select * from persons;

drop table persons;

--- Operations with Select Command

select * from employee;


drop table employee;


create table if not exists employee(
    id int,
    name VARCHAR(50),
    age int,
    hiring_date date,
    salary int,
    city varchar(50)
);

insert into employee values(1,'Shashank', 24, '2021-08-10', 10000, 'Lucknow');

insert into employee values(2,'Rahul', 25, '2021-08-10', 20000, 'Khajuraho');

insert into employee values(3,'Sunny', 22, '2021-08-11', 11000, 'Bangalore');

insert into employee values(5,'Amit', 25, '2021-08-11', 12000, 'Noida');

insert into employee values(1,'Puneet', 26, '2021-08-12', 50000, 'Gurgaon');


select * from employee;

--- how to count total records
select count(*) from employee;


--- alias declaration
select count(*) as total_row_count from employee;


--- display all columns in the final result
select * from employee;


--- display specific columns in the final result
select name, salary from employee;


--- aliases for mutiple columns
select name as employee_name, salary as employee_salary from employee;


select * from employee;

--- print unique hiring_dates from the employee table when employees joined it
select Distinct(hiring_date) as distinct_hiring_dates from employee;


select * from employee;

--- How many unique age values in the table??

select  count(distinct(age)) as total_unique_ages from employee;

--- Increment salary of each employee by 20% and display final result with new salary
SELECT  id,
        name,
        salary as old_salary, 
        (salary + salary * 0.2) as new_salary
FROM employee;


-- Syntax for update command
select * from employee;

--- Upadtes will be made for all rows
UPDATE employee SET age = 20;

select * from employee;

--- update the salary of employee after giving 20% increment
UPDATE employee SET salary = salary + salary * 0.2;

select * from employee;


--- How to filter data using WHERE Clauses
select * from employee where hiring_date = '2021-08-10';


select * from employee;

--- Update the salary of employees who joined the company on 2021-08-10 to 80000
update employee SET salary = 80000 where hiring_date = '2021-08-10';

select * from employee;


--- how to delete specific records from table using delete command
--- delete records of those employess who joined company on 2021-08-10

delete from employee where hiring_date = '2021-08-10';


select * from employee;

--- How to apply auto increment
create table auto_inc_exmp
(
  id int auto_increment,
  name varchar(20),
  primary key (id)
);

insert into auto_inc_exmp(name) values('Shashank');
insert into auto_inc_exmp(name) values('Rahul');

select * from auto_inc_exmp;

--- Use of limit 
select * from employee;
select * from employee limit 2;


# sorting data in mysql by using 'Order By'
select * from employee;

# arrage data in ascending order
select * from employee order by name;


# arrage data in descending order
select * from employee order by name desc;

# display employee data in desc order of salary and if salaries are same for more than one employees
# arrange their data in ascedinding order of name

select * from employee order by salary desc, name asc;

# when we ignore multilevel ordering
select * from employee order by salary desc;

# Write a query to find the employee who is getting maximum salary?
select * from employee order by salary desc limit 1;


# Write a query to find the employee who is getting minium salary?
select * from employee order by salary limit 1;

