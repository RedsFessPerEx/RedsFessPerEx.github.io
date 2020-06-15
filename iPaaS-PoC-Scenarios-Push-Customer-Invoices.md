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
* Connect to MSSQL (on-premise)
* Execute SQL and map into Service[]

### Notes
* Persist Search for future reference

### Supporting Information
- [x] Input _Schema_

| Item | Type | Type Details | Reference Name | Remarks |
| --- | --- | --- | --- | --- |
| Port | string | | @port | |
| Duration Start Date | string | ISO 8601 | @modifiedfrom | |
| Duration End Date | string | ISO 8601 | @modifiedto | |

- [x] Output _Schema_

| Item | Type | Type Details | Remarks |
| --- | --- | --- | --- |
| Service | string | | |
| Vessel | string | | |
| Voyage | string | | |
| Port | string | | |
| Terminal | string | | |
| Arrival Date | string | | |
| Departure Date | string | | |
| Berth Date | string | | |
| Unberth Date | string | ISO 8601 | |
| Created Date | string | ISO 8601 | |
| Modified Date | string | ISO 8601 | |


- [ ] MSSQL Connection Details

- [x] SQL Details

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
