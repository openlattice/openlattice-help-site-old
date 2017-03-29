---
layout: page
title: How to configure Apache Spark for data integrations
description: Instructions for configuring Apache Spark in data integrations
---

## 1. Set Your Database Driver
[Apache Spark](http://spark.apache.org/) is the data engine used for reading and processing your data.

If you are integrating data from a datasource that is _not_ a CSV, add a **Java Database Connectivity (JDBC) driver** to your Gradle project's build dependecies:

**build.gradle:**
```
dependecies {
  ...
  compile group: 'mysql', name: 'mysql-connector-java', version: '6.0.6'
  ...
}
```
* PostgreSQL: [https://mvnrepository.com/artifact/org.postgresql/postgresql](https://mvnrepository.com/artifact/org.postgresql/postgresql){:target="_blank"}
* MySQL: [https://mvnrepository.com/artifact/mysql/mysql-connector-java](https://mvnrepository.com/artifact/mysql/mysql-connector-java){:target="_blank"}

## 2. Configure Apache Spark
This will be configured in your `DataIntegration.java` file, which can be found in your `src` folder:

```text
/src/main/java/com/dataloom/integrations/dataintegration/DataIntegration.java
```

 Here are some examples of Apache Spark configurations for CSV and MySQL integrations:

**CSV**:
```java
Dataset<Row> payload = sparkSession.read()
  .format( "com.databricks.spark.csv" )
  .option( "header", "true" )
  .load( path );
```
* `com.databricks.spark.csv` specifies the `csv` data format for Sparks
* Set `header` to `true` if the first row is a header row
* Set `header` to `false` if the first row is not a header row
* `load( path )` will load the `your_csv_filename.csv` you specified from `build.gradle`

**MySQL**:
```java
Dataset<Row> payload = sparkSession.read()
  .format( "jdbc" )
  .option( "url", "jdbc:mysql://127.0.0.1:3306/Test" )
  .option( "driver", "com.mysql.jdbc.Driver" )
  .option( "dbtable", "mytablename" )
  .option( "user", "root" )
  .option( "password", "mypassword" )
  .load();
```
* `jdbc` is a database-independent Java API for reading data from MySQL and PostgreSQL databases.
* Set `url` to the url scheme for your database: `jdbc:mysql://[host]:[port]/[database]`
* Set `dbtable` to the name of the table you'd like to integrate.
* Set `user` and `password` to the login credentials for your database.

**Parquet**:
A tutorial for integrating Parquet files is coming soon. Please see to the following reference for more information:
* [http://spark.apache.org/docs/latest/sql-programming-guide.html#parquet-files](http://spark.apache.org/docs/latest/sql-programming-guide.html#parquet-files)

**JSON**:
A tutorial for integrating JSON files is coming soon. Please see to the following reference for more information:
* [http://spark.apache.org/docs/latest/sql-programming-guide.html#json-datasets](http://spark.apache.org/docs/latest/sql-programming-guide.html#json-datasets)


<hr>

{% include related.html content="
* [Go back to Loom's Data Integration tutorial](/guides/integrations/#configure-apache-spark-to-read-your-datasource)
"
%}


