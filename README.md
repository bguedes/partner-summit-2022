# Partner Summit 2022 - Workshop Student Guide

![](images/banner.png)

This document guides students through the Hands on lab for Partner Summit 2022. It will take you step by step to completing the Prerequisites and deliver this demo.

---

## Introduction

The purpose of this repository is to enable the easy and quick setup of the Partner Summit workshop. Cloudera Data Platform (CDP) has been built from the ground up to support hybrid, multi-cloud data management in support of a Data Fabric architecture. This worshop provide an introduction to CDP, with a focus on the data management capabilities that enable the Data Fabric and Data Lakehouse.

## Overview

In this exercise, we will work get stock data from [Alpha Vantage](https://www.alphavantage.co/), offers free stock APIs in JSON and CSV formats for realtime and historical stock market data,

- Data ingestion and streaming—provided by ***Cloudera Data Flow (CDF)*** and
***Cloudera Data Engineering (CDE)**.
- Global data access, data processing and persistence—provided by ***Cloudera Data Hub (CDH)***.
- Data visualization with ***CDP Data Visualization***.

***Cloudera DataFlow (CDF)*** is a scalable, real-time streaming analytics platform that ingests, curates, and analyzes data for key insights and immediate actionable intelligence. CDF’s Flow Management is powered by Apache NiFi, a no-code data ingestion and management solution. Apache NiFi is a very mature open source solution meant for large scale, high velocity enterprise data ingestion use cases.

***Cloudera Data Engineering (CDE)*** is a serverless service for Cloudera Data Platform that allows you to submit batch jobs to auto-scaling virtual clusters. CDE enables you to spend more time on your applications, and less time on infrastructure. CDE allows you to create, manage, and schedule Apache Spark jobs without the overhead of creating and maintaining Spark clusters. With Cloudera Data Engineering, you define virtual clusters with a range of CPU and memory resources, and the cluster scales up and down as needed to run your Spark workloads, helping to control your cloud costs.

***CDP Data Visualization*** enables data engineers, business analysts, and data scientists to quickly and easily explore data, collaborate, and share insights across the data lifecycle—from data ingest to data insights and beyond. Delivered natively as part of Cloudera Data Platform (CDP), Data Visualization delivers a consistent and easy to use data visualization experience with intuitive and accessible drag-and-drop dashboards and custom application creation.

![](images/architecture.png)

## Pre-requisites

1. Laptop with a supported OS (Windows 7 not supported) or Macbook.
2. A modern browser - Google Chrome (IE, Firefox, Safari not supported).
2. Wifi Internet connection.


## Step 1: Get Alpha Vantage Key

![](images/alphaVantagePortal.png)

![](images/claimApiKey.png)

![](images/getKey.png)

## Step 1: Access CDP Public Cloud Portal

Please use the login url [Workshop login](https://login.cdpworkshops.cloudera.com/auth/realms/se-workshop-1/protocol/saml/clients/cdp-sso)

![](images/login1.png)

Enter the username and password shared by your instructor.

![](images/login2.png)

You should be able to get the following home page of CDP Public Cloud.

![](images/login3.png)

## Step 2: Create the flow to ingest stock data via API to Object Storage

![](images/setWorkloadPasswordStep1.png)

![](images/setWorkloadPasswordStep2.png)

![](images/setWorkloadPasswordStep3.png)

![](images/setWorkloadPasswordStep4.png)

## Step 3: Create the flow to ingest stock data via API to Object Storage

![](images/portalCDF.png)


### Create a new CDF Catalog

![](images/cdfManageDeploymentStep0.png)

![](images/cdfImportFowDefinition.png)

[Stocks_Intraday_Alpha_Template.json](Stocks_Intraday_Alpha_Template.json)

![](images/cdfFlowCatalogCreated.png)


### Deploy DataFlow

![](images/cdfFlowDeploy.png)

![](images/cdfDeploymentStep1.png)

![](images/cdfDeploymentStep2.png)

![](images/cdfDeploymentStep3.png)

![](images/cdfDeploymentStep4.png)

![](images/cdfDeploymentStep5.png)

![](images/cdfDeploymentStepFinal.png)

![](images/cdfDeploymentStepDeploying.png)

![](images/cdfWorking.png)

###  View Nifi DataFlow

![](images/cdfWorking.png)

![](images/cdfManageDeploymentStep1.png)

![](images/cdfManageDeploymentStep2.png)

![](images/nifiDataflow.png)

### Create Iceberg Table

```sql

CREATE DATABASE stocks;

CREATE TABLE IF NOT EXISTS stocks.stock_intraday_1min (
  interv STRING,
  output_size STRING,
  time_zone STRING,
  open DECIMAL(8,4),
  high DECIMAL(8,4),
  low DECIMAL(8,4),
  close DECIMAL(8,4),
  volume BIGINT)
PARTITIONED BY (
  ticker STRING,
  last_refreshed string,
  refreshed_at string)
STORED AS iceberg;

```

## Step 4: Process and Ingest Iceberg using CDE

## Step 5: Query Iceberg Tables in Hue and Cloudera Data Visualization

```sql

DESCRIBE HISTORY stocks.stock_intraday_1min;

```

```sql

SELECT count(*), ticker
FROM stocks.stock_intraday_1min
FOR SYSTEM_VERSION AS OF <snapshotid>
GROUP BY ticker;

```

```sql

SELECT count(*), ticker
FROM stocks.stock_intraday_1min
GROUP BY ticker;

```
