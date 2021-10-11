# Database Management Essentials Course Assignment

## Module 3
### Requirement Summary
#### Create Table requirements
1.	You should use standard SQL data types specified in the notes except for using VARCHAR2 (an Oracle data type) instead of VARCHAR for columns containing varying length character strings. For MySQL and PostgreSQL, you should use VARCHAR for variable length strings. 
2.	For primary key fields (CustNo, LocNo, EventNo, PlanNo, EmpNo, ResNo, and FacNo), use the VARCHAR (or VARCHAR2 in Oracle) data type with length 8.  For consistency, corresponding foreign keys (such as EventRequest.CustNo) should also be the same data type and length. 
3.	For Oracle, you should use the DATE data type for the columns involving dates or times. The EventPlanLine.TimeStart and EventPlanLine.TimeEnd columns will store both date and time data so you should use the DATE data type. For MySQL, you should use the DATE data type for columns with just date details (date columns in the EventRequest and EventPlan tables) and DATETIME for columns with date and time details (time columns in the EventPlanLine table). For PostgreSQL, you should use the DATE data type for columns with just date details (date columns in the EventRequest and EventPlan tables) and TIMESTAMP for columns with date and time details (time columns in the EventPlanLine table).
4.	Use CHAR(1) for the Customer.Internal column as Oracle does not provide a BOOLEAN data type.  MySQL has the Boolean data type and PostgreSQL has the BIT(1) data type, but I suggest that you use CHAR(1) instead. 

#### constraints
1. For each primary key, you should specify a PRIMARY KEY constraint clause. For single column primary keys (CustNo, LocNo, EventNo, PlanNo, EmpNo, ResNo, and FacNo), the constraint clause can be inline or external. For multiple column primary keys (combination of PlanNo and LineNo), the CONSTRAINT clause must be external. 
2. For each foreign key, you should specify a FOREIGN KEY constraint clause. The constraint clauses can be inline or separate. 
3. Define NOT NULL constraints for all columns except eventplan.empno, EventRequest.DateAuth, EventRequest.BudNo, and EventPlan.Notes.  Make sure that you define NOT NULL constraints for the PK of each table. Because of MySQL syntax limitations for NOT NULL constraints (inline with no constraint name and no CONSTRAINT keyword), you should define inline NOT NULL constraints. 
4. Define a named CHECK constraint to restrict the eventrequest.status column to have a value of “Pending”, “Denied”, or “Approved”. You can use the IN operator in this constraint. In MySQL, the syntax does not allow the CONSTRAINT keyword and a constraint name for CHECK constraints. You should use the CHECK keyword followed the condition enclosed in parentheses. 
5. Define named CHECK constraints to ensure that the resource.rate and eventrequest.estaudience are greater than 0. In MySQL, you cannot use a constraint name and the CONSTRAINT keyword for CHECK constraints. In MySQL, the syntax does not allow the CONSTRAINT keyword and a constraint name for CHECK constraints. You should use the CHECK keyword followed the condition enclosed in parentheses. 
6. Define a named CHECK constraint involving EventPlanLine.TimeStart and EventPlanLineTimeEnd. The start time should be smaller (chronologically before) than the end time. This CHECK constraint must be external because it involves two columns. In MySQL, the syntax does not allow the CONSTRAINT keyword and a constraint name for CHECK constraints. You should use the CHECK keyword followed the condition 

#### populating tables
INSERT ... VALUES ...

