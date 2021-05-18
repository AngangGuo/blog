---
title: "Notes - Sqlite Database"
date: 2021-01-13T16:55:05-08:00
categories:
- Tech
- Database
tags:
- SQLite
- CSV 
draft: false
---

## Commands
### Get unique values
```sqlite
SELECT DISTINCT report_date FROM daily;
```
### Like and Escape
* `%`: zero or more characters
* `_`: one character

```sqlite
-- Labels end with 30126
SELECT Label from RL WHERE Label LIKE '%30126';

-- Contains 10%
SELECT Label from RL WHERE Label LIKE '%10\%%' ESCAPE '\';
```
### How many records?
```sqlite
SELECT count(*) from vanall WHERE Shipped_Date="2021-01-12";
```

### Last Shipped Date
```sqlite
-- Current max date in the table
SELECT max(Shipped_Date) from vanall;
```

### Insert Or Update
```sqlite
INSERT or replace INTO daily (date,name_id,testing_hour,sellable,liquidation,quality,concession) 
VALUES ("2021-02-01",5,1.0,2,3,4,5);
```

### Insert data get from other tables
```sqlite
-- insert new data from temp into table vanall
INSERT INTO vanall
SELECT * from temp where Shipped_Date>"2021-01-12";
```

### Update
```sqlite
UPDATE associate SET show=1 WHERE show IS NULL
```

### Delete
```sqlite
-- remove all the assets that are not shipped yet
DELETE FROM vanall WHERE Shipped_Date is NULL
```

### Creating Sub-Query
```sqlite
with yvr3(facility,class,cat, sub) as
(select "YVR3", class,cat, count(*) as sub from amazon where location < "QC-76.11"
group by class,cat),

yyc1(facility, class,cat,sub) as 
(select "YYC1", class,cat,count(*) as sub from amazon where location > "QC-76.10"
group by class,cat)

select yyc1.facility,yyc1.class,yyc1.cat, yyc1.sub from yyc1
left join yvr3
on (yyc1.class=yvr3.class and yyc1.cat=yvr3.cat)
where yvr3.cat is null
order by yyc1.sub desc;
```

### FULL OUT JOIN 
Sqlite doesn't support `FULL OUT JOIN` or `RIGHT JOIN`.
It’s easy to perform a RIGHT OUTER JOIN in SQLite by simply reversing the order of tables and using a LEFT OUTER JOIN. 
It’s also possible to do a FULL OUTER JOIN by combining LEFT OUTER JOINs using the UNION keyword.

See {{< ref "#fulloutjoin" >}} on how to emulate full out join in Sqlite.

### Sqlite: Get table information
```sqlite
PRAGMA table_info(vancouver)
```

