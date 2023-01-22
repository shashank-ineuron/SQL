# How to use Frame Clause - Rows BETWEEN
select * from daily_sales;

select *,
      sum(sales_amount) over(order by sales_date rows between 1 preceding and 1 following) as prev_plus_next_sales_sum
from daily_sales;

select *,
      sum(sales_amount) over(order by sales_date rows between 1 preceding and current row) as prev_plus_next_sales_sum
from daily_sales;

select *,
      sum(sales_amount) over(order by sales_date rows between current row and 1 following) as prev_plus_next_sales_sum
from daily_sales;

select *,
      sum(sales_amount) over(order by sales_date rows between 2 preceding and 1 following) as prev_plus_next_sales_sum
from daily_sales;

select *,
      sum(sales_amount) over(order by sales_date rows between unbounded preceding and current row) as prev_plus_next_sales_sum
from daily_sales;

select *,
      sum(sales_amount) over(order by sales_date rows between current row and unbounded following) as prev_plus_next_sales_sum
from daily_sales;

select *,
      sum(sales_amount) over(order by sales_date rows between unbounded preceding and unbounded following) as prev_plus_next_sales_sum
from daily_sales;

# Alternate way to esclude computation of current row
select *,
      sum(sales_amount) over(order by sales_date rows between unbounded preceding and unbounded following) - sales_amount as prev_plus_next_sales_sum
from daily_sales;

# How to work with Range Between

select *,
      sum(sales_amount) over(order by sales_amount range between 100 preceding and 200 following) as prev_plus_next_sales_sum
from daily_sales;

# Calculate the running sum for a week
# Calculate the running sum for a month
insert into daily_sales values('2022-03-20',900);
insert into daily_sales values('2022-03-23',200);
insert into daily_sales values('2022-03-25',300);
insert into daily_sales values('2022-03-29',250);


select * from daily_sales;

select *,
       sum(sales_amount) over(order by sales_date range between interval '6' day preceding and current row) as running_weekly_sum
from daily_sales;


--- Common table expression

create table amazon_employees(
    emp_id int,
    emp_name varchar(20),
    dept_id int,
    salary int

 );

 insert into amazon_employees values(1,'Shashank', 100, 10000);
 insert into amazon_employees values(2,'Rahul', 100, 20000);
 insert into amazon_employees values(3,'Amit', 101, 15000);
 insert into amazon_employees values(4,'Mohit', 101, 17000);
 insert into amazon_employees values(5,'Nikhil', 102, 30000);

 create table department
 (
    dept_id int,
    dept_name varchar(20) 
  );

  insert into department values(100, 'Software');
    insert into department values(101, 'HR');
      insert into department values(102, 'IT');
        insert into department values(103, 'Finance');

--- Write a query to print the name of department along with the total salary paid in each department
--- Normal approach
select d.dept_name, tmp.total_salary
from (select dept_id , sum(salary) as total_salary from amazon_employees group by dept_id) tmp
inner join department d on tmp.dept_id = d.dept_id;

--- how to do it using with clause??
with dept_wise_salary as (select dept_id , sum(salary) as total_salary from amazon_employees group by dept_id)

select d.dept_name, tmp.total_salary
from dept_wise_salary tmp
inner join department d on tmp.dept_id = d.dept_id;

with dept_wise_salary as (select dept_id , sum(salary) as total_salary from amazon_employees group by dept_id), 
dept_wise_max_salary as (select dept_id , max(salary) as max_salary from amazon_employees group by dept_id)

select * from dept_wise_max_salary;

select * from dept_wise_salary;




--- Write a Query to generate numbers from 1 to 10 in SQL

with recursive generate_numbers as   
(
  select 1 as n
  union 
  select n+1 from generate_numbers where n<10
) 

select * from generate_numbers;


create table emp_mgr
(
id int,
name varchar(50),
manager_id int,
designation varchar(50),
primary key (id)
);


insert into emp_mgr values(1,'Shripath',null,'CEO');
insert into emp_mgr values(2,'Satya',5,'SDE');
insert into emp_mgr values(3,'Jia',5,'DA');
insert into emp_mgr values(4,'David',5,'DS');
insert into emp_mgr values(5,'Michael',7,'Manager');
insert into emp_mgr values(6,'Arvind',7,'Architect');
insert into emp_mgr values(7,'Asha',1,'CTO');
insert into emp_mgr values(8,'Maryam',1,'Manager');


select * from emp_mgr;

--- for our CTO 'Asha', present her org chart

with recursive emp_hir as  
(
   select id, name, manager_id, designation from emp_mgr where name='Asha'
   UNION
   select em.id, em.name, em.manager_id, em.designation from emp_hir eh inner join emp_mgr em on eh.id = em.manager_id
)

select * from emp_hir;

--- Print level of employees as well
with recursive emp_hir as  
(
   select id, name, manager_id, designation, 1 as lvl from emp_mgr where name='Asha'
   UNION
   select em.id, em.name, em.manager_id, em.designation, eh.lvl + 1 as lvl from emp_hir eh inner join emp_mgr em on eh.id = em.manager_id
)

select * from emp_hir;