#### Query examples
```
-- 1.1 varchar on postgreSQL
-- 1.2 VARCHAR(8) : PK (CustNo, LocNo, EventNo, PlanNo, EmpNo, ResNo, and FacNo)
-- 1.2 VARCHAR(8) : FK
-- 1.3 DATE : date columns in the EventRequest and EventPlan tables
-- 1.3 TIMESTAMP  : time columns in the EventPlanLine table
-- 1.4 CHAR(1) : Customer.Internal column
-- 2.1 PK constraint, inline(or external), external for a multiple PK
-- 2.2 FK constraint, inline(or separate)
-- 2.3 NOT NULL for all (except, eventplan.empno, EventRequest.DateAuth, EventRequest.BudNo, and EventPlan.Notes)
-- 2.4 CHECK for eventrequest.status (use IN operator with “Pending”, “Denied”, or “Approved”)
-- 2.5 CHECK for resource.rate and eventrequest.estaudience ( > 0)
-- 2.6 CHECK for EventPlanLine.TimeStart and EventPlanLineTimeEnd (start time < end time), external
CREATE TABLE Employee (
	EmpNo		VARCHAR(8)		NOT NULL,
	EmpName		VARCHAR(32)		NOT NULL,
	EmpEmail	VARCHAR(128)	NOT NULL,
	CONSTRAINT EmployeePK PRIMARY KEY (EmpNo)
);
INSERT INTO Employee (EmpNo, EmpName, EmpEmail) VALUES ('E500', 'Tester', 'test@gmail.com');

CREATE TABLE ResourceTbl (
	ResNo		VARCHAR(8)		NOT NULL,
	ResName		VARCHAR(32)		NOT NULL,
	ResRate		DECIMAL(5,2)	NOT NULL,
	CONSTRAINT 	ResrouceTblPK 	PRIMARY KEY (ResNo)
);
INSERT INTO ResourceTbl (ResNo, ResName, ResRate) VALUES ('R1001', 'fisherman', '14.00');

CREATE TABLE EventRequest (
	EventNo		VARCHAR(8)		NOT NULL,
	EventName	VARCHAR(8)		NOT NULL,
	DateAuth	DATE,
	BudNo		VARCHAR(32),
	Status		VARCHAR(16)		NOT NULL,
	Estaudience	INTEGER			NOT NULL,
	CustNo		VARCHAR(8)		NOT NULL,
	FacNo		VARCHAR(8)		NOT NULL,
	CONSTRAINT	EventRequestPK	PRIMARY KEY (EventNo),
	CONSTRAINT	CustomerFK		FOREIGN KEY (CustNo) REFERENCES Customer (CustNo),
	CONSTRAINT	FacilityFK		FOREIGN KEY (FacNo) REFERENCES Facility (FacNo),
	CHECK		(Status IN ('Pending', 'Denied', 'Approved')),
	CHECK		(Estaudience > 0)
);
INSERT INTO EventRequest (EventNo, EventName, DateAuth, BudNo, Status, Estaudience, CustNo, FacNo) 
VALUES ('E300', 'Fishing', '2021-10-25', 'B901', 'Approved', '10', 'C100', 'F100');

CREATE TABLE EventPlan (
	PlanNo		VARCHAR(8)		NOT NULL,
	EventDate	DATE			NOT NULL,
	PlanNote	VARCHAR(256),
	EmpNo		VARCHAR(8),
	EventNo		VARCHAR(8)		NOT NULL,
	CONSTRAINT	EventPlanPK		PRIMARY KEY	(PlanNo),
	CONSTRAINT	EmployeeFK		FOREIGN KEY	(EmpNo) REFERENCES Employee (EmpNo),
	CONSTRAINT	EventRequestFK	FOREIGN KEY	(EventNo) REFERENCES EventRequest (EventNo)
);
INSERT INTO EventPlan (PlanNo, EventDate, PlanNote, EmpNo, EventNo) 
VALUES ('P700', '2021-12-24', 'Test Event', 'E500', 'E300');

CREATE TABLE EventPlanLine (
	LineNo		VARCHAR(8)		NOT NULL,
	PlanNo		VARCHAR(8)		NOT NULL,
	TimeStart	TIMESTAMP		NOT NULL,
	TimeEnd		TIMESTAMP		NOT NULL,
	LocNo		VARCHAR(8)		NOT NULL,
	CONSTRAINT	EventPlanLinePK	PRIMARY KEY	(LineNo, PlanNo),
	CONSTRAINT	LocationFK		FOREIGN KEY	(LocNo) REFERENCES Location (LocNo),
	CHECK		(TimeStart < TimeEnd)
);
INSERT INTO EventPlanLine (LineNo, PlanNo, TimeStart, TimeEnd, LocNo) 
VALUES ('L800', 'P700', '2021-12-24 7:00','2021-12-25 16:30', 'L100');

CREATE TABLE Customer (
  CustNo   CHAR(8)     	NOT NULL,
  CustName VARCHAR(32) 	NOT NULL,
  Address  VARCHAR(64) 	NOT NULL,
  Internal CHAR(1)     	NOT NULL,
  CONSTRAINT CustomerPk PRIMARY KEY (CustNo)
);

INSERT INTO Customer (CustNo, CustName, Address, Internal) VALUES ('C100', 'Hyungkyu', '6, Kingston', 1);
  
CREATE TABLE Facility (
  FacNo   CHAR(8)    	NOT NULL,
  FacName VARCHAR(32) 	NOT NULL,
  CONSTRAINT FacilityPK PRIMARY KEY (FacNo)
);

INSERT INTO Facility (FacNo, FacName) VALUES ('F100', 'Football stadium');

CREATE TABLE Location (
  LocNo   CHAR(8)     	NOT NULL,
  FacNo   CHAR(8)     	NOT NULL,
  LocName VARCHAR(32) 	NOT NULL,
  CONSTRAINT LocationPK PRIMARY KEY (LocNo),
  CONSTRAINT FacilityFK FOREIGN KEY (FacNo) REFERENCES Facility (FacNo)
);

INSERT INTO Location (LocNo, FacNo, LocName) VALUES ('L100', 'F100', 'Locker room');
```

![image](https://user-images.githubusercontent.com/59367560/136868595-67b1c9fc-9adf-4871-bcbb-fbf3fa22d569.png)
