# All-my-codes-for-mysql
I have worked on some databases to perform some tasks


create schema practice;
use practice;
create table employeeinfo(EmpID int,EmpFname varchar(30),EmpLname varchar(30),Department varchar(20),Project varchar(20),Address varchar(30),DOB date, Gender varchar(1) ) ;
insert into employeeinfo values
(101,'Tim','David','HR','P1','Hyderabad(HYD)','1976/12/01','M'),
(102,'Teresa','Macdonald','Admin','P2','Delhi(DEL)','1996/05/02','F'),
(103,'Sydnee','Lewis','Account','P3','Mumbai(BOM)','1980/01/01','M'),
(104,'Jaelynn','Hahn','HR','P1','Hyderabad(HYD)','1992/05/02','F'),
(105,'Deandre','Simmons','Admin','P2','Delhi(DEL)','1994/07/03','M');
select * from employeeinfo;
create table EmployeePosition(EmpID int,EmpPosition varchar(20),DateOfJoining date,Salary decimal);
insert into EmployeePosition values
(101,'Manager','2022/05/01','500000'),
(102,'Executive','2022/05/02',75000),
(103,'Manager','2022/05/01',90000),
(102,'Lead','2022/05/02',85000),
(101,'Executive','2022/05/01',300000);
select * from Employeeposition;

select upper(EmpFname) as EmpName from employeeinfo;

select count('HR') as number_of_employee from employeeinfo;

select curdate();
select current_date();

select left(EmpLname,4) from employeeinfo;

select substring_index(Address,'(',1) as cityname from employeeinfo;

create table employeeinfocopy as select * from employeeinfo;
select * from employeeinfocopy;

select EmpID from employeePosition where Salary between 50000 and 100000;

select e1.EmpID,e1.EmpFname from employeePosition e2,employeeinfo e1 where e2.EmpID=e1.EmpID  AND Salary between 50000 and 100000;

select EmpFname from employeeinfo where EmpFname like 'S%';

select * from employeeinfo limit 2;

select concat(EmpFname,' ',EmpLname) as FullName from employeeinfo;
drop table orders;
create table orders(order_id int primary key auto_increment, customer_name varchar(25),order_price decimal,item_name varchar(20));

select * from orders;
insert into orders(customer_name,order_price,item_name) values
('Raju',200,'Cloth'),
('Raju',300,'Sports'),
('Anil',400,'Food'),
('Abhishek',500,'Electronics'),
('Anil',240,'Cloth'),
('Sunil',530,'Electronics'),
('Abhishek',300,'Food');

select count(order_id) from orders;

use assignment;
show tables;
select * from customer;
select * from orders;
select * from salesman;

/**
From the following tables write a SQL query to find those orders where the order amount exists between 500 and 2000. Return ord_no, purch_amt, cust_name, city.**/
select order_no, purch_amt,c.cust_name,city from orders join customer c on orders.customer_id=c.customer_id where purch_amt between 500 and 2000;

/*Write a SQL query to find the salesperson(s) and the customer(s) he represents. Return Customer Name, city, Salesman, commission.*/
select c.cust_name,c.city,s.name,s.commission from salesman s join customer c using (salesman_id); 

/*From the following tables write a SQL query to locate those salespeople who do not live in the same city where their customers
 live and have received a commission of more than 12% from the company. Return Customer Name, customer city, Salesman, salesman city, commission.*/

select c.cust_name,c.city,s.name,s.city,s.commission from salesman s join customer c using (salesman_id) where c.city!=s.city and s.commission>0.12; 

/*From the following tables write a SQL query to find the details of an order. Return ord_no, ord_date, purch_amt, Customer Name, grade, Salesman, commission.*/
select o.order_no,o.ord_date,o.purch_amt,c.cust_name,c.grade,s.name,s.commission from orders o inner join customer c on o.customer_id=c.customer_id inner join salesman s on c.salesman_id=s.salesman_id; 

/*From the following tables write a SQL query to display the customer name, customer city, grade, salesman, salesman city. The results should be sorted by ascending customer_id.*/
select c.cust_name,c.city,c.grade,s.name,s.city from customer c join salesman s using (salesman_id) order by c.customer_id asc;


/*From the following tables write a SQL query to find those customers with a grade less than 300. Return cust_name, customer
 city, grade, Salesman, salesmancity. The result should be ordered by ascending customer_id.*/

select c.cust_name,c.city,c.grade,s.name,s.city from customer c left join salesman s using (salesman_id) where c.grade<300 order by c.customer_id asc;


/*Write a SQL statement to make a report with customer name, city, order number, order date, and order amount in ascending order according to the order date
 to determine whether any of the existing customers have placed an order or not.*/
select c.cust_name,c.city,o.order_no,o.ord_date,o.purch_amt from customer c left join orders o on c.customer_id=o.customer_id order by o.ord_date asc;

