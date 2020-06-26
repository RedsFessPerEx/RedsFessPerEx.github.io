[Scenarios Index](iPaaS-PoC-Scenarios)

# Push Invoices

## Purpose
(Scheduled) Posting of customer invoices into SAP

## Input
* Invoice Start Date
* Invoice End Date

## Output
* SAP Response []
  - SAP Response

## Mechanism
* Connect to MSSQL
* Execute SQL
* Map SQL results into _SAP Posting Document(s)_
  - Each set of _header.pKey_ results map into one _SAP Posting Document_ ([schema](Samples/002/SAP%20posting.xsd))([sample](Samples/002/JV_773702_20200603050031_1.xml))
* Post into SAP
* Map [SAP response](Samples/002/20200603053600_Success_MessageID%20773702)(s) into SAP Response []

## Supporting Information

### Input _Schema_

| Item | Type | Type Details | Reference Name | Remarks |
| --- | --- | --- | --- | --- |
| Invoice Start Date | string | ISO 8601 | @DateFrom | |
| Invoice End Date | string | ISO 8601 | @DateTo | |


### Output _Schema (SAP Response)_

| Item | Type | Type Details | Remarks |
| --- | --- | --- | --- |
| SAP Response | string | | Verbatim response received from SAP |


### MSSQL Connection Details

| Item | Value | Remarks |
| --- | --- | --- |


### SELECT Details

#### SELECT

| Item | Type | Type Details | Remarks |
| --- | --- | --- | --- |
| Detail.ACCNT_CODE | | | |
| Detail.ACTUALTEU | | | |
| Detail.ACTUALWT | | | |
| Detail.ASSIGNMENTNO | | | |
| Detail.BUNKERQTY | | | |
| Detail.CHARGETYPE | | | |
| Detail.COMMITTEDTEU | | | |
| Detail.COMMODITY | | | |
| Detail.COMMODITYTYPE | | | |
| Detail.CONTAINERTYPE | | | |
| Detail.CONTRSIZE | | | |
| Detail.COSTCENTER | | | |
| Detail.CUSTOMERSUBCODE | | | |
| Detail.D_C | | | |
| Detail.DESCRIPTN | | | |
| Detail.ELIGIBILITYMARKER | | | |
| Detail.EXPTYPE | | | |
| Detail.FRTTYPE | | | |
| Detail.GL_INDICATOR | | | |
| Detail.INTERCO | | | |
| Detail.JVPARTNER | | | |
| Detail.LEG | | | |
| Detail.PAYMENT_BLOCK | | | |
| Detail.POD | | | |
| Detail.POL | | | |
| Detail.PORT | | | |
| Detail.POD | | | |
| Detail.POL | | | |
| Detail.PROFIT_CTR | | | |
| Detail.REP_AMT | | | |
| Detail.REVTEU | | | |
| Detail.SECTOR | | | |
| Detail.SERVICE | | | |
| Detail.SHPTERML | | | |
| Detail.SHTERMD | | | |
| Detail.STAFFNAME | | | |
| Detail.TAX_BASEAMOUNT | | | |
| Detail.TAX_CODE | | | |
| Detail.TBL | | | |
| Detail.TERMINAL | | | |
| Detail.TPCODE | | | |
| Detail.TRANSAMOUNT | | | |
| Detail.TRANSCURRENCY | | | |
| Detail.TRANSTYPE | | | |
| Detail.VENDORCODE | | | |
| Detail.VESSEL | | | |
| Detail.Vessel+detail.POL+detail.POD+detail.Voyage  | | | |
| Detail.VIAPORT | | | |
| Detail.VOYAGE | | | |
| Header.BASEL_DATETIME | | | |
| Header.BUSINESSUNIT | | | |
| Header.BUSINESSUNIT | | | |
| Header.JRNAL_TYPE | | | |
| Header.pKey | | | |
| Header.PSTNG_DATE | | | |
| Header.TRANSDATETIME | | | |
| Header.TRANSNO | | | |

#### FROM / WHERE

| Item | Details | Remarks |
| --- | --- | --- |
| FROM | vg_ts_SAPLinkHeader Header | |
| INNER JOIN   | vg_ts_SAPLinkDetail Detail on Header.pkey=detail.ts_SAPLinkHeaderpkey  |   |
| WHERE | Header.AuditStat = 1 | |
| WHERE | detail.AuditStat=1 | |
| WHERE | IsLocked = 0 | |
| WHERE | JRNAL_TYPE in ('I1', 'I2') | |
| WHERE | PSTNG_DATE between @DateFrom and @DateTo | |
| WHERE | POSTSTATUS in ( 'N', 'F') | |
| ORDER BY | Header.CreatedDateTime | |


