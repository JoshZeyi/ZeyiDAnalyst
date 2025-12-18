Repair Shop Sales Exploratory Analysis Using My SQL
PROJECT OVERVIEW:
This report presents a Exploratory analysis of a car repair shop’s operations, focusing on customer analysis, vehicle analysis, job performance analysis, parts usage analysis, and financial analysis. The goal is to derive actionable insights and optimization recommendations to enhance the efficiency and profitability of the repair shop.

TECH STACK
MY SQL
METHODOLOGY
Data Preparation
Creating Tables:
Invoice Table: Contains invoice number, date, subtotal ,sales tax rate, total labor, total parts, total, customer ID and vehicle ID Customer Table: Stores customer ID, name, address and phone number.
Parts Table: Includes part ID ,job ID, part, part name, quantity, unit price, amount and invoiceID.
Vehicle Table: Contains vehicle ID, make, model, year ,color, vin, reg, mileage and owner name.
Job Table: Records details about each job, including job ID, VehicleID, description, hours, rate, amount, invoiceID.
CREATE DATABASE task_2
USE task_2;


CREATE TABLE `customer` (
  `CustomerID` int DEFAULT NULL,
  `Name` text,
  `Address` text,
  `Phone` text
)

INSERT INTO `customer` VALUES (1,'Jennifer Robinson','126 Nairn Ave, Winnipeg, MB, R3J 3C4','204-771-0784'),(2,'Michael Smith','250 Broadway, Winnipeg, MB, R3C 0R5','204-555-1234'),(3,'Sarah Johnson','789 Main St, Winnipeg, MB, R2W 3N2','204-666-5678'),(4,'Emily Brown','456 Elm St, Winnipeg, MB, R3M 2S5','204-777-9101'),(5,'David Wilson','123 Oak St, Winnipeg, MB, R2J 3C4','204-888-1112');


CREATE TABLE `invoice` (
  `InvoiceID` int DEFAULT NULL,
  `InvoiceDate` text,
  `Subtotal` double DEFAULT NULL,
  `SalesTaxRate` int DEFAULT NULL,
  `SalesTax` double DEFAULT NULL,
  `TotalLabour` double DEFAULT NULL,
  `TotalParts` double DEFAULT NULL,
  `Total` double DEFAULT NULL,
  `CustomerID` int DEFAULT NULL,
  `VehicleID` int DEFAULT NULL
) 
INSERT INTO `invoice` VALUES (12345,'2023-09-10',969.87,13,207.33,625,969.87,1802.2,1,1),(12346,'2023-09-15',325,13,42.25,325,250,617.25,2,2),(12347,'2023-09-20',200,13,26,200,150,376,3,3),(12348,'2023-09-25',300,13,39,300,325,664,4,4),(12349,'2023-09-30',440,13,57.2,440,340,837.2,5,5);


CREATE TABLE `job` (
  `JobID` int DEFAULT NULL,
  `VehicleID` int DEFAULT NULL,
  `Description` text,
  `Hours` double DEFAULT NULL,
  `Rate` double DEFAULT NULL,
  `Amount` double DEFAULT NULL,
  `InvoiceID` int DEFAULT NULL
)

INSERT INTO `job` VALUES (1,1,'Diagnose front wheel vibration',0.5,125,62.5,12345),(2,1,'Replace front CV Axel',3.5,125,437.5,12345),(3,1,'Balance tires',1,125,125,12345),(4,2,'Oil change',1,75,75,12346),(5,2,'Replace brake pads',2,125,250,12346),(6,3,'Replace battery',1.5,100,150,12347),(7,3,'Tire rotation',1,50,50,12347),(8,4,'Transmission check',2,150,300,12348),(9,4,'Replace air filter',0.5,50,25,12348),(10,5,'Coolant flush',1.5,120,180,12349),(11,5,'Replace spark plugs',2,130,260,12349);


CREATE TABLE `parts` (
  `PartID` int DEFAULT NULL,
  `JobID` int DEFAULT NULL,
  `Part#` text,
  `PartName` text,
  `Quantity` int DEFAULT NULL,
  `UnitPrice` double DEFAULT NULL,
  `Amount` double DEFAULT NULL,
  `InvoiceID` int DEFAULT NULL
)

INSERT INTO `parts` VALUES (1,2,'23435','CV Axel',1,876.87,876.87,12345),(2,2,'7777','Shop Materials',1,45,45,12345),(3,3,'W187','Wheel Weights',4,12,48,12345),(4,5,'54321','Brake Pads',1,200,200,12346),(5,6,'67890','Battery',1,120,120,12347),(6,7,'11223','Tire Rotation Kit',1,30,30,12347),(7,8,'33445','Transmission Fluid',1,100,100,12348),(8,9,'99887','Air Filter',1,25,25,12348),(9,10,'77654','Coolant',1,60,60,12349),(10,11,'99876','Spark Plugs',4,20,80,12349);