## Work
### How to get Liquidation Class & Category Report
* Download the latest Inventory All Fields file from [Egnyte](https://cloudblue.egnyte.com/#username)
* Import the CSV data into a temp database(`RL`). See {{< ref "#csv" >}} 
* Execute the following command to get the report data.

```sqlite
SELECT Asset_Tag, Class, Category, Mfg, Model_Number,
    Repair_Disposition, Final_Functional_Status, 
    Audit_Final_Notes, Total_Fees, BER_Threshold_Percentage, 
    BER_Threshold, BER_Threshold_Remaining 
FROM RL
WHERE Shipped_Date >= "2021-03-07" AND Shipped_Date <= "2021-03-13" AND Last_FG_Site = "FG(RL-LIQ)"
```

* Copy all the data from DB Browser and Paste the data into "Weekly-Stats-Template.xlsx"
* Save it as `20210316 - RL Liquidation Report.xlsx` 
* Remove the two yellow columns and save it as `20210316 - Amazon Liquidation Report.xlsx`
* Attach these two files and send the report

### How to get the associates' daily/weekly testing details report? {#fulloutjoin}
To Get the Sellable(including Fraud) and Liquidation assets details for each associate.
```sqlite
WITH a AS(
SELECT Auditor, count(*) AS sellable
FROM RL
WHERE Date_put_to_FG_Site >= "2021-03-07" AND Date_put_to_FG_Site <= "2021-03-13" AND (Repair_Disposition = "COMP" OR Repair_Disposition = "FRAUD")
GROUP BY Auditor),

b AS (
SELECT Auditor, count(*) AS liq
FROM RL
WHERE Date_put_to_FG_Site >= "2021-03-07" AND Date_put_to_FG_Site <= "2021-03-13" AND (Repair_Disposition = "LIQC" OR Repair_Disposition = "LIQF")
GROUP BY Auditor)

SELECT a.Auditor AS auditor, a.sellable, b.liq FROM a
LEFT JOIN b ON a.Auditor = b.Auditor

UNION ALL

SELECT b.Auditor AS auditor, a.sellable, b.liq FROM b
LEFT JOIN a ON a.Auditor = b.Auditor
WHERE a.Auditor IS NULL

ORDER BY auditor
```

## Import CSV file Into Sqlite Database {#csv}
We have inventory all fields file which is in CSV format. 
There're several methods to load these data into database for further processing.

### Import CSV file from command line
Download and install [SQLite](https://www.sqlite.org/download.html).
From command line execute the following commands to import your CSV file into a table:

Note: All the columns' data types are `TEXT` 
```
C:\Sqlite> sqlite
sqlite> 
sqlite> .mode csv
sqlite> .import rl.csv assets
sqlite> .schema assets

CREATE TABLE IF NOT EXISTS "assets"(
"Facility" TEXT,
"Source" TEXT,
"Source_Location" TEXT,
...
);

sqlite> .save assets.db
sqlite> .quit

```

### Import Data Using DB Browser
[DB Browser for SQLite](https://sqlitebrowser.org/) (DB4S) is a high quality, visual, open source tool to create, design, 
and edit database files compatible with SQLite.

To import CSV data using DB4S, select `File > Import > Table From CSV File...`

![Menu - Import From CSV File...](/images/2021/db-browser-import-table-from-csv-file.PNG)

Import Options:
* Table name: assets
* Column names in first line: Yes, the first line can be used as column names
* Disable data type detection: Leave it unchecked so that DB4S can detect the column types
* Click OK to import it

![Import Options](/images/2021/db-browser-import-csv-options.PNG)

DB4S can detect the column data types: TEXT, INTEGER, REAL, etc.

Tips: 
* You can use `PRAGMA table_info(table_name)` to get all the column names of a table.
* You can get the table create statement from Database Structure tab


## Reference
* Create table statement as of 2021-01-18
```
CREATE TABLE "assets" (
"Facility"	TEXT,
"Source"	TEXT,
"Source_Location"	TEXT,
"Job_Number"	TEXT,
"Job_Type"	TEXT,
"OwnerShip"	TEXT,
"Label"	TEXT,
"Asset_Tag"	TEXT,
"Amazon_ASIN_Number"	TEXT,
"Load_ID"	TEXT,
"Class"	TEXT,
"Category"	TEXT,
"Mfg"	TEXT,
"Mfg_Part_Number"	TEXT,
"Model_Number"	TEXT,
"Serial_Number"	TEXT,
"Customer_Return_Reasons_Comments"	TEXT,
"CustomerReturnComments"	TEXT,
"Manifest_Upload_Date"	TEXT,
"Received_Date"	TEXT,
"Date_Sorted"	TEXT,
"Production_Date"	TEXT,
"Repair_Date"	TEXT,
"Date_put_to_FG_Site"	TEXT,
"Shipped_Date"	TEXT,
"Site"	TEXT,
"Inventory_Status"	TEXT,
"Location"	TEXT,
"COGS"	INTEGER,
"Repair_Disposition"	TEXT,
"Repair_Status"	TEXT,
"Defect_Code"	TEXT,
"Repair_Code"	TEXT,
"Duration_Mins"	INTEGER,
"Technician"	TEXT,
"Warranty"	TEXT,
"Warranty_Type"	TEXT,
"Pallet_ID"	TEXT,
"Model_Desc"	TEXT,
"Short_Desc"	TEXT,
"Audit_Functional_Status"	TEXT,
"Final_Functional_Status"	TEXT,
"Audit_Grade"	TEXT,
"Final_Grade"	TEXT,
"Failure"	TEXT,
"Include"	TEXT,
"Cosmetic"	TEXT,
"Missing"	TEXT,
"Audit_Notes"	TEXT,
"Audit_Final_Notes"	TEXT,
"Orig_Box"	TEXT,
"Orig_Packing"	TEXT,
"UDF1"	TEXT,
"UDF2"	TEXT,
"UDF3"	TEXT,
"Auditor"	TEXT,
"Date_Audited"	TEXT,
"Repair_Notes"	TEXT,
"QC_Completed"	TEXT,
"Asset_Status"	TEXT,
"Last_Updated_By"	TEXT,
"Last_Updated_Date"	TEXT,
"Customer_Inventory_Status"	TEXT,
"SO_Number"	INTEGER,
"Sold_to_Customer"	TEXT,
"Vendor_Name"	TEXT,
"Return_Condition"	TEXT,
"FREIGHT"	REAL,
"Receiving_Labor_Duration_Mins"	INTEGER,
"Receiving_Labor_Fees"	INTEGER,
"Sort_Labor_Duration_Mins"	INTEGER,
"Sort_Labor_Fees"	INTEGER,
"Sort_Screening_Fixed_Fees"	REAL,
"Production_Labor_Duration_Mins"	INTEGER,
"Production_Labor_Fees"	INTEGER,
"Production_Functional_test_Fixed_Fees"	INTEGER,
"Parts_Cost"	INTEGER,
"Repair_with_Parts_Labor_Duration_Mins"	INTEGER,
"Repair_with_Parts_Labor_Fees"	INTEGER,
"Repair_without_Parts_Labor_Duration_Mins"	INTEGER,
"Repair_without_Parts_Labor_Fees"	INTEGER,
"Data_Erasure_Labor_Duration_Mins"	INTEGER,
"Data_Erasure_Labor_Fees"	INTEGER,
"Data_Erasure_Memory_wipe_Fixed_Fees"	INTEGER,
"Imaging_Labor_Duration_Mins"	INTEGER,
"Imaging_Labor_Fees"	INTEGER,
"Imaging_Software_Reload_Fixed_Fees"	INTEGER,
"Kitting_Duration_Mins"	INTEGER,
"Kitting_Labor_Fees"	INTEGER,
"Kitting_Fixed_Fees"	INTEGER,
"Deep_Clean_Sanitization_Duration_Mins"	INTEGER,
"Deep_Clean_Sanitization_Labor_Fees"	INTEGER,
"Deep_Clean_Sanitization_Fixed_Fees"	INTEGER,
"Clean_and_Pack_Duration_Mins"	INTEGER,
"Clean_And_Pack_Labor_Fees"	INTEGER,
"Clean_And_Pack_Fixed_Fees"	INTEGER,
"Special_Handling"	INTEGER,
"UI_Grading"	INTEGER,
"Exception_Handling"	REAL,
"Rebox_Labor_Duration_Mins"	INTEGER,
"Rebox_Labor_Fees"	INTEGER,
"Rebox_Fixed_Fees"	INTEGER,
"OTV_Vendor_Parts_Charges"	INTEGER,
"OTV_Vendor_Labor_Charges"	INTEGER,
"OTV_Vendor_Parts_Redeployment_Charges"	INTEGER,
"OTV_Processing_Ingram_admin_Fees"	INTEGER,
"OTV_Ship_To_Vendor_Cost"	INTEGER,
"OTV_Vendor_Shipping_Cost"	INTEGER,
"OTV_Vendor_Name"	TEXT,
"OTV_Asset_Status"	TEXT,
"IM_Original_SKU"	TEXT,
"IM_New_SKU"	TEXT,
"Total_Duration"	INTEGER,
"Total_Fees"	REAL,
"RTV_Processing"	INTEGER,
"Job_Currency"	TEXT,
"Went_To_Sort"	INTEGER,
"Went_To_Production"	INTEGER,
"Went_To_Repair"	INTEGER,
"Went_To_Data_Erasure"	INTEGER,
"Went_To_Imaging"	INTEGER,
"Went_To_Kitting"	INTEGER,
"Went_To_Clean_and_Pack"	INTEGER,
"Went_To_Deep_Clean"	INTEGER,
"Went_To_Rebox"	INTEGER,
"Went_To_FG_RTV"	INTEGER,
"Went_To_FG_RL"	INTEGER,
"Purchase_Cost"	TEXT,
"Warranty_Parts"	INTEGER,
"Warranty_Labor"	INTEGER,
"Settlement_Date"	TEXT,
"Current_Site_Moved_Date"	TEXT,
"Current_Repair_Status_Moved_Date"	TEXT,
"Sellable_QC_Audit_Primary_Inspection_UDF1"	TEXT,
"UDF1_Selection_Date"	TEXT,
"UDF1_Created_By"	TEXT,
"Sellable_QC_Audit_Secondary_Failure_UDF2"	TEXT,
"UDF2_Selection_Date"	TEXT,
"UDF2_Created_By"	TEXT,
"Sellable_QC_Audit_Third_Failure_UDF3"	TEXT,
"UDF3_Selection_Date"	TEXT,
"UDF3_Created_By"	TEXT,
"Sellable_QC_Audit_Fourth_Failure_UDF4"	TEXT,
"UDF4_Selection_Date_Time"	TEXT,
"UDF4_Created_By"	TEXT,
"Liquidation_QC_Audit_UDF5"	TEXT,
"UDF5_Selection_Date"	TEXT,
"UDF5_Created_By"	TEXT,
"QC_Notes_UDF6"	TEXT,
"UDF6_Selection_Date"	TEXT,
"UDF6_Created_By"	TEXT,
"Total_Number_Of_Assets_Associated_With_Each_PO"	TEXT,
"UPC_Number"	TEXT,
"STAT_Receiving_SLA"	INTEGER,
"RTAT_Repair_SLA"	INTEGER,
"TTAT_Total_Turn_Around_Time_SLA"	INTEGER,
"BER_Threshold_Percentage"	INTEGER,
"BER_Threshold"	REAL,
"BER_Threshold_Remaining"	REAL,
"Last_FG_Site"	TEXT,
"Retest_Requested"	TEXT,
"Sort_TAT"	INTEGER,
"FG_TAT"	INTEGER
)
```