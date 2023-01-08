---Command to create DATABASE
Create DATABASE BigDataBootCamp;
Create DATABASE Test;

---To list down all the databases
Show Databases;

---Command to drop a DATABASE
Drop DATABASE Test;

--- Go inside the particular DATABASE
use BigDataBootCamp;

--- Command to create a TABLE
Create table if not exists employee
(
    id int,
    name VARCHAR(50)
);

---Command to list down all the TABLES
show tables;


---Command to list down all the TABLES
show create table employee;


--- Command to create a TABLE
Create table if not exists employee
(
    id int,
    name VARCHAR(50),
    salary DOUBLE,
    hiring_date DATE
);

--- Syntax 1 To insert data into a TABLE
insert into employee values(1,'Shashank',1000,'2021-09-15');

--- This statement will fail
insert into employee values(2,'Rahul','2021-09-15');

--- Syntax 2 To insert data into a TABLE
insert into employee(salary,name,id) values(2000,'Rahul',2);

--- Insert multiple rows using single insert statement
insert into employee values(3,'Amit',3000,'2021-09-15'),(4,'Niting',3500,'2021-09-16'),(5,'Kajal',4000,'2021-09-20');

--- Use select command to query the data
Select * from employee;

--- Example for Integrity Constraints
Create table if not exists employee_with_constraints
(
    id int NOT NULL,
    name VARCHAR(50) NOT NULL,
    salary DOUBLE,
    hiring_date DATE DEFAULT '2021-01-01',
    UNIQUE (id),
    CHECK (salary > 1000)
);


--- Example 1 for Integrity Constraint failure
--- Exception will be thrown -> Column 'id' cannot be null
insert into employee_with_constraints values(null,'Amit',3000,'2021-09-15');

--- Example 2 for Integrity Constraint failure
--- Exception will be thrown -> Column 'name' cannot be null
insert into employee_with_constraints values(3,null,3000,'2021-09-15');


--- Example 3 for Integrity Constraint failure
--- Exception will be thrown -> Check constraint 'employee_with_constraints_chk_1' is violated.
insert into employee_with_constraints values(1,'Shashank',500,'2021-09-15');

--- Insert corect data
insert into employee_with_constraints values(1,'Shashank',1200,'2021-09-15');

--- Example 4 for Integrity Constraint failure
--- Exception will be thrown -> Duplicate entry '1' for key 'employee_with_constraints.id'
insert into employee_with_constraints values(1,'Amit',1300,'2021-09-28');

--- Example 5 for Integrity Constraint
insert into employee_with_constraints values(2,'Amit',1300,null);
insert into employee_with_constraints(id,name,salary) values(3,'Mukesh',2400);

select * from employee_with_constraints;

--- Add alias name for constraints
Create table if not exists employee_with_constraints_tmp
(
    id int NOT NULL,
    name VARCHAR(50) NOT NULL,
    salary DOUBLE,
    hiring_date DATE DEFAULT '2021-01-01',
    CONSTRAINT unique_id UNIQUE (id),
    CONSTRAINT salary_check CHECK (salary > 1000)
);

--- Example for Integrity Constraint failure with alias name of constraint
--- Exception will be thrown -> Check constraint 'salary_check' is violated.
insert into employee_with_constraints_tmp values(1,'Shashank',500,'2021-09-15');
