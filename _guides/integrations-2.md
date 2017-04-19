---
layout: page
title: Integrations Part 2 – Integration Scripts
description: Customize our provided template integration scripts. This guide is recommended for individuals who are completing data integrations. Reach out to help@thedataloom.com if you need any assistance with your integration.
weight: 1
---

* TOC
{:toc}


## 1. Set up your programming environment

The integration script we provide requries [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) to run. We also recommend that you use a programming text editor to edit the template integration files we provide you:

* [Notepad++](https://notepad-plus-plus.org/)
* [Atom](https://atom.io/)
* [Sublime Text 3](https://www.sublimetext.com/)


## 2. Download template integration script

Feel free to use our template integration script to get started. This integration
script was used to integrate the [Sample OpenFlights Airport Data](https://thedataloom.com/gallery/#/entitysets/514e6a5f-2bcd-4206-bb1c-448a9dbcf06d).

Download and unzip the [template project](/files/tutorial.zip). The folder contents should look like this:

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

## 3. Configure Gradle

[Gradle](https://gradle.org/) is a build tool for running the integration script. Update the following information in `build.gradle` to match your project:

**build.gradle**
```gradle
description = "Sample OpenFlights Airport Data"
run.args = ["airports.csv","test@example.com","examplepassword"]
```
* `description` should match the **title** you created for your new dataset.
* `run.args` should contain the name of your csv file for csv integrations, along with login credentials for an individual with write permissions to the dataset.

## 4. Configure Apache Spark

The main integration script `DataIntegration.java` is located at:

```text
/src/main/java/com/dataloom/integrations/dataintegration/DataIntegration.java
```

In this file, there will be some code that tells Apache Spark what format your datasource is in:

```java
// Example Spark Configuration for CSV Data Integrations
Dataset<Row> payload = sparkSession.read()
    .format( "com.databricks.spark.csv" )
    .option( "header", "true" )
    .load( path );
```

[Apache Spark](http://spark.apache.org/) is the data engine for reading and processing your data. These settings will vary depending on the type of data source you are trying to integrate.

{% include related.html content="
* [How to configure Apache Spark for data integrations](/guides/spark/)
"%}

## 5. Define your integration

In `DataIntegration.java` you will also need to define the entity types, relationships, and properties for your data.

### Define Entity Types

Entity types are a set of properties that describe a particular object or unit. For example, Arrest data might contain 3 different entity types:

| Entity Type | Properties included in this type                                                            |
|-------------|---------------------------------------------------------------------------------------------|
| Person      | First name, Last name, Identification Number, Home address, Gender, Race, and Date of Birth |
| Location    | City, Address, Apartment, PD Zone and Beat                                                  |
| Case        | UCR & NCIC codes, Case Number, Day of Week                                                  |

Using the information provided by your data model to define your entity types at the top of your script:

```java
// 3 Tables for 3 Entity Types: People, Cases, and Locations
public static String            PEOPLE_ENTITY_SET_NAME = "people";
public static FullQualifiedName PEOPLE_ENTITY_SET_TYPE = new FullQualifiedName( "general.person" );
public static FullQualifiedName PEOPLE_ENTITY_SET_KEY  = new FullQualifiedName( "general.personid" );
public static String            PEOPLE_ALIAS           = "people";

public static String            CASE_ENTITY_SET_NAME = "cases";
public static FullQualifiedName CASE_ENTITY_SET_TYPE = new FullQualifiedName( "general.case" );
public static FullQualifiedName CASE_ENTITY_SET_KEY  = new FullQualifiedName( "publicsafety.case" );
public static String            CASE_ALIAS           = "case";

public static String            LOCATION_ENTITY_SET_NAME = "locations";
public static FullQualifiedName LOCATION_ENTITY_SET_TYPE = new FullQualifiedName( "general.location" );
public static FullQualifiedName LOCATION_ENTITY_SET_KEY  = new FullQualifiedName( "general.address" );
public static FullQualifiedName LOCATION_ENTITY_SET_KEY2 = new FullQualifiedName( "general.apartment" );
public static String            LOCATION_ALIAS           = "location";
```

### Define Relationships

"What cases did this person appear in? What officers were involved in each case?"

You can answer these types of questions using Loom once you've defined relationships between your entities.

In the template integration, there are two relationships defined `"appearsin"` and `"occurredat"`:

```java
// RELATIONSHIPS
public static String            APPEARS_IN_ENTITY_SET_NAME = "appearsin";
public static FullQualifiedName APPEARS_IN_ENTITY_SET_TYPE = new FullQualifiedName( "general.appearsin" );
public static FullQualifiedName APPEARS_IN_ENTITY_SET_KEY  = new FullQualifiedName( "publicsafety.occdatetime" );
public static String            APPEARS_IN_ALIAS           = "appearsin";

public static String            OCCURRED_AT_ENTITY_SET_NAME = "occurredat";
public static FullQualifiedName OCCURRED_AT_ENTITY_SET_TYPE = new FullQualifiedName( "general.occurredat" );
public static FullQualifiedName OCCURRED_AT_ENTITY_SET_KEY  = new FullQualifiedName( "publicsafety.occdatetime" );
public static String            OCCURRED_AT_ALIAS           = "occurredat";
```

### Create Your Flight Path

Loom's [Shuttle API](/api/) maps your data to the Loom data model and uploads
your data to Loom's servers. Replace the properties and column names with the values that correspond with your dataset.

**Full Flight code:**

```java
Flight flight = Flight.newFlight()
        .createEntities()

        .addEntity( PEOPLE_ALIAS )
        .ofType( PEOPLE_ENTITY_SET_TYPE )
        .to( PEOPLE_ENTITY_SET_NAME )
        .key( PEOPLE_ENTITY_SET_KEY )

        .addProperty( new FullQualifiedName( "general.lastname" ) )
        .value( row -> row.getAs( "last name" ) ).ok()
        .addProperty( new FullQualifiedName( "general.firstname" ) )
        .value( row -> row.getAs( "first name" ) ).ok()
        .addProperty( new FullQualifiedName( "general.middlename" ) )
        .value( row -> row.getAs( "middle" ) ).ok()
        .addProperty( new FullQualifiedName( "general.personid" ) )
        .value( row -> row.getAs( "person id no" ) ).ok()
        .addProperty( new FullQualifiedName( "general.homeaddress" ) )
        .value( row -> row.getAs( "address" ) ).ok()
        .addProperty( new FullQualifiedName( "general.gender" ) )
        .value( row -> row.getAs( "sex" ) ).ok()
        .addProperty( new FullQualifiedName( "general.race" ) )
        .value( row -> row.getAs( "race" ) ).ok()
        .addProperty( new FullQualifiedName( "general.dob" ) )
        .value( row -> standardizeDate( row.getAs( "dob" ) ).ok().ok()

        .addEntity( CASE_ALIAS )
        .ofType( CASE_ENTITY_SET_TYPE )
        .to( CASE_ENTITY_SET_NAME )
        .key( CASE_ENTITY_SET_KEY )

        .addProperty( new FullQualifiedName( "publicsafety.ucr" ) )
        .value( row -> row.getAs( "ucr" ) ).ok()
        .addProperty( new FullQualifiedName( "publicsafety.ucrext" ) )
        .value( row -> row.getAs( "ucr ext" ) ).ok()
        .addProperty( new FullQualifiedName( "publicsafety.ncicdes" ) )
        .value( row -> row.getAs( "ncic des" ) ).ok()
        .addProperty( new FullQualifiedName( "publicsafety.ncicexpdesc" ) )
        .value( row -> row.getAs( "ncic exp desc" ) ).ok()
        .addProperty( new FullQualifiedName( "publicsafety.case" ) )
        .value( row -> row.getAs( "case" ) ).ok()
        .addProperty( new FullQualifiedName( "general.weekday" ) )
        .value( row -> row.getAs( "day of week" ) ).ok()
        .addProperty( new FullQualifiedName( "publicsafety.adult3-juv103" ) )
        .value( row -> row.getAs( "3-adult,103-juv" ) ).ok().ok()

        .addEntity( LOCATION_ALIAS )
        .ofType( LOCATION_ENTITY_SET_TYPE )
        .to( LOCATION_ENTITY_SET_NAME )
        .key( LOCATION_ENTITY_SET_KEY, LOCATION_ENTITY_SET_KEY2 )

        .addProperty( new FullQualifiedName( "general.city" ) )
        .value( row -> row.getAs( "city" ) ).ok()
        .addProperty( new FullQualifiedName( "general.address" ) )
        .value( row -> row.getAs( "location" ) ).ok()
        .addProperty( new FullQualifiedName( "general.apartment" ) )
        .value( row -> row.getAs( "apartment" ) ).ok()
        .addProperty( new FullQualifiedName( "publicsafety.pdzonestr" ) )
        .value( row -> row.getAs( "pd zone" ) ).ok()
        .addProperty( new FullQualifiedName( "publicsafety.pdbeat" ) )
        .value( row -> row.getAs( "pd beat" ) ).ok()

        .ok().ok()
        .createAssociations()

        .addAssociation( APPEARS_IN_ALIAS )
        .ofType( APPEARS_IN_ENTITY_SET_TYPE )
        .to( APPEARS_IN_ENTITY_SET_NAME )
        .key( APPEARS_IN_ENTITY_SET_KEY )
        .fromEntity( PEOPLE_ALIAS )
        .toEntity( CASE_ALIAS )
        .addProperty( new FullQualifiedName( "publicsafety.occdatetime" ) )
        .value( row -> standardizeDateTime( row.getAs( "occ year" ), row.getAs( "time occ" ) ) ).ok().ok()

        .addAssociation( OCCURRED_AT_ALIAS )
        .ofType( OCCURRED_AT_ENTITY_SET_TYPE )
        .to( OCCURRED_AT_ENTITY_SET_NAME )
        .key( OCCURRED_AT_ENTITY_SET_KEY )
        .fromEntity( CASE_ALIAS )
        .toEntity( LOCATION_ALIAS )
        .addProperty( new FullQualifiedName( "publicsafety.occdatetime" ) )
        .value( row -> standardizeDateTime( row.getAs( "occ year" ), row.getAs( "time occ" ) ) ).ok().ok().ok()
        .done();
```

**Common Error:** For each `row.getAs("column name")` make sure to match the spacing and capitalization exactly as it is written in your CSV. Apache Spark is case sensitive and will include whitespace in the column header string if it exists.

```text
Airport ID, Name, City, Country, IATA, ICAO...
```
The `Name` column in the example above would translate to: `row.getAs( " Name" )`

## 5. Create custom functions to parse your data

Sometimes, you may need to extract data from a column in your dataset.
For example, if your data had a `Name` column, but you wanted to use the `firstname` and `lastname` properties, you could write custom functions to parse the data contained in your `Name` column.

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
Loom stores dates as [DateTime ISO8601](http://joda-time.sourceforge.net/apidocs/org/joda/time/DateTime.html). If your dates are not already in this format, please use the provided `DateTimeFormatter.java` class to help parse any dates or times in your data:

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

## 6. Run your Integration

Use the command line to navigate to your integration project directory and use the `gradlew` command to run your integration script:

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
