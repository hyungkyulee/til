# DBMS Overview

## Database Management Essential
### Definitions / Terminologies
- DBMS : Database Management System
- Power User : The most active functional users
- Transaction : a unit of work that should be processed reliably without interference from other users and without loss of data due to failures
- Persistent : stability, reliablility


Query clause Evaluation order
- Rows -> Groups -> Result
> - Rows : Rows operations before group operation
    - From
    - Where
  - Groups (column) : only happen one time, and calculate summary data for decision making
    - Group by
    - Having
  - Result
    - Order by
    - Select
