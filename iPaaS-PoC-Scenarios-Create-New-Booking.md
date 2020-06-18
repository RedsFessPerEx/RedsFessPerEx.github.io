[Scenarios Index](iPaaS-PoC-Scenarios)

# New Booking

### Purpose
(Ad-hoc) Receive and Process new Booking

### Input
* EDI (COPRAR)

### Output
* None

### Mechanism
The actual EDI Partner connectivity (like SFTP, AS2) will not be used in this PoC. Instead, the connectivity (receive) will be _simulated_ by with an API Input.
* Retrieve _EDI Document_ [Sample 1](Samples/003/01.%20COPRAR_GP.edi) [Sample 2](Samples/003/02.COPRAR_DG.edi) [Sample 3](Samples/003/03.COPRAR%20_Reefer..edi) [Sample 4](Samples/003/04.COPRAR_OOG.edi)
* Map data
  - One T_EDI_HEADER
  - One or more T_EDI_DETAIL_BOOKING
* Connect to MSSQL
* Execute SQL
  - Insert one record into T_EDI_HEADER
  - Insert one or more records into T_EDI_DETAIL_BOOKING

### Supporting Information
- [x] Mapping Details

#### For T_EDI_Header Table

| SQL Field | Data Types | EDI Segment | M/C/O | Remarks |
| --- | --- | --- | --- | --- |
| Sender | Varchar(100) | Partner's ID | M | Partner's ID |
| Recevier | Varchar(100) | XPF | M | XPF |
| SenderFileType | Varchar(100) | COPRAR | M | COPRAR |
| SenderFileVersion | Varchar(100) | D95B | M | D95B |
| Filename | Varchar(200) |  | M | Name of the file |
| ReceivedDateTime | Datetime |  | M | Received datetime |
| InterchangeRef | varchar(13) |  | M | Unique Reference Id for edi message |
| MessageType | int |  | M | New-9, Replace-5, Cancel-1 |

#### For T_EDI_Detail_Booking Table

| SQL Field | Data Types | EDI Segment | M/C/O | Remarks |
| --- | --- | --- | --- | --- |
| VesselName | Varchar(100) | SegmentGroup1.TDT+20 -> C222.8212 | O |  |
| Voyage | varchar(20) | SegmentGroup1.TDT+20 -> C220.8067 | O |  |
| Pol | varchar(20) | SegmentGroup1.LOC+9 -> C517.3225 | M |  |
| PolTerminal | Varchar(100) | SegmentGroup1.LOC+9 -> C519.3223 | M |  |
| ETA | Datetime | SegmentGroup1.DTM+132 -> C507.2379 | O |  |
| Pod | varchar(20) | SegmentGroup3.LOC+11 -> C517.3225 | M |  |
| PodTerminal | Varchar(100) | SegmentGroup3.LOC+11 -> C519.3223 | M |  |
| MLO | varchar(10) | Applicable MLO | M |  |
| BookingReference | varchar(70) | SegmentGroup3.RFF+BM -> 1154 | M |  |
| MLOPO | varchar(70) | SegmentGroup3.RFF+BM -> 1154 | M |  |
| MotherVessel | Varchar(200) | SegmentGroup3.TDT+10/30 -> C222.8212 | O |  |
| MotherVoyage | Varchar(40) | SegmentGroup3.TDT+10/30 -> C220.8067 | O |  |
| MotherVesselCallSign | Varchar(20) | SegmentGroup3.TDT+10/30 -> C222.8213 | O |  |
| ISOContainerType | Varchar(20) | SegmentGroup3.EQD+CN -> C224.8155 | O |  |
| Commodity | Varchar(4) | SegmentGroup3.EQD+CN -> 8169 | M |  |
| GrossWeight | float | SegmentGroup3.MEA+WT+G+KGM -> C174.6314 | C |  |
| VGMWeight | float | SegmentGroup3.MEA+WT+VGM+KGM -> C174.6314 | M |  |
| TareWeight | float | SegmentGroup3.MEA+WT+T+KGM -> C174.6314 | M |  |
| ContainerNo | Varchar(30) | SegmentGroup3.EQD+CN -> C237.8260 | M |  |
| SealNo | Varchar(200) | SegmentGroup3.SEL.9308 | M |  |
| GoodsDescrp | Varchar(3000) | SegmentGroup3.FTX+AAA -> C108.4440 | M |  |
| CargoStatus | Varchar(4) | SegmentGroup3.FTX+CCI -> C108.4440 | M |  |
| FPOD | Varchar(100) | SegmentGroup3.LOC+88 -> C519.3223 | M |  |
| TempMin | float | SegmentGroup3.RNG+5 -> C280.6162 | M |  |
| TempOpt | float | SegmentGroup3.RNG+5 -> C280.6162 | M |  |
| TempMax | float | SegmentGroup3.RNG+5 -> C280.6152 | M |  |
| IMCO | Varchar(20) | SegmentGroup3.DGS+IMD -> C205.8351 | M |  |
| UN | int | SegmentGroup3.DGS+IMD -> C234.7124 | M |  |
| FP | float | SegmentGroup3.DGS+IMD -> C223.7106 | O |  |
| OLA | float | SegmentGroup3.DIM+6->C221.6168 | M |  |
| OLF | float | SegmentGroup3.DIM+6->C221.6168 | M |  |
| OOH | float | SegmentGroup3.DIM+9->C221.6008 | M |  |
| OWP | float | SegmentGroup3.DIM+8->C221.6140 | M |  |
| OWS | float | SegmentGroup3.DIM+7->C221.6140 | M |  |

