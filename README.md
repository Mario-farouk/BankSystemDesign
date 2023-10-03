# BankSystemDesign
# First we reseved a required document from clint

![Screenshot 2023-10-02 195047](https://github.com/Mario-farouk/BankSystemDesign/assets/136117708/5f74245b-ad53-4da0-9db2-c2d56b453416)
# Secound we start to Drow the ERD 
![Screenshot 2023-10-02 195807](https://github.com/Mario-farouk/BankSystemDesign/assets/136117708/72a84102-7802-4319-948a-29ec696075b1)
# third Mapping 

![Screenshot 2023-10-02 200013](https://github.com/Mario-farouk/BankSystemDesign/assets/136117708/d9793d97-801f-48ca-bf69-e582ac730665)
# then impliment physical schema   

![06_schema](https://github.com/Mario-farouk/BankSystemDesign/assets/136117708/1015bd3b-fd81-4ba4-82dc-8b822a5bcba3)

# Let's go for structure 
<br> create database Union_Bank
<br> go

<br> USE Union_Bank
<br> go

<br> --2)Q2.Create all the tables mentioned in the database diagram.
<br> --create tables & add constraint Section 
<br> --3)Q3. Create all the constraints based on the database diagram.

<br> --Creating table named Customers
<br> create table Customers 
<br> (
<br> CustomerId int primary key identity(1,1),
<br> FirstName varchar(50) not null,
<br> LastName varchar(50) not null,
<br> Gender varchar(1) not null,
<br> Email varchar(50) not null,
<br> BirthDate date not null,
<br> Age as year(getdate())-year (BirthDate),
<br> City varchar(50) not null,
<br> State varchar(50) not null,
<br> --constraint ch1 check(Gender in ('m','f'))
<br> );
<br> go

<br> --Creating table named Customer_phones
<br> create table Customer_phones 
<br> (
<br> CustomerId int,
<br> phone varchar(50) not null,
<br> constraint  Customer_phones_IDS primary key (CustomerId, phone),
<br> constraint Customer_phones_Customers foreign key (CustomerId) 
<br> references Customers (CustomerId)
<br> );
<br> go

<br> --Creating table named Branch
<br> create table Branch 
<br> (
<br> BranchId int primary key,
<br> BranchName varchar(50) not null,
<br> Branch_Location varchar(50) not null,
<br> );
<br> go

<br> --Creating table named Account
<br> create table Account 
<br> (
<br> AccountNumber int primary key, 
<br> AccountType varchar(50) ,
<br> Balance float not null,
<br> OpenDate date default getdate(),
<br> Account_Status varchar(30) not null,
<br> Branchid INT,
<br> CustomerId int,
<br> constraint customer_Account foreign key (CustomerId)
<br> references Customers (CustomerId) ,
<br> constraint Branch_Account foreign key (Branchid)
<br> references Branch (BranchId) ,
<br> --constraint ch2 check(Account_status in ('active','not active')),
<br> );
<br> go
  
<br> ---we can check on account type through this constraint to check if 
<br> --- the account saving or current ---
<br> alter table account add constraint check_status check(accounttype in ('saving','current'))

<br> --Creating table named Transactions
<br> create table Transactions 
<br> (
<br> Transaction_Type_Id int primary key identity (1,1),
<br> TransactionType varchar(30) not null,
<br> );
<br> go

<br> --Creating table named Card */
<br> create table Card (
<br> CardNumber int primary key,
<br> CardType varchar(30) not null,
<br> card_Status varchar (30) not null,
<br> ExpiryDate date not null,
<br> AccountNumber int,
<br> constraint Card_Account  foreign key (AccountNumber)
<br> references Account (AccountNumber) 
<br> );
<br> go

<br> --Creating table named Employee
<br> create table Employee (
<br> EmployeeId int primary key,
<br> FirstName varchar(30) NOT NULL,
<br> LastName varchar(30) NOT null,
<br> Salary DECIMAL NOT NULL,
<br> Position varchar(50) NOT NULL,
<br> SupervisorID int ,
<br> branchid INT,
<br> Dno INT ,
<br> constraint super_Employee foreign key(SupervisorID)
<br> REFERENCES Employee (EmployeeId) ,
<br> CONSTRAINT branchid_Employee foreign key(branchid)
<br> references branch(branchid) ,
<br> );
<br> go

<br> --create table named Department
<br> CREATE TABLE Department 
<br> (
<br> Dnumber INT PRIMARY KEY ,
<br> Dname VARCHAR(50) NOT NULL,
<br> MgriD INT ,
<br> constraint manager_Employee foreign key(MgriD)
<br> REFERENCES Employee (EmployeeId) 
<br> )

<br> ALTER TABLE employee 
<br> ADD constraint Dno_Employee foreign key(Dno)references Department (DNumber) 

<br> --Creating table named Loan
<br> create table Loan (
<br> LoanId int primary key,
<br> Amount decimal not null,
<br> loan_months_terms int not null,
<br> LoanType varchar(30) not null,
<br> StartDate date not null,
<br> EndDate date not null,
<br> InterestRate varchar(20) not null,
<br> customerid int,
<br> BranchId int,
<br> constraint Loan_customer foreign key (customerid)
<br> references Customers (customerid),
<br> constraint Loan_Branch foreign key (BranchId)
<br> references Branch (BranchId)
<br> );
<br> go

<br> --Creating table named ATM
<br> create table ATM (
<br> AtmId int primary key,
<br> Atm_location varchar(50) NOT NULL,
<br> Atm_status varchar(30) NOT NULL ,
<br> BranchId int,
<br> constraint ATM_Branch foreign key (BranchId)
<br> references Branch(BranchId)
<br> );
<br> GO

<br> --create table Account_Atm_transcation
<br> CREATE TABLE Account_Atm_transcation
<br> (
<br> TransactionDate  DATE DEFAULT GETDATE() ,
<br> AccountNumber INT,
<br> transactionid INT identity(1,1) primary key,
<br> Transaction_Type_Id int,
<br> Atmid INT ,
<br> Amount FLOAT NOT NULL, 
<br> CONSTRAINT Atm_account_transcation foreign key (AccountNumber) references Account(AccountNumber),
<br> CONSTRAINT transcation_Account_Atm foreign key (Transaction_Type_Id) references Transactions(Transaction_Type_Id)
<br> );
<br> go

# develop oltp system 
<br>----------------- to get employee details along with department name---------

<br>CREATE OR ALTER VIEW EmployeeDetails 
<br>AS
<br>SELECT e.employeeid, concat (e.FirstName,' ',e.LastName) AS employee_name, e.salary, d.dname AS department_name
<br>FROM Employee e JOIN Department d 
<br>ON e.Dno = d.Dnumber
<br>WHERE Dnumber=10

<br>--test view query 

<br>select * from EmployeeDetails 

<br>------------------name of manager employee-----------------------------

<br>create or alter view managerofemployee 
<br>as
<br>SELECT firstname,lastname,dname
<br>FROM employee e JOIN department d
<br>ON e.dno=D.dnumber
<br>AND e.employeeid=D.mgrid

<br>--test view query 

<br>select * from managerofemployee  

<br>--------view to count each transaction and display the sum of transactions-------

<br>create or alter view most_transactions
<br>as
<br>  select transactionType, count(a.Transaction_Type_Id) as count_transactions 
<br>  from Account_Atm_transcation a, Transactions t
<br>  where a.TRANSACTION_type_ID = t.Transaction_Type_Id
<br>  group by transactionType
<br>  union all
<br>  SELECT 'Sum_of_Transactions' Transaction_type_Id, count(Transaction_type_Id)
<br>  FROM Account_Atm_transcation
 
<br>--test view query 

<br>SELECT * from most_transactions

<br>-----------create a view to get customer and their account details with the availability loans amount---------

<br>create view v_customer_acc_details 
<br>As 
<br>select c.CustomerId, phone , concat(c.FirstName,c.LastName) as "Full Name" 
<br>,a.AccountNumber , balance , amount as "Loan Amount" 
<br>from Customer_phones cp inner join customers c 
<br>on cp.customerid = c.customerid
<br>inner join account a 
<br>on c.customerId = a.customerId, Customers cl 
<br>inner join loan l 
<br>on cl.customerId = l.customerId
<br>group by c.CustomerId, phone,concat(c.FirstName,c.LastName), a.AccountNumber , balance , amount 

<br>--test view query 

<br>select * from v_customer_acc_details

<br>-----------------------------------all customers info-------------------------------------

<br>create OR ALTER view vw_customers_info 
<br>as
<br>select firstname,accounttype,balance,opendate,account_status,age,city
<br>from account a, customers s
<br>where a.CustomerId=s.CustomerId

<br>--test view query

<br>select * from vw_customers_info

<br>---------------------all customers balance more than 25000----------------------------------
<br>create OR ALTER view vw_balance 
<br>as
<br>select balance , firstname
<br>from Account a , Customers c
<br>where a.CustomerId=c.CustomerId
<br>and Balance >25000

<br>--test view query

<br>select * from vw_balance

<br>--------------------------------Funcation-----------------------------

<br>----------------Function to calculate total salary for each department --------

<br>CREATE  OR ALTER FUNCTION cal_salary(@dnum INT)
<br>RETURNS DECIMAL(10,2)
<br>BEGIN
<br>DECLARE @total DECIMAL(10,2)
<br>SELECT  @total=SUM(salary)
<br>FROM  employee 
<br>WHERE Dno=@dnum
<br>RETURN @total
<br>END 

<br>--test function query 

<br>--select * from Employee
<br>select dbo.cal_Salary(10) AS Total_Salary


<br>---------Create Scalar function name GetEmployeeSupervisor------------------------ 
<br>--Returns the name of an employee's supervisor based on their employeeid.---------

<br>create or alter function getemployeesupervisor (@employeeid int) 
<br>returns varchar(50)
<br> begin 
<br>    declare @name varchar(50),@name2 VARCHAR(50)
<br>  select @name=s.FirstName ,@name2=s.LastName
<br>  from employee e join employee s
<br>  on s.employeeid=e.supervisorid
<br>  where e.employeeid=@employeeid
<br>  return @name + ' '+ @name2
<br> end 

<br>--test function query 

<br>--select * from Employee
<br>select dbo.getemployeesupervisor (2) as fullname


<br>--------------create function Calculate Monthly payment ----------------

<br>create or alter function dbo.CalculateMonthlypayment(
<br>@interest_rate float,
<br>@loan_amount float,
<br>@loan_termmonths int,
<br>@monthly_interestrate float)
<br>returns float
<br>as 
<br>begin 
<br>	set @monthly_interestrate = @interest_rate/(12*100) ;
<br>	return ( @loan_amount*@monthly_interestrate) / (1 - power(1+ @monthly_interestrate,-@loan_termmonths)) ;
<br>End;

<br>--test function query 

<br>select dbo.CalculateMonthlypayment(4.56,116500,42,0.0038) as [Monthly Loan Payment] 

<br>-----------------create function calculate Interest Amount -------------------------

<br>create function dbo.calculateInterestAmount(@loan_amount float,@interest_rate float,@loan_termmonths int)
<br>returns float
<br>as
<br>begin
<br>	return @loan_amount*(@interest_rate/(12*100))*@loan_termmonths
<br>end;

<br>--test function query 

<br>select dbo.calculateInterestAmount(1000,10,12) as [Interest Amount]


<br>---------------------------------all customers data------------------------------

<br>create OR ALTER function Getallcustomers(@acc_id int)
<br>returns table 
<br>return 
<br>select accounttype,balance,opendate,account_status,firstname,age,city
<br>from account a, customers s
<br>where a.CustomerId=s.CustomerId

<br>---test function query

<br>select * from Getallcustomers(1001)
 
 
<br> -----------------------function to getalldataabout info----------------------------
 
<br> create or alter function getinfo()
<br> returns table 
<br> return 
<br> select phone , email,FirstName
<br> from Customer_phones cp, Customers c
<br> where c.CustomerId=cp.CustomerId

<br> --test funtion query 
 
<br> select * from getinfo()

<br>----------------Create table function to return loan customer data-------------------

<br>create OR ALTER  FUNCTION loan_customer_data (@id INT )
<br>RETURNS TABLE
<br>AS
<br>RETURN
<br>(
<br>--DECLARE @loanid INT ,@firstname varchar(150),@Amount FLOAT,@interestrate FLOAT  
<br>Select L.loanId,c.firstname,l.Amount,L.Interestrate
<br>from loan l join Customers c
<br>ON  c.customerId =l.customerid
<br>WHERE c.customerId= @id   
<br>)
<br>--test view query 

<br>select * from loan_customer_data (2)


<br>---------------------------------------Procedure---------------------------------------------

<br>---------------------Procedure to count employees in each department------------------------------

<br>create or alter proc total_of_num 
<br>as
<br>select count(e.employeeid) as count_num,d.DName
<br>from employee e join Department d
<br>on e.Dno= dnumber
<br>group by d.dname

<br>--test proc query 

<br>execute total_of_num 

<br>-------------proc that return the transactions that specific customer had done------------------

<br>CREATE OR ALTER proc get_customer_transaction @account_nu INT, @trans_date DATE
<br>AS
<br>    begin
<br>    if EXISTS (SELECT * FROM Account_Atm_transcation WHERE accountnumber = @account_nu)
<br>    begin
<br>        SELECT transactiondate, accountnumber,a.transactionid,Atmid
<br>		,Amount,a.transaction_type_id, TransactionType
<br>        FROM Account_Atm_transcation a join Transactions t
<br>		on a.TRANSACTION_type_ID = t.Transaction_Type_Id
<br>        WHERE a.accountnumber = @account_nu
<br>        AND a.transactiondate = @trans_date	
<br>    END
<br>    ELSE
<br>    BEGIN
<br>        PRINT 'This id is invalid'
<br>end
<br>        RETURN 
<br>    END 
<br>--test proc query

<br>--SELECT * FROM Account 
<br>EXECUTE get_customer_transaction 1002, '2023-08-19'

<br>----------proc that return the amount of money of specific transaction type-------------

<br>create or alter proc total_amount_of_specific_transaction @transaction_type varchar(20)
<br>as 
<br>select t.transactionType,a.TRANSACTION_type_ID ,sum(amount) as sum_amount_transaction
<br>from transactions t, Account_Atm_transcation a
<br>where a.TRANSACTION_type_ID = t.Transaction_Type_Id 
<br>and transactionType =  @transaction_type
<br>group by transactionType, a.Transaction_Type_Id

<br>--test proc query 

<br>--SELECT * FROM Account_Atm_transcation aat 
<br>EXECUTE total_amount_of_specific_transaction 'Withdrawal'

<br>------------Create a stored procedure to add "Dear" as a prefix to customer's name.-------------------- 

<br>CREATE OR ALTER PROCEDURE sp_Update_customer 
<br>@customerId int
<br>AS
<br>UPDATE customers 
<br>SET Firstname = Concat('Dear' , ' ' , FirstName)
<br>where CustomerId = @customerId 

<br>--test proc query 

<br>--SELECT * FROM customers

<br>EXEC sp_Update_customer 1;
  
<br>---------Create a stored procedure that accepts AccountId as a parameter and returns customer's full name and account's details.--------- 

<br>create PROCEDURE sp_Customer_Details @AccountNumber INT
<br>AS
<br>SELECT c.firstname +' ' +c.lastname AS Customer_full_name , a.accountNumber ,a.balance 
<br>FROM Customers c
<br>JOIN Account a
<br>ON c.customerID = a.customerId
<br>WHERE a.accountNumber = @AccountNumber;

<br>--test proc query

<br>EXEC sp_Customer_Details 1001;

<br>-------------Write a procedure query to remove cstate column from Customer table. -----------------------

<br>CREATE PROCEDURE sp_Remove_Column
<br>AS
<br>ALTER TABLE Customers
<br>DROP COLUMN state

<br>--test proc query      $danguer$   $danguer$   $danguer$   $danguer$   $danguer$ 

<br>EXEC sp_Remove_Column;

<br>-----------------------stored about customers their accounts are saving-----------------------

<br>create or alter proc get_allsaving 
<br>as
<br>(
<br>select firstname,accounttype,balance,age
<br>from Customers c ,Account a
<br>where c.CustomerId=a.CustomerId
<br>and accounttype='saving'
<br>)

<br>get_allsaving

<br>----------------------------------current------------------------------
<br>create proc getallcurrent as 
<br>(
<br>select firstname,accounttype,balance,age
<br>from Customers c ,Account a
<br>where c.CustomerId=a.CustomerId
<br>and accounttype='current'
<br>)

<br>getallcurrent

<br>-------------------------------credit--------------------------------------------
<br> create or alter proc getallcredit 
<br> as 
<br> select cardtype,firstname , accounttype 
<br> from card ca ,customers c , account a 
<br> where ca.accountnumber=A.accountnumber
<br> and A.customerid=c.customerid
<br> and cardtype='credit'

<br> getallcredit

<br>----------------------------------debit---------------------------------
<br> create or alter proc getalldebit 
<br> as 
<br> select cardtype,firstname , accounttype 
<br> from card ca ,customers c , account a 
<br> where ca.accountnumber=A.accountnumber
<br> and A.customerid=c.customerid
<br> and cardtype='debit'

<br> getalldebit


<br>---------------------------------------Trigger-----------------------------------------------------

<br>----------------------Trigger to update last_updated column when an employee is updated---------------

<br>ALTER TABLE employee 
<br>ADD last_updated DATETIME
    
<br>CREATE OR ALTER TRIGGER UpdateEmployeeLastUpdated
<br>ON Employee
<br>AFTER UPDATE
<br>as
<br>BEGIN
<br>    UPDATE Employee
<br>    SET last_updated = GETDATE()
<br>	FROM employee e,INSERTED i
<br>    WHERE e.employeeid = i.employeeid;
<br>END

<br>--test trigger query 

<br>UPDATE employee 
<br>SET salary+=200000
<br>WHERE employeeid=1

<br>SELECT * FROM employee 

<br>---------------------Trigger that update balance depend on the transaction type----------------------

<br>create or alter trigger update_balance
<br>on Account_Atm_transcation
<br>after insert 
<br>AS

<br> if exists (select transaction_Type_id from inserted where transaction_Type_id=1)

<br> begin 
<br> declare @amount float, @account_no int
<br> select @amount = amount, @account_no = Accountnumber from inserted
<br> update Account
<br> set balance -= @amount where Accountnumber = @account_no
<br> END

<br> else if exists(select transaction_Type_id from inserted where transaction_Type_id=2)

<br> begin 
<br> select @amount = amount, @account_no = Accountnumber from inserted
<br> update Account
<br> set balance += @amount where Accountnumber = @account_no
<br> end

<br>--test trigger query 

<br> insert into Account_Atm_transcation
<br> values ('2023-8-19',1001,1,200,500)

<br> --select* from Account
<br> -----------------------------------index------------------------------------------

<br>CREATE NONCLUSTERED INDEX idx_CustomerID ON Account (Balance)

<br>CREATE UNIQUE INDEX uni_accountnumber ON account (accountnumber) 
<br>--SELECT * FROM account
<br>SELECT * FROM Customers 
<br>----------------------------------cursor----------------------------------------------
<br>declare c9 cursor
<br>for
<br>	select FirstName,gender
<br>	from customers
<br>for update 
<br>declare @N varchar(100), @G varchar(10)
<br>open c9
<br>fetch c9 into  @N, @G
<br>WHILE @@fetch_status=0
<br>begin
<br>    if (@G = 'm' )
<br>		BEGIN
<br>		update Customers 
<br>		set FirstName = 'Mr.'+@N
<br>		WHERE CURRENT OF c9
<br>		END
<br>	else if (@G = 'f' )
<br>		BEGIN
<br>		update customers 
<br>		set FirstName = 'Mrs.'+@N
<br>		WHERE CURRENT OF c9
<br>		END
<br>fetch c9 into  @N, @G
<br>end
<br>close c9
<br>deallocate c9

<br>--test  cursor query 

<br>SELECT * FROM customers 

<br>----------------------------------default--------------------------------------
<br>create default def1 as 5000

<br>sp_bindefault  def1,'Employee.salary'

<br>--test default query 

<br>SELECT * FROM employee
<br>----------------------------------rules-----------------------------------------
<br>create rule r1 as @x>1000
<br>sp_bindrule  r1,'Employee.salary'

<br>--test rules query 

<br>SELECT * FROM employee

<br>--------------------------------------cte---------------------------------------------------
<br>-------------CREATE cte employee salary greather  than 45000 and his department name--------------

<br> WITH CTE_Employee 
<br> AS 
<br> ( SELECT FirstName, LastName,  Salary,d.dname
<br> FROM Employee e JOIN department d 
<br> ON d.Dnumber= e.Dno
<br>WHERE Salary > '45000' )
<br>--run cte 
<br>SELECT FirstName, LastName,Dname
<br>FROM CTE_Employee

<br>--SELECT * FROM employee 

<br>---------------------------------------case-----------------------------------------------
<br>----------------------create case on age to return baby,young,old-------------------------
<br>SELECT FirstName, LastName, Age,  
<br>CASE 
<br>WHEN Age > 30 THEN 'Old' 
<br>WHEN Age BETWEEN 27 AND 30 THEN 'Young' 
<br>ELSE 'Baby'
<br>END AS description_Age
<br>FROM Customers 
<br>WHERE Age IS NOT NULL 
<br>ORDER BY age
<br>-------------------------------------rank----------------------------------
<br>SELECT *, dense_RANK() OVER(PARTITION BY Dno ORDER BY Salary ) AS employe_rank
<br>FROM Employee 
<br>ORDER BY Dno, Salary

<br>--test
<br>SELECT * FROM employee

<br>-----------------------------------top -------------------------           
<br>select top 2 * from employee  
<br>-----------------------------------like -------------------------
<br>select * from employee e 
<br>where firstname like '_k%'
<br>-----------------------------------end--------------------------------


