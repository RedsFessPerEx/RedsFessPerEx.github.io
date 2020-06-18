[Scenarios Index](iPaaS-PoC-Scenarios)

# Share Booking with JV Partner

### Purpose
(Ad-hoc) Share Booking with JV Partner

### Input
* Vessel
* Voyage
* Port
* Call Seq
* MLO

### Output
* COPRAR Document []
  - COPRAR Document

### Mechanism
The actual EDI Partner connectivity (like SFTP, AS2) will not be used in this PoC. Instead, the connectivity (send) will be _simulated_ by with an API Output.
* Connect to MSSQL
* Execute SQL
* Map data into one or more _EDI (COPARN)_

### Supporting Information
- [ ] Input _Schema_

| Item | Type | Type Details | Reference Name | Remarks |
| --- | --- | --- | --- | --- |
| Vessel | string | | @vessel | |
| Voyage | string |  | @voyage | |
| Port | string |  | @port | |
| Call Seq | int |  | @callseq | |
| MLO | string |  | @mlo | |


- [x] Output _Schema (COPRAR Document)_

| Item | Type | Type Details | Remarks |
| --- | --- | --- | --- |
| COPRAR Document | string | | Verbatim EDI (COPRAR) |

- [x] MSSQL Connection Details

| Item | Value | Remarks |
| --- | --- | --- |
| Data Source | 192.168.176.71\TCMSQA | |
| Initial Catalog | TCMS_6_Dev |   |
| User ID  | TCMSUser  |   |
| Password   | tgb123.DB.01  |   |

- [x] SQL Details

#### SELECT

| Item | Type | Type Details | Remarks |
| --- | --- | --- | --- |
| Detail.BookingReference | | |
| Detail.CargoStatus | | |
| Detail.Commodity | | |
| Detail.ContainerNo | | |
| Detail.ETA | | |
| Detail.FP | | |
| Detail.FPOD | | |
| Detail.GoodsDescrp | | |
| Detail.GrossWeight | | |
| Detail.IMCO | | |
| Detail.ISOContainerType | | |
| Detail.MLO | | |
| Detail.MLOPO | | |
| Detail.MotherVessel | | |
| Detail.MotherVesselCallSign | | |
| Detail.MotherVoyage | | |
| Detail.OLA | | |
| Detail.OLF | | |
| Detail.OOH | | |
| Detail.OWP | | |
| Detail.OWS | | |
| Detail.Pod | | |
| Detail.PodTerminal | | |
| Detail.Pol | | |
| Detail.PolTerminal | | |
| Detail.SealNo | | |
| Detail.TareWeight | | |
| Detail.TempMax | | |
| Detail.TempMin | | |
| Detail.TempOpt | | |
| Detail.UN | | |
| Detail.VesselName | | |
| Detail.VGMWeight | | |
| Detail.Voyage | | |
| Header.Filename | | |
| Header.InterchangeRef | | |
| Header.MessageType | | |
| Header.ReceivedDateTime | | |
| Header.Recevier | | |
| Header.Sender | | |
| Header.SenderFileType | | |
| Header.SenderFileVersio | | |


#### FROM / WHERE

| Item | Details | Remarks |
| --- | --- | --- |
| FROM | T_EDI_Header Header | |
| INNER JOIN   |  T_EDI_Detail_Booking Detail on Header.Pkey=Detail.TEDIHeaderPkey |   |
| WHERE | Header.AuditStat=1  | |
| WHERE | Detail.AuditStat=1 | |
| WHERE | Detail.Vessel=@vessel  |   |
| WHERE | Detail.voyage=@voyage |   |
| WHERE | Detail.Pol=@port |   |
| WHERE | Detail.PolCallSeq=@callseq  |   |
| WHERE | Detail.mlo=@mlo |   |

#### Mapping Details

