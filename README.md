# Draw  ERD  with Crow’s foot notation and prepare  table with PostgreSQL and insert sample data
## Question: Draw the Entity- Relationship Diagram (ERD) for the following scenario:
### Business rules:
* A salesperson may manage many other salespeople. A salesperson is managed by only one salespeople. A salesperson can be an agent for many customers.

* A customer is managed by one salespeople. A customer can place many orders. 

* An order can be placed by one customer. An order lists many inventory items.

* An inventory item may be listed on many orders. An inventory item is assembled from many parts. A part may be assembled into many inventory items. 

* Many employees assemble an inventory item from many parts.  A supplier supplies many parts. A part may be supplied by many suppliers.

## ERD in chen notation:<br>
Multidisciplinary Data Analyst with 8 years of experience, including over 4 years as a Data Management Associate at UNHCR, Cox's Bazar, and 4 years in web development. Expert in data management, planning, and coordination. Led major projects at UNHCR, Cox's Bazar, overseeing over 20 distribution campaigns, including non-food items and health-related vaccinations, and collecting 13M+ assistance records. Proficient in planning, targeting, data collection, data cleaning (Excel, Python, SQL), and reporting. Skilled in data wrangling, analysis (SQL, R, Python, Excel), and visualization (Power BI, Tableau). Fluent in English.
<img align="left" alt="erd_with_chen_notation | LinkedIn" width="1000px" src="https://github.com/maeshakib/z_resources/blob/main/salesperson_erd_with_chen_notation_20240609_01.png" /> <br>



## ERD in crow's foot notation:<br>
Multidisciplinary Data Analyst with 8 years of experience, including over 4 years as a Data Management Associate at UNHCR, Cox's Bazar, and 4 years in web development. Expert in data management, planning, and coordination. Led major projects at UNHCR, Cox's Bazar, overseeing over 20 distribution campaigns, including non-food items and health-related vaccinations, and collecting 13M+ assistance records. Proficient in planning, targeting, data collection, data cleaning (Excel, Python, SQL), and reporting. Skilled in data wrangling, analysis (SQL, R, Python, Excel), and visualization (Power BI, Tableau). Fluent in English.

<img align="left" alt="erd_with_chen_notation | LinkedIn" width="1000px" src="https://github.com/maeshakib/z_resources/blob/main/ERD_with%20crows_foot_notation.png" /><br>
 
## postgresql  table creation and sample data insert code:<br>
Multidisciplinary Data Analyst with 8 years of experience, including over 4 years as a Data Management Associate at UNHCR, Cox's Bazar, and 4 years in web development. Expert in data management, planning, and coordination. Led major projects at UNHCR, Cox's Bazar, overseeing over 20 distribution campaigns, including non-food items and health-related vaccinations, and collecting 13M+ assistance records. Proficient in planning, targeting, data collection, data cleaning (Excel, Python, SQL), and reporting. Skilled in data wrangling, analysis (SQL, R, Python, Excel), and visualization (Power BI, Tableau). Fluent in English.

