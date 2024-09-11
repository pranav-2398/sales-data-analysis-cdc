# Change Data Capture and Analysis on AWS | Data Engineering Project

## List of Contents
- [Introduction](#introduction)
- [Architecture](#architecture)
- [End to End Flow](#end-to-end-flow)

## Introduction

This project was created for learning purposes and the intention behind doing it was to understand how change data capture works and how the same can be established using AWS Services. 

## Architecture

![Architecture](./assets/Sales%20Data%20CDC%20Architecture.png)

## End to End Flow

1. **Python Script** - The Python Script is triggered from Terminal (can be triggered from Lambda as well) which generates mock data to be stored in the DynamoDB Orders Table.
2. **DynamoDB Streams** - As data is getting published/updated continously, DynamoDB Stream will capture the changes with event being classified as an *INSERT* or *MODIFY* etc.  
3. **AWS EventBridge Pipe** - As soon as DynamoDB Stream data is generated, eventbridge pipe will be triggered which will copy data onto Kinesis Data Stream.
4. **AWS Kinesis Data Stream** - It will store the stream data in different shards as it will continously receive the data from the pipe.
5. **AWS Kinesis FireHose** - Consumes data from Kinesis Streams continously but writes in batches to target based on buffer size to reduce write calls.
6. **AWS Lambda** - Used as a custom transformer to transform the stream data to appropriate format before writing it onto target.
7. **AWS S3** - Target destination For storing the transformed Data from Firehose.
8. **Glue Crawler** - A crawler is created on the S3 data to register the metadata on the glue data catalog
9. **AWS Athena** - Used to query the S3 Data using Glue Data Catalog.