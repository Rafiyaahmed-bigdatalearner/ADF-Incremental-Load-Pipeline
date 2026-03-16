# Azure Data Factory Incremental Load Pipeline

## Project Overview
This project demonstrates a **dynamic incremental data load pipeline** built using **Azure Data Factory (ADF)**.  
It moves data from a **source Azure SQL table** to a **sink Azure SQL table**, while copying only **new or updated records** based on the `inserttime` column.

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
│ └─ IncrementalLoad.json
├─ Datasets/
│ ├─ OrdersSource.json
│ └─ OrdersSink.json
├─ LinkedServices/
│ └─ AzureSQLDatabase.json
├─ SampleData/
│ └─ insert_sample_data.sql
├─ parameters.json
└─ README.md

---

## Prerequisites
1. Azure subscription with **Azure Data Factory** and **Azure SQL Database**.  
2. Tables `orders` (source) and `Orders_Sink` (sink) in Azure SQL Database.  
3. GitHub account (optional for version control).  

---

## Setup Instructions

### 1. Deploy Linked Services
- Use `LinkedServices/AzureSQLDatabase.json` as a template.  
- Replace placeholders (`<your-server-name>`, `<your-username>`, `<your-password>`) with your own credentials.  

### 2. Deploy Datasets
- Import `Datasets/OrdersSource.json` and `Datasets/OrdersSink.json` into ADF.  
- Ensure `linkedServiceName.referenceName` points to your linked service.

### 3. Create Pipeline
- Import `Pipelines/IncrementalLoad.json` into ADF.  
- Map pipeline parameters to your datasets and activities.  

### 4. Configure Parameters
- Edit `parameters.json` to define:  
  - `SourceTable`  
  - `SinkTable`  
  - `LastRunDate` (for incremental load)  
  - `SourceDatabase`, `SinkDatabase`, `ServerName`  

### 5. Insert Sample Data
- Run `SampleData/insert_sample_data.sql` in your source database to populate initial records.  

### 6. Run the Pipeline
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

---

## How to Extend
- **Add additional tables:** Create new source and sink datasets for each table.  
- **Handle multiple tables in parallel:** Modify the pipeline to copy multiple tables simultaneously.  
- **Parameterize additional properties:** For example, schema name, incremental column, or custom filters.  
- **Improve monitoring:** Add alerts or logging for pipeline failures or data quality checks.  

---

## Skills Demonstrated
- **Azure Data Factory:** Pipeline orchestration, Lookup, Copy Activity, parameterization.  
- **Incremental ETL & Data Engineering Design:** Designing pipelines for efficient, incremental data movement.  
- **SQL & Azure SQL Database operations:** Query optimization, data validation, and schema design.  
- **Parameterization & Dynamic Content in ADF:** Using parameters and dynamic expressions to make pipelines reusable.  
- **Version control & GitHub:** Exporting ADF artifacts as JSON and maintaining a portfolio-ready repository.
