# Database Management Essentials Course Assignment

## Module 3
### Requirement Summary
- Create Table requirements
1.	You should use standard SQL data types specified in the notes except for using VARCHAR2 (an Oracle data type) instead of VARCHAR for columns containing varying length character strings. For MySQL and PostgreSQL, you should use VARCHAR for variable length strings. 
2.	For primary key fields (CustNo, LocNo, EventNo, PlanNo, EmpNo, ResNo, and FacNo), use the VARCHAR (or VARCHAR2 in Oracle) data type with length 8.  For consistency, corresponding foreign keys (such as EventRequest.CustNo) should also be the same data type and length. 
3.	For Oracle, you should use the DATE data type for the columns involving dates or times. The EventPlanLine.TimeStart and EventPlanLine.TimeEnd columns will store both date and time data so you should use the DATE data type. For MySQL, you should use the DATE data type for columns with just date details (date columns in the EventRequest and EventPlan tables) and DATETIME for columns with date and time details (time columns in the EventPlanLine table). For PostgreSQL, you should use the DATE data type for columns with just date details (date columns in the EventRequest and EventPlan tables) and TIMESTAMP for columns with date and time details (time columns in the EventPlanLine table).
4.	Use CHAR(1) for the Customer.Internal column as Oracle does not provide a BOOLEAN data type.  MySQL has the Boolean data type and PostgreSQL has the BIT(1) data type, but I suggest that you use CHAR(1) instead. 

- constraints
•	For each primary key, you should specify a PRIMARY KEY constraint clause. For single column primary keys (CustNo, LocNo, EventNo, PlanNo, EmpNo, ResNo, and FacNo), the constraint clause can be inline or external. For multiple column primary keys (combination of PlanNo and LineNo), the CONSTRAINT clause must be external. 
•	For each foreign key, you should specify a FOREIGN KEY constraint clause. The constraint clauses can be inline or separate. 
•	Define NOT NULL constraints for all columns except eventplan.empno, EventRequest.DateAuth, EventRequest.BudNo, and EventPlan.Notes.  Make sure that you define NOT NULL constraints for the PK of each table. Because of MySQL syntax limitations for NOT NULL constraints (inline with no constraint name and no CONSTRAINT keyword), you should define inline NOT NULL constraints. 
•	Define a named CHECK constraint to restrict the eventrequest.status column to have a value of “Pending”, “Denied”, or “Approved”. You can use the IN operator in this constraint. In MySQL, the syntax does not allow the CONSTRAINT keyword and a constraint name for CHECK constraints. You should use the CHECK keyword followed the condition enclosed in parentheses. 
•	Define named CHECK constraints to ensure that the resource.rate and eventrequest.estaudience are greater than 0. In MySQL, you cannot use a constraint name and the CONSTRAINT keyword for CHECK constraints. In MySQL, the syntax does not allow the CONSTRAINT keyword and a constraint name for CHECK constraints. You should use the CHECK keyword followed the condition enclosed in parentheses. 
•	Define a named CHECK constraint involving EventPlanLine.TimeStart and EventPlanLineTimeEnd. The start time should be smaller (chronologically before) than the end time. This CHECK constraint must be external because it involves two columns. In MySQL, the syntax does not allow the CONSTRAINT keyword and a constraint name for CHECK constraints. You should use the CHECK keyword followed the condition ![image](https://user-images.githubusercontent.com/59367560/136861552-e1c0ebb1-de02-4516-a9b8-5bdb2da1311c.png)

- populating tables