- [x] MSSQL Connection Details

| Item | Value | Remarks |
| --- | --- | --- |
| Data Source | 192.168.176.71\TCMSQA | |
| Initial Catalog | TCMS_6_Dev |   |
| User ID  | TCMSUser  |   |
| Password   | tgb123.DB.01  |   |

- [x] SQL Details

#### INSERT INTO T_EDI_HEADER
| SQL Field | Type | Remarks |
| --- | --- | --- |
| Sender | Varchar(100) | |
| Recevier | Varchar(100) | |
| SenderFileType | Varchar(100) | |
| SenderFileVersion | Varchar(100) | |
| Filename | Varchar(200) | |
| ReceivedDateTime | Datetime | |
| InterchangeRef | varchar(13) | |
| MessageType | int | |

#### INSERT INTO T_EDI_DETAIL_BOOKING
| SQL Field | Type | Remarks |
| --- | --- | --- |
| TEDIHeaderPkey  | int  | SCOPE_IDENTITY()  |
| VesselName | Varchar(100) | |
| Voyage | varchar(20) | |
| Pol | varchar(20) | |
| PolTerminal | Varchar(100) | |
| ETA | Datetime | |
| Pod | varchar(20) | |
| PodTerminal | Varchar(100) | |
| MLO | varchar(10) | |
| BookingReference | varchar(70) | |
| MLOPO | varchar(70) | |
| MotherVessel | Varchar(200) | |
| MotherVoyage | Varchar(40) | |
| MotherVesselCallSign | Varchar(20) | |
| ISOContainerType | Varchar(20) | |
| Commodity | Varchar(4) | |
| GrossWeight | float | |
| VGMWeight | float | |
| TareWeight | float | |
| ContainerNo | Varchar(30) | |
| SealNo | Varchar(200) | |
| GoodsDescrp | Varchar(3000) | |
| CargoStatus | Varchar(4) | |
| FPOD | Varchar(100) | |
| TempMin | float | |
| TempOpt | float | |
| TempMax | float | |
| IMCO | Varchar(20) | |
| UN | int | |
| FP | float | |
| OLA | float | |
| OLF | float | |
| OOH | float | |
| OWP | float | |
| OWS | float | |
