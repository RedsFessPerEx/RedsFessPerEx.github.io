[Scenarios Index](iPaaS-PoC-Scenarios)

# Search Services

## Purpose
(Ad-hoc) Search for services servicing specified Port over specified Duration

## Input
* Port
* Duration Start Date
* Duration End Date

## Output
* Service []
  - Service
  - Vessel
  - Voyage
  - Port
  - Terminal
  - Arrival Date
  - Departure Date
  - Berth Date
  - Unberth Date
  - Created Date
  - Modified Date

## Mechanism
* Connect to MSSQL (on-premise)
* Execute SQL
* Map SQL results into Service []

## Supporting Information

### Input _Schema_

| Item | Type | Type Details | Reference Name | Remarks |
| --- | --- | --- | --- | --- |
| Port | string | | @port | |
| Duration Start Date | string | ISO 8601 | @modifiedfrom | |
| Duration End Date | string | ISO 8601 | @modifiedto | |

### Output _Schema (Service)_

| Item | Type | Type Details | Remarks |
| --- | --- | --- | --- |
| Service | string | | |
| Vessel | string | | |
| Voyage | string | | |
| Port | string | | |
| Terminal | string | | |
| Arrival Date | string | ISO 8601 | |
| Departure Date | string | ISO 8601 | |
| Berth Date | string | ISO 8601 | |
| Unberth Date | string | ISO 8601 | |
| Created Date | string | ISO 8601 | |
| Modified Date | string | ISO 8601 | |


### MSSQL Connection Details

| Item | Value | Remarks |
| --- | --- | --- |
| Data Source | 192.168.176.71\TCMSQA | |
| Initial Catalog | TCMS_6_Dev |   |
| User ID  | TCMSUser  |   |
| Password   | tgb123.DB.01  |   |


### SQL Details

#### SELECT

| Item | Type | Type Details |  Maps to Output | Remarks |
| --- | --- | --- | --- | --- |
| Service | string | VARCHAR(5) | Service | |
| VesselCode | string | VARCHAR(10) | Vessel | |
| Voyage | string | VARCHAR(50) | Voyage | |
| Port | string | VARCHAR(5) | Port | |
| Terminal | string | VARCHAR(10) | Terminal | |
| Arrival | datetime | YYYY-MM-DD hh:mm | Arrival Date | |
| Departure | datetime | YYYY-MM-DD hh:mm | Departure Date | |
| Berth | datetime | YYYY-MM-DD hh:mm | Berth Date | |
| Unberth | datetime | YYYY-MM-DD hh:mm | Unberth Date | |
| VoyageCreatedDateTime | datetime | YYYY-MM-DD hh:mm | Created Date | |
| VoyageModifiedDate | datetime | YYYY-MM-DD hh:mm | Modified Date | |

#### FROM / WHERE

| Item | Details | Remarks |
| --- | --- | --- |
| FROM | v_VesselSchedulewithCodsBI | |
| WHERE | port = @port | |
| WHERE | VoyageModifiedDate between @modifiedfrom and @modifiedto | |


## ⭐ Sample Test Inputs

| Sample Number | Port | Duration Start Date | Duration End Date | Remarks |
| --- | --- | --- | --- | --- |
| 1 | SGSIN | 1 Jan 2019 | 31 Jan 2019 |  |
| 2 | MYPKG | 1 Feb 2019 | 28 Feb 2019 |  |
| 3 | BGCGP | 1 Mar 2019 | 31 Mar 2019 |  |
| 4 | AEJEA | 1 Apr 2019 | 30 Mar 2019 |  |

You can also vary the dates to create additional queries.
