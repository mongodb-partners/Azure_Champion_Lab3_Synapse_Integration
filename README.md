# Lab 3 - MongoDB Atlas Integration with Synapse Analytics

## Introduction:

Data is growing day by day and it is paramount for companies to be able to manage their data silos. Also, important in today’s Data economy is the need to make consistent and real-time data available for quick analytics and to derive faster and better insights.

MongoDB Atlas is a great choice for the foundation of an Operational Data Layer (ODL) which can bring an end to the woes of the siloed data and can be that central Single view which can enable large scale limitless real-time analytics. Azure Synapse analytics is a unified platform that supports the multi-dimensional, infinite scale  analytics needs of the modern enterprise.Thus, the synergy would enable any enterprise to operationalise their siloed data and use it for powerful analytics to derive rich insights from their data.

## MongoDB Atlas and Synapse integration:

### Objectives:

In this Lab you will explore both Batch and Real time Sync (optional). There are two methods to achieve Real time sync.

[Batch/ Microbatch integration using the MongoDB connector in Synapse](#batch-integration)

#### OPTIONAL

[Real time integration using MongoDB Atlas Triggers and Azure Functions](#using-mongodb-atlas-triggers-and-azure-functions)

OR

[Real time integration using MongoDB Spark Streaming Connector](#using-mongodb-spark-streaming-connector)

#### Batch Integration

In Azure Synapse Analytics, you can integrate MongoDB on-premises instances and MongoDB Atlas as a Source or Sink resource. With historical data, you can retrieve all the data at once. You can also retrieve data incrementally for specific periods by using a filter in batch mode. Then you can use SQL pools and Apache Spark pools in Azure Synapse Analytics to transform and analyze the data. If you need to store the analytics or query results in an analytics data store, you can use the MongoDB sink resource in Azure Synapse Analytics.

img1

For more information about how to set up and configure the connectors, see these resources for [MongoDB](https://learn.microsoft.com/en-us/azure/data-factory/connector-mongodb?tabs=data-factory) and [MongoDB Atlas](https://learn.microsoft.com/en-us/azure/data-factory/connector-mongodb-atlas?tabs=data-factory). 

In this Lab, we are going to set up a simple Synapse pipeline to copy data from MongoDB Atlas collection to the default storage ADLS Gen2 in Synapse. The high level steps involved in this exercise are:  
* Creating an **Azure Synapse Pipeline**
* Setting Up **MongoDB Atlas as the Source** in the Pipeline using the direct MongoDB connector
Setting Up the default **ADLS Gen2 in Synapse as the Sink** in the Pipeline using the Azure Blob Storage connector
* Publishing the changes to **save the Pipeline** and **trigger the Pipeline** to have the data copied from MongoDB Atlas collection to the default ADLS Gen2 in Synapse

Follow the Github link [here](https://github.com/mongodb-partners/Azure_Synapse_Batch_Integration_MongoDBConnector) for a step by step process to practice this Lab.

#### OPTIONAL

**Real Time Integration** 
For a real time sync or change data capture, an OOTB solution is not yet available, however a custom solution is published at Architecture Center [here](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/analytics/azure-synapse-analytics-integrate-mongodb-atlas). There are some other solutions also like using a MongoDB [spark streaming connector](https://www.mongodb.com/blog/post/introducing-mongodb-spark-connector-version-10-1) or [MongoDB Atlas triggers](https://www.mongodb.com/docs/atlas/app-services/triggers/).

#### Using MongoDB Atlas Triggers and Azure Functions

The real time sync between MongoDB Atlas and Synapse can be achieved using MongoDB [change streams](https://www.mongodb.com/docs/manual/changeStreams/) and a solution is detailed in the Architecture document [here](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/analytics/azure-synapse-analytics-integrate-mongodb-atlas). In this lab, we will perform a much simpler solution and the major advantage of this solution is that it uses [Atlas triggers](https://www.mongodb.com/docs/atlas/app-services/triggers/) and [functions](https://www.mongodb.com/docs/atlas/app-services/functions/) which abstracts the code needed to set up change streams and take an action based on the same.

img2

In this Lab, we will create an Atlas trigger, Atlas function and Azure function to copy the full document inserted into the MongoDB Atlas collection in an Insert operation to the default ADLS gen2 storage in Azure Synapse. The high level steps involved in this exercise are:

* _Fetch the attributes_ pertaining to the default ADLS Gen2 storage of the Synapse Workspace
* _Create an Azure function_ adding the ADLS Gen2 storage attributes as environment variables (saved in previous step)
* _Create an Atlas trigger_ to keep watching the MongoDB collection for any Insert operations adding an Atlas function to invoke the Azure function end point (created in previous step)
* _Insert a document_ into the MongoDB collection and test the connectivity from 

Insert event -> Atlas Trigger -> Atlas function -> Azure function -> Store in ADLS Gen2(Synapse)

**Follow the Github link [here](https://github.com/mongodb-partners/Azure_Synapse_RealTimeSync_Using_AtlasTrigger_and_AzureFunction) for a step by step process to practice this Lab.**

#### Using MongoDB Spark Streaming Connector

MongoDB’s latest [Spark connector v10.1](https://www.mongodb.com/blog/post/introducing-mongodb-spark-connector-version-10-1)  provides [streaming capabilities](https://www.mongodb.com/docs/spark-connector/current/structured-streaming/) which allows streaming of changes from MongoDB or to MongoDB in both continuous and micro-batch modes. Using the connector, we just need a small piece of code that reads a stream of changes from MongoDB collection and writes to the ADLS gen2 in Synapse in a table format which can be queried like any other tables. The code is packaged as a Pipeline template and the User just needs to Import the pipeline, give the parameters to point to the MongoDB collection and table name in ADLS Gen2 and trigger the Pipeline.

img3

In this Lab, we will set up an Azure Synapse pipeline with a spark notebook that can listen to changes in  a MongoDB collection (parameter) and push the entire inserted document into a table in the default ADLS gen2 in Synapse. The high level steps involved in this exercise are:

* _Pre-requisties specific to Spark streaming_ : This step involves adding the MongoDB spark connector package in Synapse, creating a Spark pool and associating the MongoDB connector package to the spark pool.
* _Import the Pipeline_ packaged as a zip file which already has the Spark notebook step set up.
* _Provide the parameters required for the Spark notebook_ which is basically the connection string, database and collection names for the MongoDB collection to be watched upon for Insert operations and the name of the output table in ADLS Gen2.
* _Trigger the pipeline_ and verify if the insertion of the document into the MongoDB Atlas did cause the change to get replicated in the table specified in ADLS gen 2 in Synapse.

**Follow the Github link [here](https://github.com/mongodb-partners/Synapse-Spark-Streaming) for a step by step process to practice this Lab.**

