| SQL Field | Data Types | EDI Segment | M/C/O | Remarks |
| --- | --- | --- | --- | --- |
| Header.Sender | Varchar(100) | Partner's ID | M | Partner's ID |
| Header.Recevier | Varchar(100) | XPF | M | XPF |
| Header.SenderFileType | Varchar(100) | COPRAR | M | COPRAR |
| Header.SenderFileVersion | Varchar(100) | D95B | M | D95B |
| Header.Filename | Varchar(200) |  | M | Name of the file |
| Header.ReceivedDateTime | Datetime |  | M | Received datetime |
| Header.InterchangeRef | varchar(13) |  | M | Unique Reference Id for edi message |
| Header.MessageType | int |  | M | New-9, Replace-5, Cancel-1 |
| Detail.VesselName | Varchar(100) | SegmentGroup1.TDT+20 -> C222.8212 | O | Depends on location requirement |
| Detail.Voyage | varchar(20) | SegmentGroup1.TDT+20 -> C220.8067 | O | Depends on location requirement |
| Detail.Pol | varchar(20) | SegmentGroup1.LOC+9 -> C517.3225 | M | UN/LOCODE |
| Detail.PolTerminal | Varchar(100) | SegmentGroup1.LOC+9 -> C519.3223 | M | Either code or description must be provided. May required data mapping |
| Detail.ETA | Datetime | SegmentGroup1.DTM+132 -> C507.2379 | O | Depends on location requirement |
| Detail.Pod | varchar(20) | SegmentGroup3.LOC+11 -> C517.3225 | M | UN/LOCODE |
| Detail.PodTerminal | Varchar(100) | SegmentGroup3.LOC+11 -> C519.3223 | M | Either code or description must be provided. May required data mapping |
| Detail.MLO | varchar(10) | Applicable MLO | M | Xpress MLO Code |
| Detail.BookingReference | varchar(70) | SegmentGroup3.RFF+BM -> 1154 | M | Mandatory. Use for change booking. If no booking reference, please fill with PO |
| Detail.MLOPO | varchar(70) | SegmentGroup3.RFF+BM -> 1154 | M | Mandatory if customer needs it for invoicing spliting by PO |
| Detail.MotherVessel | Varchar(200) | SegmentGroup3.TDT+10/30 -> C222.8212 | O | Depends on location requirement |
| Detail.MotherVoyage | Varchar(40) | SegmentGroup3.TDT+10/30 -> C220.8067 | O | Depends on location requirement |
| Detail.MotherVesselCallSign | Varchar(20) | SegmentGroup3.TDT+10/30 -> C222.8213 | O | Depends on location requirement |
| Detail.ISOContainerType | Varchar(20) | SegmentGroup3.EQD+CN -> C224.8155 | O | May required data mapping if container code is customer internal c ode |
| Detail.Commodity | Varchar(4) | SegmentGroup3.EQD+CN -> 8169 | M | Empty/Full |
| Detail.GrossWeight | float | SegmentGroup3.MEA+WT+G+KGM -> C174.6314 | C | Depends on location requirement |
| Detail.VGMWeight | float | SegmentGroup3.MEA+WT+VGM+KGM -> C174.6314 | M | Depends on location requirement |
| Detail.TareWeight | float | SegmentGroup3.MEA+WT+T+KGM -> C174.6314 | M | Depends on location requirement |
| Detail.ContainerNo | Varchar(30) | SegmentGroup3.EQD+CN -> C237.8260 | M | Equipment ID |
| Detail.SealNo | Varchar(200) | SegmentGroup3.SEL.9308 | M | Depends on location manifest or B/L requirement |
| Detail.GoodsDescrp | Varchar(3000) | SegmentGroup3.FTX+AAA -> C108.4440 | M | Depends on location manifest or B/L requirement |
| Detail.CargoStatus | Varchar(4) | SegmentGroup3.FTX+CCI -> C108.4440 | M | Depends on location manifest or B/L requirement |
| Detail.FPOD | Varchar(100) | SegmentGroup3.LOC+88 -> C519.3223 | M | Depends on location manifest or B/L requirement |
| Detail.TempMin | float | SegmentGroup3.RNG+5 -> C280.6162 | M | Mandatory for Reefer Cargo if no transport temperature provided |
| Detail.TempOpt | float | SegmentGroup3.RNG+5 -> C280.6162 | M | Mandatory for Reefer Cargo if no transport temperature provided |
| Detail.TempMax | float | SegmentGroup3.RNG+5 -> C280.6152 | M | Mandatory for Reefer Cargo if no transport temperature provided |
| Detail.IMCO | Varchar(20) | SegmentGroup3.DGS+IMD -> C205.8351 | M | Mandatory for DG Cargo.  |
| Detail.UN | int | SegmentGroup3.DGS+IMD -> C234.7124 | M | Mandatory for DG Cargo. Should be ISO UN Code. |
| Detail.FP | float | SegmentGroup3.DGS+IMD -> C223.7106 | O | Optional for DG Cargo. Flash Point Should be Numeric. |
| Detail.OLA | float | SegmentGroup3.DIM+6->C221.6168 | M | Mandatory for OOG cargo If Exceeds the Length at back |
| Detail.OLF | float | SegmentGroup3.DIM+6->C221.6168 | M | Mandatory for OOG cargoIf Exceeds the Length at front |
| Detail.OOH | float | SegmentGroup3.DIM+9->C221.6008 | M | Mandatory for OOG cargoIf Exceeds the Length at heigh |
| Detail.OWP | float | SegmentGroup3.DIM+8->C221.6140 | M | Mandatory for OOG cargoIf Exceeds the width at left |
| Detail.OWS | float | SegmentGroup3.DIM+7->C221.6140 | M | Mandatory for OOG cargoIf Exceeds the width at right |
