# BankSystemDesign
# First we reseved a required document from client

![Screenshot 2023-10-02 195047](https://github.com/Mario-farouk/BankSystemDesign/assets/136117708/5f74245b-ad53-4da0-9db2-c2d56b453416)
# Secound we start to Drow the ERD 
![ERD](https://github.com/Mario-farouk/BankSystemDesign/assets/136117708/d0678ad1-73d9-419e-bdbd-30db41bcfc4c)
# third Mapping 

![Mapping](https://github.com/Mario-farouk/BankSystemDesign/assets/136117708/fa82748a-00b3-4f3a-9062-7d8786c7b822)
# then impliment physical schema   

![05_OLTP_schema](https://github.com/Mario-farouk/BankSystemDesign/assets/136117708/00b74b32-aa4a-4d7d-be11-3d5d177b669f)

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
# Denormalized Data Base star Schema 
<br> Business Process "Transactions "
<br> Grain "Low Level"
<br> Dimensions -----> Account - Branch - Customer -ATM - Date - card 
<br> Fact Table : Transaction Table 

# Star Schema
![OLAP_schema](https://github.com/Mario-farouk/BankSystemDesign/assets/136117708/958f8dcc-3156-4e32-810f-fc30ff025c07)

# Business Intelligence Using SSIS SSAS SSRS 
# First Integration using SSIS

![Atm_BranchDims](https://github.com/Mario-farouk/BankSystemDesign/assets/136117708/7b29f9a0-fe0f-4b65-a96c-743ea04e68c0)

# Secound Cube "The cube are Genarlized About Businnes CEOs and CMOs should closely track financial performance, set clear goals, and analyze key metrics to measure profits. They must identify and manage risks by conducting assessments and creating strategies to mitigate potential threats."
![Cube](https://github.com/Mario-farouk/BankSystemDesign/assets/136117708/1dc9ce9b-360f-41bf-9d89-d278387ee4ff)

# Third SSRS 
![Screenshot (122)](https://github.com/Mario-farouk/BankSystemDesign/assets/136117708/5fdf312b-161c-42b1-b22e-9ce07a9d9330)







