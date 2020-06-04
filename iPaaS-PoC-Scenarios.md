| Scenario Number | Description |
| :---: | --- |
| 1 | [Search Services](#search-services) |
| 2 | [Push Customer Invoices](#push-invoices) |
| 3 | [Create New Booking](#create-new-booking) |
| 4 | [Share Booking with JV Partner](#share-booking-with-jv-partner) |





# Search Services

### Purpose
(Ad-hoc) Search for services servicing specified Port over specified Duration

### Input
* Port
* Duration Start Date
* Duration End Date

### Output
* Service[]
  * Service
  * Vessel
  * Voyage
  * Port
  * Terminal
  * Arrival Date
  * Departure Date
  * Berth Date
  * Unberth Date
  * Created Date
  * Modified Date

### Mechanism
* Connect to MSSQL
* SELECT

### Notes
* Persist Search for future reference

### Supporting Information
- [ ] Input _Schema_
- [ ] Output _Schema_
- [ ] MSSQL Connection Details
- [ ] SELECT Details





# Push Invoices

### Purpose
(Scheduled) Push of customer invoices

### Input
* Document Number

### Output
* Invoice Details

### Mechanism
* Connect to MSSQL
* SELECT
  * WHERE _Document Number_
* Map to EDI INVOICE
* Connect to SAP
  * Call _Web Service_ to submit Invoice

### Supporting Information
- [ ] Input _Schema_
- [ ] Output _Schema_
- [ ] MSSQL Connection Details
- [ ] SELECT Details
- [ ] SAP Connection Details
- [ ] EDI INVOICE Details





# Create New Booking

### Purpose
(Ad-hoc) Create new Booking

### Input
* EDI BOOKING

### Output
* EDI ACKNOWLEDGEMENT

### Mechanism
* Retrieve _EDI BOOKING_ from FTP
* Perform Master Data Mapping
* Connect to MSSQL
* INSERT
* Create _EDI ACKNOWLEDGEMENT_
* Deposit into FTP

### Supporting Information
- [ ] Input _Schema_
- [ ] Output _Schema_
- [ ] FTP Details
- [ ] MSSQL Connection Details
- [ ] INSERT Details
- [ ] EDI INVOICE Details
- [ ] EDI ACKNOWLEDGEMENT Details





# Share Booking with JV Partner

### Purpose
(Ad-hoc) Share Booking with JV Partner

### Input
* Service
* Vessel
* Voyage
* Port
* Start Date
* End Date

### Output
* EDI BOOKING (COPARN)

### Mechanism
* Connect to MSSQL
  * VIEW
* Create _EDI BOOKING (COPARN)_
* Email Customer

### Supporting Information
- [ ] Input _Schema_
- [ ] Output _Schema_
- [ ] MSSQL Connection Details
- [ ] VIEW Details
- [ ] EDI BOOKING Details
- [ ] Email Details
