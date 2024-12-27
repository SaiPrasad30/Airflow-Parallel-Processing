# Weather-Data-Pipeline-Orchestration-Using-Apache-Airflow-Self-Managed-and-AWS-Managed

## Overview

This project implements a data pipeline that collects weather data from the OpenWeather API. The data is processed, stored in multiple systems, and managed for efficient parallel processing and integration into a centralized data lake. The pipeline is orchestrated using AWS Managed Airflow, ensuring a fully managed and scalable solution for workflow automation.

## Project Goals
1. **Automated Data Extraction**: Fetch weather data from the OpenWeather API at regular intervals.
2. **Parallel Processing**: Efficiently transform and store data in an AWS RDS Postgres database and an Amazon S3 data lake simultaneously.
3. **Centralized Storage**: Combine processed data streams into a unified data lake for downstream analytics and reporting.
4. **Workflow Orchestration**: Use AWS Managed Airflow to automate, schedule, and monitor the pipeline without managing infrastructure.
5. **Scalability**: Design the pipeline to handle high volumes of data and support future enhancements with minimal operational overhead.

## Services Used

1. **OpenWeather API**:
   - Data source for weather information.
   - Provides historical weather data.

2. **Amazon S3 (Data Lake)**:
   - Used as a centralized storage repository for raw and processed data.
   - Supports scalability for large datasets.

3. **AWS RDS (Postgres)**:
   - Stores processed data for structured queries.
   - Provides relational database capabilities.

4. **AWS Managed Airflow (Orchestrator)**:
   - Fully managed Apache Airflow service for workflow orchestration.
   - Eliminates the need to manage EC2 instances for Airflow.
   - Automates scheduling, monitoring, and dependency resolution.

5. **Python**:
   - Used for data transformation tasks (ETL process).

## Tables in AWS RDS PostgreSQL

As part of the pipeline implementation, two tables are created and managed within AWS RDS PostgreSQL. These tables facilitate structured storage and relational queries for weather and city data. The tables are joined to generate enriched insights, which are then loaded into the data lake.

### 1. `city_look_up` Table
   - **Purpose**: Stores metadata about cities, including state, population census, and land area.
   - **Columns**:
     - `city` (TEXT): Name of the city.
     - `state` (TEXT): State to which the city belongs.
     - `census_2020` (NUMERIC): Population census data from 2020.
     - `land_area_sq_mile_2020` (NUMERIC): Land area in square miles as of 2020.
   - **Data Ingestion**: The data is loaded into this table from a CSV file stored in Amazon S3 using the `aws_s3.table_import_from_s3` PostgreSQL function.

### 2. `weather_data` Table
   - **Purpose**: Stores weather data for cities, fetched from the OpenWeather API.
   - **Columns**:
     - `city` (TEXT): Name of the city.
     - `description` (TEXT): Weather description.
     - `temperature_farenheit` (NUMERIC): Current temperature in Fahrenheit.
     - `feels_like_farenheit` (NUMERIC): Temperature feels like in Fahrenheit.
     - `minimun_temp_farenheit` (NUMERIC): Minimum temperature in Fahrenheit.
     - `maximum_temp_farenheit` (NUMERIC): Maximum temperature in Fahrenheit.
     - `pressure` (NUMERIC): Atmospheric pressure.
     - `humidity` (NUMERIC): Humidity percentage.
     - `wind_speed` (NUMERIC): Wind speed in meters per second.
     - `time_of_record` (TIMESTAMP): Timestamp of the weather data record.
     - `sunrise_local_time` (TIMESTAMP): Local sunrise time.
     - `sunset_local_time` (TIMESTAMP): Local sunset time.

## Joining the Tables

To combine city metadata with weather data, the pipeline performs a SQL join operation between the `city_look_up` and `weather_data` tables based on the `city` column.

### Join Query
```sql
SELECT 
    w.city,                    
    description,
    temperature_farenheit,
    feels_like_farenheit,
    minimun_temp_farenheit,
    maximum_temp_farenheit,
    pressure,
    humidity,
    wind_speed,
    time_of_record,
    sunrise_local_time,
    sunset_local_time,
    state,
    census_2020,
    land_area_sq_mile_2020                    
FROM weather_data w
INNER JOIN city_look_up c ON w.city = c.city;
```

- **Purpose**: To enrich the weather data with additional contextual information such as the state, population census, and land area.
- **Output**: The joined data is transformed into a structured format and exported as a CSV file to the Amazon S3 data lake.

## Updated Workflow with AWS Managed Airflow

The pipeline is orchestrated using **AWS Managed Airflow**, which automates and schedules tasks such as:
1. **Creating and truncating tables**.
2. **Extracting weather data from the OpenWeather API**.
3. **Loading city metadata from Amazon S3 into the `city_look_up` table**.
4. **Transforming and loading weather data into the `weather_data` table**.
5. **Joining the two tables** and exporting the enriched data to the data lake.

### Key Features
- **Scalability**: Handles large volumes of data with minimal manual intervention.
- **Parallel Processing**: Processes city metadata and weather data simultaneously for efficiency.
- **Monitoring**: Tracks pipeline execution and provides error handling for individual tasks.
