Formula 1 Data Engineering Project

An end-to-end data engineering learning project using Microsoft Azure, Azure Data Lake Storage Gen2 (ADLS Gen2), Azure Databricks, Apache Spark, PySpark, SQL, and Delta Lake to ingest, organize, transform, and analyze Formula 1 racing data.

The goal is to turn separate racing datasets into structured, analysis-ready tables that support comparisons of driver, constructor, and race performance.

Project Objectives
- Ingest Formula 1 datasets such as circuits, races, drivers, constructors, results, qualifying, pit stops, and lap times.
- Store source and transformed data in Azure Data Lake Storage Gen2.
- Use Databricks notebooks and PySpark to process data.
- Organize data into raw, processed, and presentation layers.
- Build analytical tables such as race results and driver standings.
- Use SQL to explore driver and constructor performance.

Architecture

Formula 1 Data Sources
(Ergast API reference / CSV / JSON files)
              |
              v
Azure Data Lake Storage Gen2
              |
              v
Azure Databricks + Apache Spark / PySpark
              |
              v
Raw Layer: f1_raw
              |
              v
Processed Layer: f1_processed
              |
              v
Presentation Layer: f1_presentation
              |
              v
SQL Analysis
(driver and constructor performance)

The project material documents these layers and workflows. Confirm the exact source-loading approach and successful end-to-end execution from the notebooks and your Azure environment.

__________________________________________________________________________________________________________________

Technologies Used
__________________________________________________________________________________________________________________

Technology	                        Purpose
Microsoft Azure	                    Cloud infrastructure
Azure Data Lake Storage Gen2	    Stores source and transformed data
Azure Databricks	                Notebook-based development and execution
Apache Spark / PySpark	            Reads, filters, joins, transforms, and aggregates data
SQL	                                Creates tables and analyzes racing performance
Delta Lake	                        Supports table storage and operations such as MERGE
Parquet	                            Columnar format for analytical data
CSV and JSON	                    Source data formats
Databricks utilities (dbutils)	    Notebook parameters, notebook execution, and secret retrieval
Ergast API / Formula 1 datasets	    Racing data source/reference

__________________________________________________________________________________________________________________

Data Layers
1. Raw — f1_raw
Contains source-oriented tables for datasets such as:
- circuits
- races
- constructors
- drivers
- results
- pit_stops
- lap_times
- qualifying
This layer makes source data queryable with Spark and SQL.

2. Processed — f1_processed
An intermediate layer used by downstream transformations. The notebooks reference processed paths and use formats including Parquet and Delta.

3. Presentation — f1_presentation
The analysis-ready layer. The documented notebooks include race-results output and driver-standings logic intended to make analytical queries easier.

These layers are conceptually similar to Bronze, Silver, and Gold, respectively. The project uses the names Raw, Processed, and Presentation.
Main Workflow and Notebook Areas

Notebook names below are based on the project material and may depend on the folder structure in your Databricks workspace.

1. Configure storage access
The setup material includes approaches named:
- access_adls_using_access_keys
- access_adls_using_cluster_scoped_credentials
A setup example also retrieves a storage credential through dbutils.secrets.get().

Purpose: configure Databricks to read from and write to Azure Data Lake Storage Gen2.

Security: Never commit storage keys, tokens, passwords, or other secrets to GitHub. Use Databricks secret scopes or an appropriate identity-based authentication method. If a credential has ever been committed, rotate it and remove it from repository history where appropriate.

2. Ingest source datasets
The ingestion notebooks shown include:
Notebook	Dataset
0.ingest_all_files	Orchestrates ingestion notebooks
1.ingest_circuits_file	Circuits
2.ingest_races_file	Races
3.ingest_constructors_file	Constructors / teams
4.ingest_drivers_file	Drivers
5.ingest_results_file	Race results
6.ingest_pit_stops_file	Pit stops
7.ingest_lap_times_file	Lap times


Qualifying data is also represented in the raw-table workflow.

Purpose: load Formula 1 datasets through notebook-based ingestion, using parameters such as source and file date where configured.

3. Create raw tables
The 1.create_raw_tables notebook creates the f1_raw database and tables for source datasets. The material demonstrates reading CSV and JSON data, including options for different JSON layouts.

Purpose: make source data queryable.

