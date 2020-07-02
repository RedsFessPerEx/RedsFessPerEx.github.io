[Scenarios Index](iPaaS-PoC-Scenarios)

# Share Booking with JV Partner

## Purpose
(Ad-hoc) Share Booking with JV Partner

## Input
* Vessel
* Voyage
* Port
* Call Seq
* MLO

## Output
* COPRAR Document []
  - COPRAR Document

## Mechanism
The actual EDI Partner connectivity (like SFTP, AS2) will not be used in this PoC. Instead, the connectivity (send) will be _simulated_ by with an API Output.
* Connect to MSSQL
* Execute SQL
* Map data into one or more _EDI (COPARN)_

## Supporting Information

### Input _Schema_

| Item | Type | Type Details | Reference Name | Remarks |
| --- | --- | --- | --- | --- |
| Vessel | string | | @vessel | |
| Voyage | string |  | @voyage | |
| Port | string |  | @port | |
| Call Seq | int |  | @callseq | |
| MLO | string |  | @mlo | |


### Output _Schema (COPRAR Document)_

| Item | Type | Type Details | Remarks |
| --- | --- | --- | --- |
| COPRAR Document | string | | Verbatim EDI (COPRAR) |

### MSSQL Connection Details

| Item | Value | Remarks |
| --- | --- | --- |
| Data Source | 192.168.176.71\TCMSQA | |
| Initial Catalog | TCMS_6_Dev |   |
| User ID  | TCMSUser  |   |
| Password   | tgb123.DB.01  |   |

### SQL Details

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
| Header.MessageType | int |  | M | |
| Detail.VesselName | Varchar(100) | SegmentGroup1.TDT+20 -> C222.8212 | O | |
| Detail.Voyage | varchar(20) | SegmentGroup1.TDT+20 -> C220.8067 | O | |
| Detail.Pol | varchar(20) | SegmentGroup1.LOC+9 -> C517.3225 | M | |
| Detail.PolTerminal | Varchar(100) | SegmentGroup1.LOC+9 -> C519.3223 | M | |
| Detail.ETA | Datetime | SegmentGroup1.DTM+132 -> C507.2379 | O | |
| Detail.Pod | varchar(20) | SegmentGroup3.LOC+11 -> C517.3225 | M | |
| Detail.PodTerminal | Varchar(100) | SegmentGroup3.LOC+11 -> C519.3223 | M | |
| Detail.MLO | varchar(10) | Applicable MLO | M | |
| Detail.BookingReference | varchar(70) | SegmentGroup3.RFF+BM -> 1154 | M | |
| Detail.MLOPO | varchar(70) | SegmentGroup3.RFF+BM -> 1154 | M | |
| Detail.MotherVessel | Varchar(200) | SegmentGroup3.TDT+10/30 -> C222.8212 | O | |
| Detail.MotherVoyage | Varchar(40) | SegmentGroup3.TDT+10/30 -> C220.8067 | O | |
| Detail.MotherVesselCallSign | Varchar(20) | SegmentGroup3.TDT+10/30 -> C222.8213 | O | |
| Detail.ISOContainerType | Varchar(20) | SegmentGroup3.EQD+CN -> C224.8155 | O | |
| Detail.Commodity | Varchar(4) | SegmentGroup3.EQD+CN -> 8169 | M | |
| Detail.GrossWeight | float | SegmentGroup3.MEA+WT+G+KGM -> C174.6314 | C | |
| Detail.VGMWeight | float | SegmentGroup3.MEA+WT+VGM+KGM -> C174.6314 | M | |
| Detail.TareWeight | float | SegmentGroup3.MEA+WT+T+KGM -> C174.6314 | M | |
| Detail.ContainerNo | Varchar(30) | SegmentGroup3.EQD+CN -> C237.8260 | M | |
| Detail.SealNo | Varchar(200) | SegmentGroup3.SEL.9308 | M | |
| Detail.GoodsDescrp | Varchar(3000) | SegmentGroup3.FTX+AAA -> C108.4440 | M | |
| Detail.CargoStatus | Varchar(4) | SegmentGroup3.FTX+CCI -> C108.4440 | M | |
| Detail.FPOD | Varchar(100) | SegmentGroup3.LOC+88 -> C519.3223 | M | |
| Detail.TempMin | float | SegmentGroup3.RNG+5 -> C280.6162 | M | |
| Detail.TempOpt | float | SegmentGroup3.RNG+5 -> C280.6162 | M | |
| Detail.TempMax | float | SegmentGroup3.RNG+5 -> C280.6152 | M | |
| Detail.IMCO | Varchar(20) | SegmentGroup3.DGS+IMD -> C205.8351 | M | |
| Detail.UN | int | SegmentGroup3.DGS+IMD -> C234.7124 | M | |
| Detail.FP | float | SegmentGroup3.DGS+IMD -> C223.7106 | O | |
| Detail.OLA | float | SegmentGroup3.DIM+6 -> C221.6168 | M | |
| Detail.OLF | float | SegmentGroup3.DIM+6 -> C221.6168 | M | |
| Detail.OOH | float | SegmentGroup3.DIM+9 -> C221.6008 | M | |
| Detail.OWP | float | SegmentGroup3.DIM+8 -> C221.6140 | M | |
| Detail.OWS | float | SegmentGroup3.DIM+7 -> C221.6140 | M | |

