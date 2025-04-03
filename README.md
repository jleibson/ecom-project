# ecom-data-pipeline: A Data Engineering Project for E-commerce Data

## Overview

This repository contains a data engineering project designed to simulate the data processing and analysis workflows of an e-commerce platform. The project includes generating synthetic datasets, transforming data, querying data, and loading data into analytical databases using a variety of AWS services and tools. This comprehensive workflow showcases the application of data engineering principles and best practices.

## Technologies Used

*   **Python**: For generating synthetic datasets.
*   **Apache Spark (PySpark)**: For data transformation and ETL processes.
*   **AWS Glue**: For automating ETL workflows and converting data formats.
*   **Amazon S3**: For scalable and cost-effective data storage.
*   **Amazon Athena**: For querying data directly from S3 using SQL.
*   **Amazon Redshift**: For structured data storage and advanced analytics.
*   **IAM (Identity and Access Management)**: For managing permissions and securing access to AWS resources.

## Project Structure

The repository is organized into the following directories:

*   `data_generation/`: Contains Python scripts for generating synthetic datasets (customer, catalog, and transaction data).
*   `etl/`: Contains scripts for ETL processes:
    *   `glue_scripts/`: Contains glue transformation python scripts.
    *   `pyspark_scripts/`: Contains PySpark scripts for data transformation.
*   `sql/`: Contains SQL scripts for creating tables in Redshift and querying data in Athena.
*   `iam/`: Contains example JSON documents showcasing IAM policies.

## Detailed Steps

### 1. Data Generation

The first step is to generate synthetic datasets that mimic the structure and characteristics of real-world e-commerce data.

#### Scripts

*   `data_generation/generate_customer_data.py`: Generates synthetic customer data, including `CustomerID`, `Name`, `Email`, `Country`, and `SignupDate`.
*   `data_generation/generate_catalog_data.py`: Generates synthetic catalog data, including `ProductID`, `ProductName`, `Category`, `Price`, and `Stock`.
*   `data_generation/generate_transactions_data.py`: Generates synthetic transaction data, including `TransactionID`, `CustomerID`, `ProductID`, `Timestamp`, `Amount`, and `Country`.

#### How to Run

1.  Ensure you have Python installed.
2.  Install the Faker library:
    ```
    pip install Faker
    ```
3.  Run each script:
    ```
    python data_generation/generate_customer_data.py
    python data_generation/generate_catalog_data.py
    python data_generation/generate_transactions_data.py
    ```
4.  The scripts will generate three CSV files (`customer_data.csv`, `catalog_data.csv`, and `transactions_data.csv`) in the `data_generation/` directory. Sample datasets have been created for you, which reside in the `sample_data` directory, so you do not have to execute.

### 2. Data Transformation

The next step is to transform the raw CSV files into Parquet format for efficient querying and analytics. This is done using AWS Glue and PySpark scripts.

#### Scripts

*   `etl/pyspark_scripts/csv_to_parquet_pyspark.py`: Converts CSV files to Parquet format using PySpark. This script reads the data from S3, applies transformations, and writes the output back to S3.
*   `etl/glue_scripts/csv_to_parquet.py`: Can be uploaded as the source script for AWS Glue Studio jobs.

#### How to Run (PySpark)

1.  Ensure you have Apache Spark installed and configured.
2.  Install the `boto3` library for AWS interaction:
    ```
    pip install boto3
    ```
3.  Set up AWS credentials to allow the script to access S3.
4.  Modify the script to point to your S3 bucket and data locations.
5.  Run the script:
    ```
    spark-submit etl/pyspark_scripts/csv_to_parquet_pyspark.py
    ```

#### How to Run (AWS Glue Studio)

1.  Create an AWS Glue Studio job.
2.  Configure the source to read CSV files from S3.
3.  Configure the transformation to convert the data to Parquet format.
4.  Configure the target to write Parquet files back to S3.
5.  Upload `etl/glue_scripts/csv_to_parquet.py` as the source script.
6.  Run the Glue job.

#### Output

The Parquet files will be written to the specified S3 locations (e.g., `s3://your-bucket/parquet/customer_data/`).

### 3. Querying Data with Athena

Amazon Athena allows you to query data directly from S3 without the need to load it into a database. To use Athena, you must first create a Glue Data Catalog crawler to register your datasets.

#### Steps

1.  Create a Glue Data Catalog crawler.
2.  Configure the crawler to scan your S3 bucket for datasets.
3.  Run the crawler to automatically discover schemas and register the datasets in the Glue Data Catalog.
4.  Once the datasets are registered, you can query them using SQL in Athena.

#### Example Queries

The `sql/athena_queries.sql` file contains example queries for analyzing the data.

### 4. Loading Data into Redshift

To perform advanced analytics on structured data, you can load the transformed datasets into Amazon Redshift, a fully managed data warehouse service.

#### Steps

1.  Create a Redshift cluster.
2.  Define schemas for the datasets in Redshift. The `sql/redshift_ddl.sql` file contains the DDL (Data Definition Language) statements for creating the tables.
3.  Load the data from S3 into Redshift using the `COPY` command. You will need to configure an IAM role with permissions to access S3.

#### Example COPY Command
COPY customer_data
FROM 's3://your-bucket/customer_data.csv'
IAM_ROLE 'arn:aws:iam::<account-id>:role/<role-name>'
CSV
IGNOREHEADER 1;

1.  Create a Redshift cluster.
2.  Define schemas for the datasets in Redshift. The `sql/redshift_ddl.sql` file contains the DDL (Data Definition Language) statements for creating the tables.
3.  Load the data from S3 into Redshift using the `COPY` command. You will need to configure an IAM role with permissions to access S3.

#### Example COPY Command

COPY customer_data
FROM 's3://your-bucket/customer_data.csv'
IAM_ROLE 'arn:aws:iam::<account-id>:role/<role-name>'
CSV
IGNOREHEADER 1;


### 5. IAM Role Configuration

IAM (Identity and Access Management) is used to manage permissions for accessing AWS resources. You will need to configure an IAM role with the appropriate permissions for your AWS Glue jobs, Athena queries, and Redshift COPY commands. Sample Policies have been created for your convenience.

#### S3 Read-Only Access
Allows read-only access to your S3 bucket.

#### Glue Service Role
Allows full access to glue jobs and common S3 access.

#### Redshift Full Access
Allows Redshift access to operate on specified resources.

## IAM Policy Configuration

1. Create IAM roles and grant access according to the services you will utilize.

## Additional Considerations

### Cost Optimization

*   Use cost-effective storage options like S3 Glacier for archiving old data.
*   Scale compute resources dynamically to match workload demands.
*   Monitor costs regularly using AWS Cost Explorer.

### Security

*   Use encryption to protect data at rest and in transit.
*   Implement strong access controls using IAM.
*   Regularly audit security configurations.

### Automation

*   Automate data pipeline workflows using AWS Step Functions or Apache Airflow.
*   Use Infrastructure as Code (IaC) tools like AWS CloudFormation or Terraform to manage your infrastructure.
*   Implement automated monitoring and alerting using Amazon CloudWatch.

## Conclusion

This data engineering project provides a foundation for building and deploying data pipelines for e-commerce platforms. By following the steps outlined in this document, you can gain valuable experience with a variety of AWS services and tools.

## About The Author

Please view [my Github](github.com/jleibson) for my written works.

