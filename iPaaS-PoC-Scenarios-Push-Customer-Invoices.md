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
