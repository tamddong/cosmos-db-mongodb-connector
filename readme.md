# CosmosDB Spark Connector - MongoDB API

This tutorial helps to use Spark Connector with CosmosDB using MongoDB API

## Getting Started

The official CosmosDB Spark Connector for SQL API can be found : [here](https://github.com/Azure/azure-cosmosdb-spark).
### Prerequisites
1. You need to create a HDInsight Spark cluster using this [guide](https://docs.microsoft.com/en-us/azure/hdinsight/spark/apache-spark-jupyter-spark-sql).
2. Install the MongoDB-Spark jar using this [guide](https://github.com/Azure/azure-cosmosdb-spark/wiki/Spark-to-Cosmos-DB-Connector-Setup)
  Instead of loading the SQL API jar like in the guide, use this jar [org.mongodb.spark:mongo-spark-connector_2.11:2.4.0](https://mvnrepository.com/artifact/org.mongodb.spark/mongo-spark-connector_2.11/2.4.0)
3. Create a CosmosDB with MongoDB API
4. Create a collection and an item you'd like you write data to using the Spark Connector

### Configuration

#### PySpark Jypyter Config
```
%%configure -f
{ "name":"Spark-to-Cosmos_DB_Connector",
  "conf": {
    "spark.jars.packages": "org.mongodb.spark:mongo-spark-connector_2.11:2.4.0",
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
  }
}
```
### MongoDB Spark Connector

#### PySpark 
```
CONNECTION_STRING = "mongodb://youralias-cosmosdb-mongo:rc3M7b17nk3mskZgNmJIGNfNL1RurvioLnpxM6CXZNkhwBr0dBorPZZvNLVrtrh1TNvliyQjjDyCGUXhNkwtPg==@youralias-cosmosdb-mongo.mongo.cosmos.azure.com:10255/collectionname.itemname?ssl=true&replicaSet=globaldb&maxIdleTimeMS=120000&appName=@youralias-cosmosdb-mongo@"
```
You can get your connection string in your CosmosDB MongoDB API > Azure Portal > Connection String
Also remember to add in your collection name and item name after the port number. (In the sample link above, port number is 10255)

#### How to save data from Spark to CosmosDB using MongoDB format
```
save_df = spark.createDataFrame([("1994",  32143,"Aaron"),("2052", 54354,"Jeff"), ("2015",54353,"Nick" )], ["year", "zip","name"])
save_df.write.format("com.mongodb.spark.sql").option("uri",CONNECTION_STRING).mode("overwrite").save()
```
#### How to load data from CosmosDB to Spark using MongoDB format
```
read_df = spark.read.format("com.mongodb.spark.sql").option("uri",CONNECTION_STRING).load()
read_df.show()
```

