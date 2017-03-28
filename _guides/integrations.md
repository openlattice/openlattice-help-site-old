---
layout: page
title: How to integrate your data into Loom
description: Create a dataset and integrate your existing data.
weight: 1
---

* TOC
{:toc}

## 1. Create Property and Entity Types

**Data integrations** take your data and map it to Loom's data model. At this time, you need a Loom administrator to create these for you.

> **How do I submit a request?** List your column headers and descriptions in this [this spreadsheet](/files/DatasetColumnHeaderSubmission.xlsx) and email it to us at [{{site.email}}](mailto:{{site.email}}). An administrator will reply within 24 hours with further instructions.

## 2. Add a new Dataset

Once you receive the property and entity type information for your dataset, go to the **Datasets** tab and create the dataset with your entity set information.

{%
  include image.html
  caption="Example: Creating a dataset for crime index dataset"
  path="guides/integrations/create-datasource.png"
%}

## 3. Create an Integration account

You need an account with **write permissions** to add data to a dataset. At this
time, there are a couple of ways you could do this:

<span class="bad">Not Recommended:</span> _Dataset owners use their personal login_

> Owners have write permissions by default, but can also read and delete data, manage user permissions, and change the dataset owner. Dataset owners _can_ use their personal login credentials to perform a data integration, but this is not recommended.

<span class="good">Recommended:</span> _Create a separate Integration Account_

> Create a new Loom account and use those credentials for your data integrations.
Integration Accounts are no different
than regular user accounts, except it will only be granted write permissions, not owner permissions, to your dataset. **Anyone with the login credentials to your Integration Account will have permissions to add data to your dataset, so it's important to keep these credentials in a safe place.**

You can skip this step if you've performed a data integration and already have an integration account.

