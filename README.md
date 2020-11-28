# Production-of-Cryptocurrency-Data-Lake-Using-Spark-
This project is a ETL pipeline processing structured financial data and unstructured social media data related to cryptocurrencies(dataset with millions of record). Which prepare for exploring the relationship between the price trend of cryptocurrency assets and the sentiment of its social media platform.

#Use python, spark, BianceAPI, etc. to extract tradedata from the cryptocurrency exchange platform, transform it to marketdata on AWS EMR, and store it in AWS S3 Bucket. 

#Use python, spark, TwitterAPI, etc. to extract tweets from the Twitter platform, transform and store them in AWS S3 Bucket. 

#Perform data quality checks on tweets and marketdata and persist them on AWS S3 Bucket. 

#Utilized:Python,Pyspark,Spark,SQL,AWS,Amazon S3,AWS EMR,BianceAPI,TwitterAPI,Data Quality,Structured data,Unstructured Data,Data Lake,ETL,Big Data,Hadoop.
# Introduction

The project uses financial- and social data related to the crypto currency industry. The idea is to prepare data for internal analysis purposes  e.g. to explore the relationship between price trends of cryptocurrency assets (e.g. Bitcoin) and its sentiment on social media platforms. 
Hereby, structured financial market data (e.g. Bitcoin price over a year) as well as unstructured social media data (e.g. Bitcoin tweets on twitter) will be extracted from the source platforms (cryptocurrency exchange, twitter etc). Then the data will be transformed (e.g. remove duplicates, denormalize spouce data). Finally, the data are loaded to the target system (e.g. S3 system)

# Project Scope

The project simulates an ETL process using Jupyter Notebook. As part of the ETL process quality checks are implemented. For demo purposes all steps of the ETL process are executed locally. Hereby, the a local workspace simulates a S3 bucket. Note that the pre-processing (e.g tokenization for sentiment analysis) of the unstructered social media data is not considered to be part of the ETL process and need to be done in seperate project.
The final purpose of the data model to work with atomar and denormalized dataset. Hereby, 6 final tables are created and loaded into the destination system as parquet files. Furthermore, the project aims to utilize the Spark kernel focusing on Pyspark Syntax. 

# High Level Architecture

Tradingdata about the different cryptocurrency assets will be retrieved from a public third-party API (cryptocurrency exchange binance). Hereby, a get request will be sent to the exchange requesting data about the daily closes for the last 360 days. A similar approach is taken to request the data from Twitter API. However, the Twitter API is non-public. Therefore, a Twitter App providing credentials is created. 
The data transformation of the tweets and financial data is done locally on a Jupyter Notebook. However, in a productive scenario a data lake could be created using using Amazons Elastic Map Reduce (EMR) and Spark.
As last step of the ETL process the data are written as a parquet file to a S3 bucket. At this point business analysts and data scientist can post-process the cleaned parquet files support insight generation and decision making.

![alt text](https://raw.githubusercontent.com/MatthiasReichel/Cryptocurrency-Datalake-Spark/master/images/Datamodel.png)

Note: The calls to the TwitterAPI are strictly limited. Therefore, the decision was made to add an additional layer storing extracted tweets on a S3 bucket. This step is necessary to avoid a request limit is reached.

# Data Model

Note that "tables" are used as synonym for spark dataframes. The Datamodel shows the datamodel which was implemented as part of the ETL process. However, as Spark (Pyspark) is applied spark dataframes were created instead of sql tables.  

![alt text](https://raw.githubusercontent.com/MatthiasReichel/Cryptocurrency-Datalake-Spark/master/images/High-Level-Architecture.png)

# Considerations

As shown in image 1 the data transformation could be executed on AWS EMR using a "Schema on Read" approach. Hereby, the trading and social data could be extracted and transformed in the EMR layer and stored on an AWS S3 bucket. As the scope of the project covers the daily close it is sufficient to execute the processing once a day. The pipeline itself could be observed by integrating e.g. airflow into the ETL process.  
To enhance the perfomance additional nodes can be added on EMR. Furthermore, partitioning could be applied when saving the final datamodel to S3. Finally, it is to highlight that that EMR cluster does not need to run 24 hours unless realtime information are necessary. 
