# Weather-Data-Pipeline-Orchestration-Using-Apache-Airflow-Self-Managed-and-AWS-Managed

#Overview

This project implements a data pipeline that collects weather data from the OpenWeather API. The data is processed, stored in multiple systems, and managed for efficient parallel processing and integration into a centralized data lake. The pipeline is orchestrated using AWS Managed Airflow, ensuring a fully managed and scalable solution for workflow automation.

#Project Goals
1) Automated Data Extraction: Fetch weather data from the OpenWeather API at regular intervals.
2) Parallel Processing: Efficiently transform and store data in an AWS RDS Postgres database and an Amazon S3 data lake simultaneously.
3) Centralized Storage: Combine processed data streams into a unified data lake for downstream analytics and reporting.
4) Workflow Orchestration: Use AWS Managed Airflow to automate, schedule, and monitor the pipeline without managing infrastructure.
5) Scalability: Design the pipeline to handle high volumes of data and support future enhancements with minimal operational overhead.


#Services Used
1) OpenWeather API:
	Data source for weather information.
	Provides historical weather data.
	
2) Amazon S3 (Data Lake):
	Used as a centralized storage repository for raw and processed data.
	Supports scalability for large datasets.
	
3) AWS RDS (Postgres):
	Stores processed data for structured queries.
	Provides relational database capabilities.
	
4) AWS Managed Airflow (Orchestrator):
	Fully managed Apache Airflow service for workflow orchestration.
	Eliminates the need to manage EC2 instances for Airflow.
	Automates scheduling, monitoring, and dependency resolution.
	
5) Python:
	Used for data transformation tasks (ETL process).