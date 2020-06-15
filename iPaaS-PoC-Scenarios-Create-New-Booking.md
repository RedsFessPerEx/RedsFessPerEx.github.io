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
