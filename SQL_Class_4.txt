# Window Functions
create table shop_sales_data
(
sales_date date,
shop_id varchar(5),
sales_amount int
);

insert into shop_sales_data values('2022-02-14','S1',200);
insert into shop_sales_data values('2022-02-15','S1',300);
insert into shop_sales_data values('2022-02-14','S2',600);
insert into shop_sales_data values('2022-02-15','S3',500);
insert into shop_sales_data values('2022-02-18','S1',400);
insert into shop_sales_data values('2022-02-17','S2',250);
insert into shop_sales_data values('2022-02-20','S3',300);

# Total count of sales for each shop using window function
# Working functions - SUM(), MIN(), MAX(), COUNT(), AVG()

# If we only use Order by In Over Clause
select *,
       sum(sales_amount) over(order by sales_amount desc) as running_sum_of_sales
from shop_sales_data;

# If we only use Partition By
select *,
       sum(sales_amount) over(partition by shop_id) as total_sum_of_sales
from shop_sales_data;

# If we only use Partition By & Order By together
select *,
       sum(sales_amount) over(partition by shop_id order by sales_amount desc) as running_sum_of_sales
from shop_sales_data;

select *,
       sum(sales_amount) over(partition by shop_id order by sales_date desc) 
       as running_sum_of_sales,
       avg(sales_amount) over(partition by shop_id order by sales_date desc) 
       as running_avg_of_sales,
       max(sales_amount) over(partition by shop_id order by sales_date desc) 
       as running_max_of_sales,
       min(sales_amount) over(partition by shop_id order by sales_date desc) 
       as running_min_of_sales
from shop_sales_data;

create table amazon_sales_data
(
    sales_date date,
    sales_amount int
);

insert into amazon_sales_data values('2022-08-21',500);
insert into amazon_sales_data values('2022-08-22',600);
insert into amazon_sales_data values('2022-08-19',300);

insert into amazon_sales_data values('2022-08-18',200);

insert into amazon_sales_data values('2022-08-25',800);


# Query - Calculate the date wise rolling average of amazon sales
select * from amazon_sales_data;

select *,
       avg(sales_amount) over(order by sales_date) as rolling_avg
from amazon_sales_data;

select *,
       avg(sales_amount) over(order by sales_date) as rolling_avg,
       sum(sales_amount) over(order by sales_date) as rolling_sum
from amazon_sales_data;

# Rank(), Row_Number(), Dense_Rank() window functions

insert into shop_sales_data values('2022-02-19','S1',400);
insert into shop_sales_data values('2022-02-20','S1',400);
insert into shop_sales_data values('2022-02-22','S1',300);
insert into shop_sales_data values('2022-02-25','S1',200);
insert into shop_sales_data values('2022-02-15','S2',600);
insert into shop_sales_data values('2022-02-16','S2',600);
insert into shop_sales_data values('2022-02-16','S3',500);
insert into shop_sales_data values('2022-02-18','S3',500);
insert into shop_sales_data values('2022-02-19','S3',300);

select *,
       row_number() over(partition by shop_id order by sales_amount desc) as row_num,
       rank() over(partition by shop_id order by sales_amount desc) as rank_val,
       dense_rank() over(partition by shop_id order by sales_amount desc) as dense_rank_val
from shop_sales_data;


create table employees
(
    emp_id int,
    salary int,
    dept_name VARCHAR(30)

);

insert into employees values(1,10000,'Software');
insert into employees values(2,11000,'Software');
insert into employees values(3,11000,'Software');
insert into employees values(4,11000,'Software');
insert into employees values(5,15000,'Finance');
insert into employees values(6,15000,'Finance');
insert into employees values(7,15000,'IT');
insert into employees values(8,12000,'HR');
insert into employees values(9,12000,'HR');
insert into employees values(10,11000,'HR');


# Query - get one employee from each department who is getting maximum salary (employee can be random if salary is same)

select 
    tmp.*
from (select *,
        row_number() over(partition by dept_name order by salary desc) as row_num
    from employees) tmp
where tmp.row_num = 1;

# Query - get one employee from each department who is getting maximum salary (employee can be random if salary is same)

select 
    tmp.*
from (select *,
        row_number() over(partition by dept_name order by salary desc) as row_num
    from employees) tmp
where tmp.row_num = 1;

# Query - get all employees from each department who are getting maximum salary
select 
    tmp.*
from (select *,
        rank() over(partition by dept_name order by salary desc) as rank_num
    from employees) tmp
where tmp.rank_num = 1;
  
# Query - get all top 2 ranked employees from each department who are getting maximum salary
select 
    tmp.*
from (select *,
        dense_rank() over(partition by dept_name order by salary desc) as dense_rank_num
    from employees) tmp
where tmp.dense_rank_num <= 2;

# Example for lag and lead
create table daily_sales
(
sales_date date,
sales_amount int
);


insert into daily_sales values('2022-03-11',400);
insert into daily_sales values('2022-03-12',500);
insert into daily_sales values('2022-03-13',300);
insert into daily_sales values('2022-03-14',600);
insert into daily_sales values('2022-03-15',500);
insert into daily_sales values('2022-03-16',200);

select * from daily_sales;

select *,
      lag(sales_amount, 1) over(order by sales_date) as pre_day_sales
from daily_sales;

# we can use this to replace null with defualt value like 0
select *,
	coalesce(lag(sales_amount,1) over(order by sales_date), 0) as prev_sales
from daily_sales;

# Query - Calculate the differnce of sales with previous day sales
# Here null will be derived
select sales_date,
       sales_amount as curr_day_sales,
       lag(sales_amount, 1) over(order by sales_date) as prev_day_sales,
       sales_amount - lag(sales_amount, 1) over(order by sales_date) as sales_diff
from daily_sales;

# Here we can replace null with 0
select sales_date,
       sales_amount as curr_day_sales,
       lag(sales_amount, 1, 0) over(order by sales_date) as prev_day_sales,
       sales_amount - lag(sales_amount, 1, 0) over(order by sales_date) as sales_diff
from daily_sales;

# Diff between lead and lag
select *,
      lag(sales_amount, 1) over(order by sales_date) as pre_day_sales
from daily_sales;

select *,
      lead(sales_amount, 1) over(order by sales_date) as next_day_sales
from daily_sales;

create table employees(
  emp_id int,
  emp_name varchar(50),
  mobile BIGINT,
  dept_name varchar(50),
  salary int 
);

insert into employees values(1,'Shashank',778768768,'Software',1000);
insert into employees values(2,'Rahul',876778877,'IT',2000);
insert into employees values(3,'Amit',098798998,'HR',5000);

insert into employees values(4,'Nikhil',67766767,'IT',3000);

select * from employees;

--- Create views in SQL
create view employee_data_for_finance as select emp_id, emp_name,salary from employees;

select * from employee_data_for_finance;

--- Create logic for department wise salary sum
create view department_wise_salary as select dept_name, sum(salary) from employees group by dept_name;

drop view department_wise_salary;

create view department_wise_salary as select dept_name, sum(salary) as total_salary from employees group by dept_name;
