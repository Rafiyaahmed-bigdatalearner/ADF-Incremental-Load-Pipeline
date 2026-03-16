# Azure Data Factory Incremental Load Pipeline

## Project Overview
This project demonstrates a **dynamic incremental data load pipeline** built using **Azure Data Factory (ADF)**. It moves data from a **source Azure SQL table** to a **sink Azure SQL table**, while copying only **new or updated records** based on the `inserttime` column.  

The pipeline is **parameterized and reusable**, making it ideal for showcasing data engineering skills in a portfolio or interview scenario.

---

## Key Features
- **Incremental Load:** Copies only new or updated records using a maximum `inserttime` reference from the sink table.  
- **Dynamic Configuration:** Pipeline, datasets, and queries use parameters from `parameters.json`.  
- **Data Quality & Validation:** Ensures non-nullable columns are filled and avoids insert errors.  
- **Scalable Design:** Supports backdated records and large datasets.  
- **Version Control Ready:** All ADF artifacts exported to JSON and ready for GitHub.

---

## Folder Structure
ADF-Incremental-Pipeline/
├─ Pipelines/
│ └─ IncrementalLoad.json ← ADF pipeline with Lookup and Copy activities
├─ Datasets/
│ ├─ OrdersSource.json ← Source dataset
│ └─ OrdersSink.json ← Sink dataset
├─ LinkedServices/
│ └─ AzureSQLDatabase.json ← Linked service template (placeholders for credentials)
├─ SampleData/
│ └─ insert_sample_data.sql ← Sample data to populate source table
├─ parameters.json ← Pipeline parameters (tables, last run date, etc.)
└─ README.md

---

## Prerequisites
1. Azure subscription with **Azure Data Factory** and **Azure SQL Database**.  
2. Tables `orders` (source) and `Orders_Sink` (sink) in Azure SQL Database.  
3. GitHub account (optional for version control).  

---

## Setup Instructions

1. **Deploy Linked Services**  
   - Use `LinkedServices/AzureSQLDatabase.json` as a template.  
   - Replace placeholders (`<your-server-name>`, `<your-username>`, `<your-password>`) with your own credentials.  

2. **Deploy Datasets**  
   - Import `Datasets/OrdersSource.json` and `Datasets/OrdersSink.json` into ADF.  
   - Ensure `linkedServiceName.referenceName` points to your linked service.

3. **Create Pipeline**  
   - Import `Pipelines/IncrementalLoad.json` into ADF.  
   - Map pipeline parameters to your datasets and activities.  

4. **Configure Parameters**  
   - Edit `parameters.json` to define:  
     - `SourceTable`  
     - `SinkTable`  
     - `LastRunDate` (for incremental load)  
     - `SourceDatabase`, `SinkDatabase`, `ServerName`  

5. **Insert Sample Data**  
   - Run `SampleData/insert_sample_data.sql` in your source database to populate initial records.  

6. **Run the Pipeline**  
   - Execute the pipeline in ADF.  
   - Verify that only records **newer than `LastRunDate`** are copied to the sink.  

---

## Sample SQL for Testing

```sql
INSERT INTO dbo.orders (order_id, FirstName, LastName, inserttime)
VALUES
(1, 'manish', 'tiwari', '2026-03-15T08:17:02.983'),
(2, 'rani', 'sharma', '2026-03-15T08:17:02.987'),
(3, 'yuvraj', 'verma', '2026-03-15T08:17:02.990'),
(4, 'hema', 'kumar', '2026-03-15T15:03:08.327'),
(5, 'Alice', 'singh', '2026-03-16T08:23:14.100');

How to Extend

Add additional tables by creating new source/sink datasets.

Modify the pipeline to handle multiple tables in parallel.

Parameterize additional properties like schema name or incremental column.

Skills Demonstrated

Azure Data Factory: Pipeline, Lookup, Copy Activity

Incremental ETL & Data Engineering Design

SQL & Azure SQL Database operations

Parameterization & Dynamic Content in ADF

Version control and GitHub-ready project structure

Repository Goal

This project serves as a portfolio-ready example for:

Data Engineers with 3–5 years experience

Showcasing ETL pipeline design, incremental load strategy, and Azure skills

Demonstrating parameterized, reusable ADF pipelines