### ⭐ SAP Connection Details

| Item | Details | Remarks |
| --- | --- | --- |
| SAP Version | S/4 HANA | |
| Login | IPAAS |   |
| Password   |  IPAAs12345$ |   |
| Client | 300 |   |
| Language  | EN  |   |
| Application Server / Host   | 192.168.9.55  |   |
| System Name   | XSD  |   |
| System Number / Instance  | 00  |   |

If the above are insufficient, please let us know what other SAP connection information you require in order to configure your SAP Connector.

### SAP Mapping

| SAP | Value | Remarks |
| --- | --- | --- |
| MessageID  | Header.pKey | |
| DocType  | Header.JRNAL_TYPE | |
| CompCodeHeader  | Header.BUSINESSUNIT | |
| HeaderTxt | Detail.Vessel+Detail.POL+Detail.POD+Detail.Voyage | |
| DocDate | Header.TRANSDATETIME | |
| PstngDate | Header.PSTNG_DATE | |
| Currency  | Detail.TRANSCURRENCY | |
| TransDate |  | |
| RefDocNo  | Header.TRANSNO | |
| CompCodeItem  | Header.BUSINESSUNIT | |
| BlineDate  | Header.BASEL_DATETIME | |
| Indic  | Detail.D_C | |
| GlAccount  | Detail.ACCNT_CODE | |
| AmtDoccur  | Detail.TRANSAMOUNT | |
| AmtLoccur  |  | |
| AmtGrpcur  |  | |
| AmtHrdcur |  | |
| VendorNo  | Detail.VENDORCODE | |
| Customer  | Detail.CUSTOMERSUBCODE | |
| TradeId  | Detail.TPCODE | |
| CrossCompCode  | Detail.INTERCO | |
| TaxCode  | Detail.TAX_CODE | |
| AmtBase | Detail.TAX_BASEAMOUNT | |
| TaxAmt | Detail.REP_AMT | |
| ProfitCtr  | Detail.PROFIT_CTR | |
| Costcenter  | Detail.COSTCENTER | |
| AllocNmbr  | Detail.ASSIGNMENTNO | |
| ItemText  | Detail.DESCRIPTN | |
| Pmnttrms  |  | |
| PymtMeth  |  | |
| PmntBlock  | Detail.PAYMENT_BLOCK | |
| ValueDate  |  | |
| ReconAcc  |  | |
| RefKey1  | Detail.EXPTYPE | |
| RefKey2  | Detail.STAFFNAME | |
| RefKey3  | Detail.ELIGIBILITYMARKER | |
| Region  |  | |
| Hub  |  | |
| SubHub  |  | |
| Service  | Detail.SERVICE | |
| Vessel  | Detail.VESSEL | |
| Voyage  | Detail.VOYAGE | |
| Leg  | Detail.LEG | |
| Sector  | Detail.SECTOR | |
| CustomerGroup  |  | |
| CustomerNo  |  | |
| JvPartner  | Detail.JVPARTNER | |
| FreightType  | Detail.FRTTYPE | |
| ChargeType  | Detail.CHARGETYPE | |
| ContainerType  | Detail.CONTAINERTYPE | |
| ContainerSize  | Detail.CONTRSIZE | |
| Commodity  | Detail.COMMODITY | |
| CommodityType  | Detail.COMMODITYTYPE | |
| CommittedTeu  | Detail.COMMITTEDTEU | |
| ActualTeu  | Detail.ACTUALTEU | |
| RevenueTeu  | Detail.REVTEU | |
| ActualWeight  | Detail.ACTUALWT | |
| BunkerQuantity  | Detail.BUNKERQTY | |
| Port  | Detail.PORT | |
| PortOfLoading  | Detail.POL | |
| PortOfDischarge  | Detail.POD | |
| Terminal  | Detail.TERMINAL | |
| TerminalOfLoading  | Detail.POL | |
| TerminalOfDischarge  | Detail.POD | |
| ThroughBl  | Detail.TBL | |
| TranshipmentPort  | Detail.VIAPORT | |
| ShipmentTermLoad  | Detail.SHPTERML | |
| ShipmentTermDischarge  | Detail.SHTERMD | |
| SpGlInd  | Detail.GL_INDICATOR | |
| Pyamt |  | |
| Pycur  |  | |
| TransType | Detail.TRANSTYPE | |