/***************************************************************************************/

/*SQL statement to generate a report with customer name, city, order number, order date, order amount, salesperson name, and commission to
 determine if any of the existing customers have not placed orders or if they have placed orders through their salesman or by themselves.*/
select c.cust_name, c.city, o.order_no, o.ord_date, o.purch_amt, s.name, s.commission from customer c left join orders o using(customer_id) left join salesman s on o.salesman_id=s.salesman_id;


/*2,Write a SQL statement to generate a list in ascending order of salespersons who work either for one or more customers or have not yet joined any of the customers.*/
select s.name, c.cust_name from salesman s left join customer c using (salesman_id) order by s.name asc;


/*3,From the following tables write a SQL query to list all salespersons along with customer name, city, grade, order number, date, and amount.*/
select s.name, c.cust_name, c.city, c.grade, o.order_no, o.ord_date, o.purch_amt from salesman s left join customer c using (salesman_id) left join orders o using (customer_id);

/*4,Write a SQL statement to make a list for the salesmen who either work for one or more customers or yet to join any of the customer. 
The customer may have placed, either one or more orders on or 
above order amount 2000 and must have a grade, or he may not have placed any order to the associated supplier.*/

select distinct(s.name) from customer c right join salesman s using (salesman_id) left join orders o on c.customer_id=o.customer_id or o.purch_amt>=2000 and c.grade is not null;

/*Write a SQL statement to generate a report with the customer name, city, order no. order date, purchase amount for only those customers on
 the list who must have a grade and placed one or more orders or which order(s) have been placed by the customer 
who neither is on the list nor has a grade.*/

select c.cust_name, c.city, o.order_no, o.purch_amt from customer c inner join orders o using(customer_id) where c.grade is not null or c.customer_id<>o.customer_id and c.grade is null;


/*Write a SQL query to combine each row of the salesman table with each row of the customer table.*/
select * from salesman cross join customer;

/*Write a SQL statement to create a Cartesian product between salesperson and customer, i.e. each salesperson will 
appear for all customers and vice versa for that salesperson who belongs to that city*/

select s.name,c.cust_name from salesman s cross join customer c where s.city=c.city;

/*Write a SQL statement to make a Cartesian product between salesman and customer i.e. each salesman will appear for all customers and vice versa
 for those salesmen who must belong to a
 city which is not the same as his customer and the customers should have their own grade*/
select s.name,c.cust_name from salesman s cross join customer c where s.city!=c.city and c.grade is not null;

/*From the following tables write a SQL query to select all rows from both participating tables as long as there is a match between pro_com and com_id.*/
select * from company_mast c inner join item_mast i on c.com_id=i.pro_com;

/*Write a SQL query to display the item name, price, and company name of all the products.*/
select i.pro_name, i.pro_price, c.com_name from company_mast c inner join item_mast i on c.com_id=i.pro_com;

/*11,From the following tables write a SQL query to calculate the average price of items of 
each company. Return average value and company name.*/

select avg(i.pro_price), c.com_name from company_mast c inner join item_mast i on  c.com_id=i.pro_com group by c.com_name;


/*From the following tables write a SQL query to calculate and find the average price of 
items of each company higher than or equal to Rs. 350. Return average value and company name.*/

select avg(i.pro_price) as avg_price, c.com_name from company_mast c inner join item_mast i on  c.com_id=i.pro_com group by c.com_name having avg_price >= 350;


/*13,From the following tables write a SQL query to find the most expensive product of each company.
 Return pro_name, pro_price and com_name.*/
 
 select i.pro_name, i.pro_price, c.com_name from company_mast c inner join item_mast i on c.com_id = i.pro_com group by c.com_name having max(i.pro_price);
 
 
 /*14,From the following tables write a SQL query to display all the data of employees
 including their department.*/
 select * from emp_details e1 join emp_department e2 on e1.emp_dept=e2.dpt_code;
 
 /*15,From the following tables write a SQL query to display the first and last names of each
 employee, as well as the department name and sanction amount.*/
 
 select e1.emp_fname, e1.emp_lname, e2.dpt_name, e2.dpt_allotment from emp_details e1 inner join emp_department e2 on e1.emp_dept=e2.dpt_code;
 
 /*16,From the following tables write a SQL query to find the departments with budgets more 
 than Rs. 50000 and display the first name and last name of employees.*/
 
 select e1.emp_fname, e1.emp_lname, e2.dpt_name, e2.dpt_allotment from emp_details e1 inner join emp_department e2 on e1.emp_dept=e2.dpt_code where e2.dpt_allotment>50000 ;
 
 /*From the following tables write a SQL query to find the names of departments where more
 than two employees are employed. Return dpt_name.*/
 
 select e2.dpt_name from emp_department e2 inner join emp_details e1 on e1.emp_dept=e2.dpt_code group by e1.emp_dept having count(e1.emp_idno)>2;