{%
  include related.html
  content="
* [How to sign up for a new account](/guides/signups/)
" %}

## 4. Grant your account Write permissions

To grant your integration account write permissions, log into your dataset owner account and go to **Datasets**. From the **Actions** menu, click **View Details**.

{%
  include image.html
  caption="View Details Action Menu"
  path="guides/integrations/view-details.png"
%}

Under **Manage Permissions** (for the dataset and each of its properties) go to the **Emails** tab and select **Write** permissions. Search for your integration account email and grant the account write permissions.

{%
  include image.html
  caption="Manage permissions for Dataset and Properties" path="guides/integrations/grant-write-permissions.png"
%}

{%
  include related.html
  content="
* [How to request, grant, and revoke permissions](/guides/permissions/)
" %}

## 5. Install Java and a Programming Text Editor

First, install the [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

We also recommend downloading a programming text editor in order to easily view and edit the integration files we provide you:

* [Notepad++](https://notepad-plus-plus.org/)
* [Atom](https://atom.io/)
* [Sublime Text 3](https://www.sublimetext.com/)


## 6. Download template integration script

We've provided you with a template integration script to help get you started. This integration
script was used to integrate the [Sample OpenFlights Airport Data](https://thedataloom.com/gallery/#/entitysets/514e6a5f-2bcd-4206-bb1c-448a9dbcf06d).

Download and unzip the [project](/files/tutorial.zip).

```text
airports.csv (Replace with your CSV)
build.gradle
/gradle/wrapper
    gradle-wrapper.jar
    gradle-wrapper.properties
gradlew
gradlew.bat
settings.gradle
/src/main/java/com/dataloom/integrations/dataintegration
    DataIntegration.java
/src/main/java/resources
    jetty.yaml
    log4j.properties
    log4j2.properties
    rhizome.yaml
```

## 7. Configure Gradle

[Gradle](https://gradle.org/) is a build tool you will use for running the integration script. In order for Gradle to run properly, change the following files to match your project:

**settings.gradle**
```gradle
rootProject.name='tutorial'
```
* `tutorial` should match the name of your project's root directory. (Changing this is optional.)

**build.gradle**
```gradle
description = "Sample OpenFlights Airport Data"
run.args = ["airports.csv","test@example.com","examplepassword"]
```
* `description` should match the title you created for your new dataset.
* `run.args` should contain the name of your CSV file (if you are integrating a CSV) and login credentials for an account with write permissions to your dataset. **This will either be your login credentials if you are the dataset owner, or the login credentials of your integration account.**

## 8. Define your Integration

The main integration script is located at:

```text
/src/main/java/com/dataloom/integrations/dataintegration/DataIntegration.java
```

### Define the entity set you are integrating data into

Set the values of `ENTITY_SET_NAME`, `ENTITY_SET_TYPE`, and `ENTITY_SET_KEY` (or keys) for your dataset.

Make sure these values match the information for the dataset you created.

```java
// Set your Entity Set Name, Type, and Primary Key
public static String ENTITY_SET_NAME = "openflightsdata";
public static FullQualifiedName ENTITY_SET_TYPE = new FullQualifiedName( "sample.openflightsdata" );
public static FullQualifiedName ENTITY_SET_KEY = new FullQualifiedName( "aviation.id" );
```

### Configure Apache Spark to read your datasource

[Apache Spark](http://spark.apache.org/) is the data engine for reading and processing your data. These settings will vary depending on the type of data source you are trying to integrate.

```java
// Example Spark Configuration for CSV Data Integrations
Dataset<Row> payload = sparkSession.read()
    .format( "com.databricks.spark.csv" )
    .option( "header", "true" )
    .load( path );
```

{% include related.html content="
* [How to configure Apache Spark for data integrations](/guides/spark/)
"%}

### Use Shuttle API to map your data to Loom's properties

[Loom's Shuttle API](/api/) maps your data to the Loom data model and uploads
your data to Loom's servers. Replace the properties and column names with the values that correspond with your dataset.

**Full Flight code:**
```java
Flight flight = Flight.newFlight()
    .addEntity( ENTITY_SET_TYPE )
        .to( ENTITY_SET_NAME )
        .key( ENTITY_SET_KEY )
        .addProperty( new FullQualifiedName( "aviation.name" ) )
        .value( row -> row.getAs( " Name" ) ).ok()
        .addProperty( new FullQualifiedName( "aviation.id" ) )
        .value( row -> row.getAs( "Airport ID" ) ).ok()
        .addProperty( new FullQualifiedName( "aviation.iata" ) )
        .value( row -> row.getAs( " IATA" ) ).ok()
        .addProperty( new FullQualifiedName( "location.longitude" ) )
        .value( row -> row.getAs( " Longitude" ) ).ok()
        .addProperty( new FullQualifiedName( "location.latitude" ) )
        .value( row -> row.getAs( " Latitude" ) ).ok()
        .addProperty( new FullQualifiedName( "general.altitude" ) )
        .value( row -> row.getAs( " Altitude" ) ).ok()
        .addProperty( new FullQualifiedName( "general.city" ) )
        .value( row -> row.getAs( " City" ) ).ok()
        .addProperty( new FullQualifiedName( "general.country" ) )
        .value( row -> row.getAs( " Country" ) ).ok()
        .addProperty( new FullQualifiedName( "aviation.icao" ) )
        .value( row -> row.getAs( " ICAO" ) ).ok()
    .ok()
.done();
```

**Example with only one property:**
```java
Flight flight = Flight.newFlight()
    .addEntity( ENTITY_SET_TYPE )
        .to( ENTITY_SET_NAME )
        .key( ENTITY_SET_KEY )
        // Add Properties here
        .addProperty( new FullQualifiedName( "aviation.name" ) )
        .value( row -> row.getAs( " Name" ) ).ok()
    .ok()
.done();
```

> **Common Error:** You may encounter an error with Apache Spark if your column headers contain spaces after each comma.
>
> ```text
> Airport ID, Name, City, Country, IATA, ICAO...
> ```
> To fix this, either remove the extra spaces from your CSV, or include the spaces in the column header String: `row.getAs( " Name" )`

## 9. Create custom functions to parse your data

Sometimes, your data might need custom functions to parse data in your dataset.
For example, if your data had a `Name` column, but you wanted to split `Name`
into `firstname` and `lastname`, you could write custom functions to extract that
data.

### Name Parsing

Here is an example of two custom functions for parsing `Name` data into
a `firstname` and `lastname`.

```java
// Custom Functions for Parsing First and Last Names
public static String getFirstName( Object obj ) {
  String name = obj.toString();
  String[] names = name.split( "," );
  return names[ 1 ].trim();
}

public static String getLastName( Object obj ) {
  String name = obj.toString();
  String[] names = name.split( "," );
  return names[ 0 ].trim();
}
```
```java
// Corresponding Flight Code
.addProperty( new FullQualifiedName( "general.firstname" ) )
    .value( row -> getFirstName( row.getAs( "Name" ) ) .ok()
```

### Date Formatting
Loom stores dates in [JODA datetime](http://joda-time.sourceforge.net/apidocs/org/joda/time/DateTime.html) formats. To ensure your dates and times are properly formatted, please use the provided `DateTimeFormatter.java` class to parse any dates or times in your data:

```java
FormattedDateTime date = new FormattedDateTime( DATE_AS_STRING , TIME_AS_STRING, "MM/dd/yyyy", "HH:mm:ss")
```

**Example Implementation:**
```java
// Custom Functions for Standardizing Dates as JODA format
public static String standardizeDate( Object myDate ) {
    if (myDate != null && !myDate.equals("")) {
        String d = myDate.toString();
        String ddate = d.split(" ")[0];
        String dtime = d.split(" ")[1];
        FormattedDateTime date = new FormattedDateTime( ddate, dtime, "MM/dd/yyyy", "HH:mm:ss");
        return date.getDateTime();
    }
    return null;
}
```

```java
// Corresponding Flight Code
.addProperty( new FullQualifiedName( "general.dob" ) )
    .value( row -> standardizeDate( row.getAs( "Birth Date" ) ) .ok()
```

## 10. Run your Integration

Open up the command line and navigate to the integration project directory and use the `gradlew` command to run your integration script:

**Mac/Linux:**
```
$ cd path/to/project
$ ./gradlew run
```

**Windows:**
```
$ cd path\to\project
$ .\gradlew.bat run
```

Once the integration completes, you will be able to begin managing your dataset
on Loom.

{%
  include related.html
  content="
* [Setting and requesting permissions](/guides/permissions/)
* [Linking datasets](/guides/linking/)
"
%}