CREATE TABLE `vehicle` (
  `VehicleID` int DEFAULT NULL,
  `Make` text,
  `Model` text,
  `Year` int DEFAULT NULL,
  `Color` text,
  `VIN` text,
  `Reg#` text,
  `Mileage` int DEFAULT NULL,
  `OwnerName` text
)

INSERT INTO `vehicle` VALUES (1,'BMW','X5',2012,'Black','CVS123456789123-115Z','BMW 123',16495,'Jennifer Robinson'),(2,'Toyota','Corolla',2015,'White','TYS678901234567-876Z','TOY 456',45000,'Michael Smith'),(3,'Honda','Civic',2018,'Blue','HCS345678901234-123X','HON 789',30000,'Sarah Johnson'),(4,'Ford','Escape',2020,'Red','FES234567890123-456Y','FOR 987',15000,'Emily Brown'),(5,'Chevrolet','Malibu',2016,'Silver','CMS456789012345-789Z','CHE 321',60000,'David Wilson');
DATA ANALYSIS
Writing Queries:
SQL queries were written to extract data for analysis, including total sales, frequent parts used, job performance, and customer spending patterns.

## Customer Analysis ##
## Top 5 customers who have spent the most on vehicle repairs and parts ##
SELECT Name, SUM(Total) AS TotalSpent
FROM customer
join invoice
on invoice.CustomerID=customer.CustomerID 
Group by Name
Order by TotalSpent Desc;

## Average spending of customers on repairs and parts ##
SELECT AVG(Total) AS AvgSpending
FROM invoice;

## Frequency of customer visits and identifying patterns ##
SELECT OwnerName, COUNT(InvoiceID) AS Visit_Count
FROM vehicle
join job
on job.VehicleID=vehicle.VehicleID
GROUP BY OwnerName
ORDER BY Visit_Count DESC;

       ## Vehicle Analysis ##
### Average Milage of vehicle serviced ###
SELECT AVG(Mileage) AS Avg_Mileage
FROM Vehicle;

## Most common vehicle makes and models brought in for service ##
SELECT Make, Model, COUNT(JobID) AS Total_service_time
FROM job
Right join vehicle
on job.VehicleID=Vehicle.VehicleID
GROUP BY Make, Model
ORDER BY Total_service-time DESC;

## Distribution of vehicle ages and trends in service requirements based on vehicle age ##
SELECT Make, Model,(2024-Year) as Age, Description
FROM Vehicle
Right Join job 
on vehicle.vehicleid=job.vehicleid;



      
      #Job Performance Analysis#
## Most common types of jobs performed and their frequency##

SELECT Description, COUNT(JobID) AS Frequency
FROM Job
GROUP BY Description
ORDER BY Frequency DESC;

## Total revenue generated from each type of job ##
SELECT Description, SUM(Amount) AS TotalRevenue
FROM Job
GROUP BY Description
ORDER BY TotalRevenue DESC;

## Jobs with the highest and lowest average costs ##
SELECT Description, AVG(Amount) AS AvgCost
FROM Job
GROUP BY Description
ORDER BY AvgCost DESC;

    ## Part Usage Analysis ##
   ## Top 5 most frequently used parts and their total usage ##
   SELECT PartName, SUM(Quantity) AS TotalUsage
FROM Parts
GROUP BY PartName
ORDER BY TotalUsage DESC
LIMIT 5;

## Average cost of parts used in repairs ##
SELECT AVG(UnitPrice) AS AvgCost
FROM Parts;

 ## Total revenue generated from parts sales ##
 SELECT SUM(Amount) AS TotalRevenue
FROM Parts;

## Financial Analysis #
 ## Total revenue generated from labour and parts for each month ##

SELECT DATE_FORMAT(InvoiceDate, '%Y-%m') AS Month, SUM(TotalLabour) AS Total_Labour_Revenue, SUM(TotalParts) AS Total_Parts_Revenue
FROM invoice
GROUP BY Month
ORDER BY Month;

## Overall profitability of the repair shop ##
SELECT SUM(Total) AS TotalRevenue, SUM(TotalLabour) + SUM(TotalParts) AS TotalCost, (SUM(Total) - (SUM(TotalLabour) + SUM(TotalParts))) AS Profit
FROM invoice;

##Impact of sales tax on the total revenue ##
SELECT SUM(Total) AS TotalRevenue, SUM(Total) * 0.13 AS SalesTaxImpact
FROM invoice;
CONCLUSION
The analysis provided valuable insights into the repair shop’s operations. By focusing on optimizing underperforming services, efficiently managing parts inventory, implementing customer loyalty programs, and adjusting the schedule for frequent job types, the repair shop can enhance its operational efficiency and profitability.

This comprehensive approach will help in making data-driven decisions to improve service delivery, customer satisfaction, and overall business performance.

OPTIMIZATION RECOMMENDATION
Identifying Underperforming Services
Underperforming Services: Jobs like “Replace Air Filter” and “Tire Rotation” have the lowest total revenue and frequency. These services may need improvement or targeted marketing efforts to increase their demand.
Parts Inventory Management
