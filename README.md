# Movie-Quality-Data-Analysis-Pipeline

# Architecture Overview:

# 1.AWS Glue ETL Job:

Data is ingested from S3 (movies-gds) into AWS Glue using a crawler (crawl_movies_data_in_s3).
The data quality checks are applied using AWS Glue’s EvaluateDataQuality function. The data is processed with transformations, and filtered into two groups: failed records and successful records.
Both the filtered data (successful and failed) is written to S3, and the transformed data is saved into the Glue Data Catalog and Redshift for further analysis.

# 2.AWS Step Functions:

Orchestrates the ETL pipeline by running the Glue crawler and waiting for it to finish before triggering the Glue job.
The Step Function includes checks for job success or failure:
If the Glue job succeeds, a success notification is sent via SNS.
If the Glue job fails, a failure notification with details is sent to SNS.

# 3.SNS Notifications:

SNS is used to send success or failure messages to subscribers (e.g., email notifications).
# 4.EventBridge:

The system is event-driven with EventBridge capturing file uploads to S3 (movies-gds) and triggering the data pipeline.
EventBridge listens for data quality evaluation results (success or failure) from AWS Glue and triggers notifications accordingly.
