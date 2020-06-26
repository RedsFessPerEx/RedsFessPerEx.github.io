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

<!-- | Item | Value | Remarks |
| --- | --- | --- | -->

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