```sql
CREATE TABLE salesperson (
    SalesPersonId int not null PRIMARY key,
    PersonName VARCHAR   ,
	 ManagerId int  ,
	DateOfBirth TIMESTAMP,
	FOREIGN key (ManagerId) REFERENCES salesperson(SalesPersonId)
);




CREATE TABLE customer (
    CustomerId int not null PRIMARY key,
    CustomerName VARCHAR  ,
	 SalesPersonAgentIdFK int  ,
	foreign key(SalesPersonAgentIdFK) REFERENCES salesperson(SalesPersonId)
);



CREATE TABLE ProdOrder (
    OrderId int not null PRIMARY key,
	OrderDate TIMESTAMP,    
    CustomerIdFK int,
	FOREIGN key(CustomerIdFK) REFERENCES customer(CustomerId)
);



-- InventoryItem table to store information about inventory items
CREATE TABLE InventoryItem (
    InventoryItemId int not null PRIMARY KEY,
    ItemName VARCHAR NOT NULL
);
--middle table
 
CREATE TABLE OrderInventoryItem (
    OrderIdFK INT NOT NULL,
    InventoryItemIdFK INT NOT NULL,
    Quantity INT NOT NULL,
    PRIMARY KEY (OrderIdFK, InventoryItemIdFK),
    FOREIGN KEY (OrderIdFK) REFERENCES  ProdOrder (OrderId),
    FOREIGN KEY (InventoryItemIdFK) REFERENCES InventoryItem(InventoryItemId)
);

-- Part table to store information about parts
CREATE TABLE Part (
    PartId int not null PRIMARY KEY,
    PartName VARCHAR NOT NULL
);


-- Junction table to establish many-to-many relationship between inventory items and parts
CREATE TABLE InventoryItemPart (
    InventoryItemIdFK INT NOT NULL,
    PartIdFK INT NOT NULL,
    PRIMARY KEY (InventoryItemIdFK, PartIdFK),
    FOREIGN KEY (InventoryItemIdFK) REFERENCES InventoryItem(InventoryItemId),
    FOREIGN KEY (PartIdFK) REFERENCES Part(PartId)
);
 

CREATE TABLE supplier (
    SupplierId int not null PRIMARY key,
	SuplierName VARCHAR,
	ContactPerson varchar,
	Email varchar,
	Address varchar
 
);

CREATE TABLE SupplierPart (
   
 SupplierIdFK int not null  ,
 PartIDFK int not null,
PRIMARY KEY (SupplierIdFK, PartIDFK),
FOREIGN KEY (SupplierIdFK) REFERENCES supplier(SupplierId),
FOREIGN KEY (PartIDFK) REFERENCES Part(PartId)
);


CREATE TABLE employee (
    EmployeeId int not null PRIMARY key,
	EmployeeName VARCHAR,
	Designation varchar,
	ContactNo varchar 
 
);

CREATE TABLE EmployeeInventoryItemAssemble (
EmployeeIdFK int,
InventoryItemIdFK int ,
PRIMARY KEY (EmployeeIdFK, InventoryItemIdFK),
FOREIGN KEY (EmployeeIdFK) REFERENCES employee(EmployeeId),
FOREIGN KEY (InventoryItemIdFK) REFERENCES InventoryItem(InventoryItemId)
);

SELECT * from customer

--Insert Data for salesperson

INSERT INTO salesperson (SalesPersonId, PersonName, ManagerId, DateOfBirth) VALUES 
(1, 'John Doe', NULL, '1980-01-15'),
(2, 'Jane Smith', 1, '1985-07-20'),
(3, 'Alice Johnson', 1, '1990-03-25');


--Insert Data for customer

INSERT INTO customer (CustomerId, CustomerName, SalesPersonAgentIdFK) VALUES 
(1, 'ACME Corporation', 1),
(2, 'Globex Corporation', 2),
(3, 'Soylent Corp', 3);


--Insert Data for ProdOrder

INSERT INTO ProdOrder (OrderId, OrderDate, CustomerIdFK) VALUES 
(1, '2024-05-01 10:00:00', 1),
(2, '2024-05-02 12:00:00', 2),
(3, '2024-05-03 14:00:00', 3);


--Insert Data for InventoryItem

INSERT INTO InventoryItem (InventoryItemId, ItemName) VALUES 
(1, 'Widget A'),
(2, 'Gadget B'),
(3, 'Thingamajig C');


--Insert Data for OrderInventoryItem

INSERT INTO OrderInventoryItem (OrderIdFK, InventoryItemIdFK, Quantity) VALUES 
(1, 1, 10),
(1, 2, 5),
(2, 2, 7),
(3, 1, 2),
(3, 3, 1);


--Insert Data for Part

INSERT INTO Part (PartId, PartName) VALUES 
(1, 'Part X'),
(2, 'Part Y'),
(3, 'Part Z');


--Insert Data for InventoryItemPart

INSERT INTO InventoryItemPart (InventoryItemIdFK, PartIdFK) VALUES 
(1, 1),
(1, 2),
(2, 2),
(2, 3),
(3, 1),
(3, 3);


--Insert Data for supplier
 
INSERT INTO supplier (SupplierId, SuplierName, ContactPerson, Email, Address) VALUES 
(1, 'Supplier One', 'Sam Supplier', 'sam@supplierone.com', '123 Supplier St.'),
(2, 'Supplier Two', 'Sally Supplier', 'sally@suppliertwo.com', '456 Supplier Ave.');


--Insert Data for SupplierPart
 
INSERT INTO SupplierPart (SupplierIdFK, PartIDFK) VALUES 
(1, 1),
(1, 2),
(2, 2),
(2, 3);


--Insert Data for employee
 
INSERT INTO employee (EmployeeId, EmployeeName, Designation, ContactNo) VALUES 
(1, 'Bob Builder', 'Engineer', '555-1234'),
(2, 'Eve Engineer', 'Technician', '555-5678');


--Insert Data for EmployeeInventoryItemAssemble
 
INSERT INTO EmployeeInventoryItemAssemble (EmployeeIdFK, InventoryItemIdFK) VALUES 
(1, 1),
(1, 2),
(2, 2),
(2, 3);


---------------- select scripts


--salesperson and customer orders
select s.personname,s.managerid,c.customername,p.orderid,p.orderdate from salesperson s join customer c 
on s.salespersonid = c.salespersonagentidfk
join prodorder p ON p.customeridfk = c.customerid

-- all customers orders and item details
select p.customeridfk,p.orderid,p.orderdate , invi.inventoryitemid,invi.itemname,oinvi.quantity
from prodorder p JOIN orderinventoryitem oinvi 
on p.orderid=oinvi.orderidfk  
join inventoryitem invi ON invi.inventoryitemid = oinvi.inventoryitemidfk


-- all employees assembles following items


select e.employeename,invi.itemname,einvi.inventoryitemidfk,invip.partidfk

from
employee e join employeeinventoryitemassemble einvi  
ON einvi.employeeidfk = e.employeeid 
join inventoryitem invi 
ON invi.inventoryitemid = einvi.inventoryitemidfk
join inventoryitempart invip ON invip.inventoryitemidfk = invi.inventoryitemid

