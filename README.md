# ETL Project: S3 – Glue – Redshift

## Overview
This project demonstrates an ETL process using AWS S3, Glue, and Redshift.

## AWS Services Overview
### S3
Amazon S3 (Simple Storage Service) is an object storage service offering industry-leading scalability, data availability, security, and performance.

### Redshift
Amazon Redshift is a fully managed data warehouse service that allows for fast querying of large datasets.

### Glue
AWS Glue is a fully managed ETL service that makes it easy to move data between your data stores.

## Prerequisites
- AWS account
- AWS CLI installed and configured


### Architecture
![architecture](https://github.com/user-attachments/assets/2ab3cb9a-4669-4cce-a84b-d8b066b0f622)


## Steps to Setup
### 1. S3 Bucket
1. Create an S3 bucket named "awsetljob".
2. Upload `employee_data.csv` to the bucket.

### 2. Amazon Redshift Cluster
1. Create a Redshift cluster with the following configurations:
   - **Node Type:** `dc2.large`
   - **Number of Nodes:** `1`
   - **Admin Username:** `awsuser`
   - **Admin Password:** `Awsuser1`
2. Configure security groups and VPC as necessary.

### Create Table in Amazon Redshift
create a table inside Redshift with the apropriate structure and datatypes (for the final output)

### 3. VPC Endpoint
1. In the AWS Management Console, navigate to the VPC dashboard.
2. Create a VPC endpoint for S3:
   - **Service Category:** AWS services
   - **Service Name:** `com.amazonaws.us-east-1.s3` (type: Gateway)
   - **VPC:** Select the default VPC
   - **Route Table:** Select the default route table
3. Apply the necessary route table configurations.


### 4. AWS Glue
#### Create a New Database
1. In the AWS Glue console, click on the "Databases" link in the left-hand menu.
2. Click on the "Add database" button to create a new database.
3. Provide a name for the database, e.g., `etl_db`.

#### Create a New Connection
1. Go to the "Connections" section in the left-hand menu.
2. Click on "Create connection".
3. Search for Redshift and select it, then proceed to the next step.
4. Enter the Redshift cluster details:
   - **Connection Name:** `redshift-connection`
   - **Cluster ID:** Your Redshift cluster identifier
   - **Database Name:** `dev`
   - **Username:** `awsuser`
   - **Password:** `Awsuser1`
5. Click "Save" to create the connection.
6. Test the connection to ensure it is successful.


#### Create Crawlers
1. Create a crawler for S3:
   - **Name:** `s3-crawler`
   - **Role:** AWSGlueServiceRole
   - **Database Name:** `etl_db`
   - **Include Path:** `s3://<bukcet name>/employee_data.csv`
2. Create a crawler for Redshift:
   - **Name:** `redshift-crawler`
   - **Role:** AWSGlueServiceRole
   - **Database Name:** `etl_db`
   - **Include Path:** `dev/public/final`
3. Run the crawlers to catalog the data.

#### Create and Run Glue Jobs
1. Create a Glue job using the visual ETL tool:
   - **Job Name:** `etl-job`
   - **IAM Role:** AWSGlueServiceRole
2. Define the ETL process:
   - Extract data from the S3 crawler.
   - Drop unnecessary fields.
   - Filter data.( age above 30)
   - Change/check schema.
   - Load data into the Redshift crawler.
3. Save and run the job.

### post Run

review the output result in the redshift table.