## ⭐ Sample Test Inputs

| Sample Number | Vessel | Voyage | Port | Call Seq | MLO | Remarks |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | AISOP | 001N | BLW | 1 | ACL |  |
| 2 | AISOP | 001N | IDBLW | 1 | ACL |  |
| 3 | AMERD | 1803 | NLRTM | 1 | SAMB |  |
| 4 | AMERD | 1803 | NLRTM | 1 | UNFR |  |
| 5 | ATCOM | 18144 | NLRTM | 1 | HL |  |
| 6 | ATCOM | 18144 | NLRTM | 1 | LT |  |
| 7 | CNBAY | 19004 | NLRTM | 1 | MANN |  |
| 8 | CNBAY | 19004 | NLRTM | 1 | SAM |  |
| 9 | CNGUL | 1701 | DEHAM | 1 | CEYL |  |
| 10 | CNGUL | 1813 | FIOUL | 1 | HL |  |
| 11 | GETRD | 18127 | NLRTM | 1 | CMA |  |
| 12 | GETRD | 18127 | NLRTM | 1 | CMA01 |  |
| 13 | HANNI | 1747 | SEAHU | 1 | HSLA |  |
| 14 | HRLSJ | 1755 | NLRTM | 1 | UNFR |  |
| 15 | HYGEN | 574E | MYPKG | 1 | ACL |  |
| 16 | HYGEN | 574E | MYPKG | 1 | AET |  |
| 17 | HYGEN | 574E | MYPKG | 1 | CCN |  |
| 18 | HYHOP | 025E | SGSIN | 1 | PIL |  |
| 19 | HZSCH | 1807 | SESOE | 1 | HSLA |  |
| 20 | HZSCH | 1808 | SESOE | 1 | HSLA |  |
| 21 | HZSCH | 1809 | SESOE | 1 | HSLA |  |
| 22 | ICRUN | 1702 | DKAAL | 1 | ACL |  |
| 23 | ICRUN | 1702 | DKAAL | 1 | ACNL |  |
| 24 | ICRUN | 1812 | NLRTM | 1 | K |  |
| 25 | ICRUN | 1812 | NLRTM | 1 | ACL |  |
| 26 | ICRUN | 1812 | NLRTM | 1 | WAN |  |
| 27 | ISEXP | 18137 | FIOUL | 1 | CMA |  |
| 28 | ISEXP | 18137 | FIOUL | 1 | HL |  |
| 29 | ISEXP | 18137 | FIOUL | 1 | HSL |  |
| 30 | ISEXP | 18137 | FIOUL | 1 | MSK |  |
| 31 | MUSIC | 400 | NLRTM | 1 | UNFR |  |
| 32 | MUSIC | 501 | NLRTM | 1 | SAMB |  |
| 33 | MUSIC | 501 | NLRTM | 1 | UNFR |  |
| 34 | NDPHP | 18134 | FIOUL | 1 | CMA |  |
| 35 | NDPHP | 18134 | FIOUL | 1 | HL |  |
| 36 | NDPHP | 18134 | FIOUL | 1 | HSL |  |
| 37 | NDPHP | 18134 | FIOUL | 1 | MSK |  |
| 38 | SPRIT | 19038 | NLRTM | 1 | APCC |  |
| 39 | SPRIT | 19038 | NLRTM | 1 | CMA |  |
| 40 | SPRIT | 19038 | NLRTM | 1 | EVER |  |
| 41 | SPRIT | 19038 | NLRTM | 1 | EVMS |  |
| 42 | SPRIT | 19038 | NLRTM | 1 | HTSU |  |
| 43 | TEST | 004W | FIOUL | 1 | NYK |  |
| 44 | TEST | 004W | FIOUL | 1 | OOCL |  |
| 45 | TEST | 004W | FIOUL | 1 | YML |  |
| 46 | TEST | 1012 | SGSIN | 1 | ACL |  |
| 47 | TEST | 1012 | SGSIN | 1 | CCN |  |
| 48 | TEST | 1012 | SGSIN | 1 | CCSA |  |
| 49 | TEST | 1012 | SGSIN | 1 | CEYL |  |
| 50 | TEST | 1012 | SGSIN | 1 | CPI |  |
| 51 | TEST | 1012 | SGSIN | 1 | CRES |  |
| 52 | TEST | 1012 | SGSIN | 1 | DYLG |  |
| 53 | TEST | 1012 | SGSIN | 1 | SAF |  |
| 54 | TEST | 1012 | SGSIN | 1 | SCGL |  |
| 55 | TEST | 16001 | FIOUL | 1 | CMA |  |
| 56 | TEST | 16001 | FIOUL | 1 | CMA01 |  |
| 57 | TEST | 16001 | FIOUL | 1 | HAM02 |  |
| 58 | TEST | 16001 | FIOUL | 1 | HAP01 |  |
| 59 | TEST | 16001 | FIOUL | 1 | HL |  |
| 60 | TEST | 16001 | FIOUL | 1 | HSL |  |
| 61 | THEAS | 022W | SGSIN | 1 | ACL |  |
| 62 | THEAS | 022W | SGSIN | 1 | SAF |  |
| 63 | WESCA | 19010 | NLRTM | 1 | APCO |  |
| 64 | XPB | 96 | INNML | 1 | ACL |  |
| 65 | XXXX | 1234 | GBSOU | 1 | IDCL |  |
