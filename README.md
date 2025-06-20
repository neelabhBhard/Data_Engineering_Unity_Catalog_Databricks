# End-to-End Data Engineering Project with Azure Databricks and Unity Catalog

## Project Overview

This project demonstrates a comprehensive data engineering solution built on Azure, addressing common challenges in data management through automation, scalability, and proper governance. The pipeline transforms raw sales data from GitHub into analytics-ready business intelligence assets using modern data engineering practices.

The main problem I tackled was dealing with sales data stored in GitHub that required manual processing with no centralized governance. I needed to create an automated, scalable pipeline that could handle incremental loads and provide analytics-ready data for business reporting.

## Architecture



<img width="902" alt="Workflow Diagram" src="https://github.com/user-attachments/assets/8c55474f-e944-4734-aad5-3d72e154a172" />

## Technology Stack

- **Azure Data Factory** - Pipeline orchestration and incremental loading
- **Azure Databricks** - Distributed data processing and transformation  
- **ADLS Gen2** - Data lake storage with medallion architecture
- **Unity Catalog** - Data governance and cataloging
- **PySpark** - Data transformation logic
- **Azure SQL Database** - Watermark table management
- **Power BI** - Analytics and visualization

## Implementation Details

### Data Ingestion Layer
Built parameterized pipelines in Azure Data Factory using a watermark-based approach for incremental loading. This approach processes only new or changed records rather than full dataset refreshes. The pipeline queries the last successful load timestamp, identifies new records, copies the delta, and updates the watermark table for the next run.

### Storage Architecture
Implemented a medallion architecture on ADLS Gen2 with three distinct layers:

**Bronze Layer**: Raw data ingestion from source systems with minimal transformation
**Silver Layer**: Cleaned and validated data after quality checks and transformations
**Gold Layer**: Business-ready dimensional model optimized for analytics

### Processing and Transformation
Used Azure Databricks for distributed data processing. When processing large datasets, the work gets automatically partitioned across multiple cluster nodes running parallel operations. This approach efficiently handles enterprise-scale data volumes.

### Data Governance
Integrated Unity Catalog for centralized data governance. This creates a searchable catalog of all data assets with metadata, lineage tracking, and access controls. Instead of hunting through various systems for data, users can search and discover assets with ownership information and data quality metrics.

### Dimensional Modeling
Designed a star schema in the Gold layer with the following structure:

- **Fact_Sales** - Core business metrics and measures
- **Dim_Branch** - Store and location dimensional context
- **Dim_Dealer** - Sales representative information
- **Dim_Model** - Product model details
- **Dim_Date** - Time dimension for temporal analysis

Implemented Slowly Changing Dimension (SCD) Type 1 logic using MERGE operations in Databricks workflows to handle evolving business data.

## Key Features

### Incremental Loading Pattern
The watermark-based incremental loading pattern proved essential for production-scale pipelines. Only processing incremental changes makes the solution both scalable and cost-effective, dramatically reducing processing time and compute costs.

### Distributed Processing
Databricks handles large-scale transformations efficiently through automatic partitioning and parallel execution. This significantly improved performance compared to traditional single-node processing approaches.

### Data Governance and Cataloging
Unity Catalog's value extends beyond simple cataloging. It provides comprehensive data lineage tracking, automated metadata management, and unified security policies across cloud platforms.

## Project Structure

```
azure-data-factory/
├── pipelines/              # ADF pipeline definitions
├── datasets/               # Dataset configurations
└── linked-services/        # Connection configurations

databricks-notebooks/
├── bronze-to-silver/       # Data cleaning and validation
├── silver-to-gold/         # Dimensional modeling
└── utilities/              # Helper functions and utilities

sql-scripts/
├── watermark-tables/       # Watermark table creation scripts
└── stored-procedures/      # Watermark update procedures
```

## Results

The implementation delivered significant improvements across multiple dimensions:

**Performance**: Dramatically reduced processing times through incremental loading and distributed processing
**Governance**: Established a single source of truth for sales data with proper cataloging and access controls
**Scalability**: Created a foundation that can scale with growing data volumes and evolving business requirements
**Analytics**: Delivered an analytics-ready dimensional model for business reporting and insights

## Technical Learnings

Working on this project reinforced several important data engineering principles:

The watermark pattern is crucial for production pipelines. It ensures data consistency while minimizing processing overhead by only handling incremental changes.

Unity Catalog provides more value than just data cataloging. The automated lineage tracking and metadata management capabilities significantly improve data discoverability and governance.

Distributed processing in Databricks with automatic workload partitioning offers substantial performance improvements over traditional approaches, especially when dealing with large datasets.

The medallion architecture pattern provides a clear progression of data quality and enables multiple downstream use cases while maintaining data integrity.

## Getting Started

### Prerequisites
- Azure subscription with Data Factory, Databricks, and ADLS Gen2 enabled
- Unity Catalog configured in Databricks workspace
- Azure SQL Database for watermark management

### Setup
1. Clone this repository
2. Configure Azure resources according to the architecture
3. Deploy Data Factory pipelines and configure linked services
4. Set up Databricks environment with required notebooks
5. Configure Unity Catalog connections and permissions
6. Execute initial pipeline run and validate results

## Contributing

Feel free to open issues or submit pull requests if you have suggestions for improvements or find any bugs.

## Contact

**Neelabh Bhard**
- GitHub: [@neelabhBhard](https://github.com/neelabhBhard)
- LinkedIn: [Connect with me](https://linkedin.com/in/your-profile)