4. Transform data
The transformation notebooks include 1.race_results. The documented workflow reads datasets, renames columns, joins race, circuit, result, driver, and constructor information, adds metadata, and writes transformed output.

Purpose: combine related source datasets into a useful representation of race performance.

5. Build processed and presentation data
The notebooks reference f1_processed and f1_presentation. The presentation workflow includes race results and driver standings. The driver-standings logic groups by season and driver, calculates measures such as points and wins, and uses a window ranking function. A function named merge_delta_data is also shown in the material.

Purpose: prepare datasets for repeatable downstream analysis.

6. Analyze racing performance
The analysis notebooks shown include:
- 1.find_dominant_drivers — queries driver performance by year, including measures such as races, wins, and points.
- 2.find_dominant_teams — aggregates constructor/team performance.
A 1.filter_demo notebook demonstrates filtering races by year and round with a Spark DataFrame.

Purpose: use SQL and Spark to compare historical driver and constructor performance.

Example Questions
- Which drivers scored the most points in a season?
- Which drivers recorded the most wins?
- Which constructors performed best over a period?
- How can race, circuit, driver, constructor, and result data be combined?
- How can raw files be organized into reusable analytical tables?

Answers depend on the datasets loaded and the queries implemented in the notebooks.

Skills Demonstrated
- Cloud data storage and access configuration
- Notebook-based data ingestion
- CSV and JSON processing
- PySpark DataFrame operations
- Schema definition and column selection
- Joins, filters, and aggregations
- SQL table creation and analytical queries
- Parquet and Delta Lake storage
- Window functions and ranking
- Parameterized notebooks and notebook orchestration
- Secret management concepts
- Layered data-lake / lakehouse design

Getting Started

This project requires an Azure environment and a Databricks workspace with suitable permissions. Resource names, storage paths, secret-scope names, and authentication configuration may need to be adapted.

1. Clone or download this repository.
2. Review the notebook and folder structure.
3. Create or configure an Azure Storage account with ADLS Gen2 enabled.
4. Configure secure Databricks access to the storage account.
5. Import the notebooks into your Databricks workspace.
6. Update environment-specific paths and notebook parameters.
7. Verify required source files or API access are available.
8. Run setup and ingestion notebooks.
9. Create raw tables, then run transformations in dependency order.
10. Run presentation and analysis notebooks.
11. Validate row counts, schemas, and query results at each stage.

Confirm the exact execution order and required parameters from the notebook code and your Databricks workspace.

Conceptual Repository Structure
Adjust this example to match the files actually present in your GitHub repository.

.
├── README.md
├── setup/
│   ├── access_adls_using_access_keys
│   └── access_adls_using_cluster_scoped_credentials
├── ingestion/
│   ├── 0.ingest_all_files
│   ├── 1.ingest_circuits_file
│   ├── 2.ingest_races_file
│   ├── 3.ingest_constructors_file
│   ├── 4.ingest_drivers_file
│   ├── 5.ingest_results_file
│   ├── 6.ingest_pit_stops_file
│   └── 7.ingest_lap_times_file
├── raw/
│   └── 1.create_raw_tables
├── transformations/
│   ├── 1.filter_demo
│   └── 1.race_results
└── analysis/
    ├── 1.find_dominant_drivers
    └── 2.find_dominant_teams

Databricks notebooks may use different file extensions or folders in your actual repository.

Known Limitations / Validation Notes
- The project material shows some WorkflowException errors for notebook paths that could not be accessed during orchestration. Verify notebook paths and permissions before running 0.ingest_all_files.
- The available material does not confirm that every pipeline stage completed successfully end to end.
- A completed Power BI dashboard or machine-learning model is not established by the documented notebooks.
- Validate data completeness, duplicate handling, schemas, and analytical results before using outputs for reporting.
Future Improvements
- Add automated data-quality checks and row-count validation.
- Add structured logging and clearer failure handling.
- Parameterize environment-specific storage paths and source dates.
- Use managed identity or service-principal-based access where appropriate.
- Schedule and monitor ingestion and transformation jobs.
- Add tests for transformation logic and incremental loads.
- Add a Power BI dashboard if visualization is part of the intended scope.
Author / Learning Context

This project is a hands-on learning exercise in Azure data engineering, using Formula 1 data to practice cloud storage, Spark transformations, layered data organization, Delta Lake, and SQL analytics.