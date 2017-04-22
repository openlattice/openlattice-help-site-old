---
layout: page
title: Integrations Part 2 – Integration Scripts
description: Customize our provided template integration scripts. This guide is recommended for individuals who are completing data integrations. Reach out to help@thedataloom.com if you need any assistance with your integration.
weight: 1
---

<div style="color:black; border: 1px solid black; padding: 10px; background-color: yellow; border-radius:5px; text-align: center;">This guide is currently being updated for our new version of Shuttle API. Please email <a href="mailto:help@thedataloom.com">help@thedataloom.com</a> with any comments or feedback.</div><br>

* TOC
{:toc}


## 1. Set up your programming environment

The integration script we provide requries [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) to run. We also recommend that you use a programming text editor to edit the template integration files we provide you:

* [Notepad++](https://notepad-plus-plus.org/)
* [Atom](https://atom.io/)
* [Sublime Text 3](https://www.sublimetext.com/)


## 2. Download template integration script

Feel free to use our template integration script to get started. This integration
script was used to integrate the [Sample Jail](https://thedataloom.com/gallery/#/entitysets/514e6a5f-2bcd-4206-bb1c-448a9dbcf06d) datasets.

Download and unzip the [template project](/files/tutorial.zip). The folder contents should look like this:

```text
fake_jail_data.csv (Replace with your CSV)
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

## 3. Add login credentials and CSV filename to build.gradle

[Gradle](https://gradle.org/) is a build tool you will use to run the integration script. `build.gradle` contains important information about project dependencies, API versions, and other details. The piece you will need to update is the CSV filename and login credentials for the individual performing the data integration. Update the following:

**build.gradle**
```gradle
run.args = ["fake_jail_data.csv","test@example.com","examplepassword"]
```

The login credentials will be used in `DataIntegration.java` to retrieve your `jwtToken`. This token will then be used to verify whether the login credentials provided have been given write access to the dataset. 

**In: `/src/main/java/com/dataloom/integrations/dataintegration/DataIntegration.java`**

```java
// Get CSV path, username, and password
final String path = args[0];
final String username = args[1];
final String password = args[2];

// Get jwtToken to verify data integrator has write permissions to dataset
final String jwtToken = MissionControl.getIdToken( username, password );
logger.info( "Using the following idToken: Bearer {}", jwtToken );
```

## 4. Configure Apache Spark

In `DataIntegration.java`, there will be some code that tells Apache Spark what format your datasource is in:

```java
// Configure Spark to load and read your datasource
final SparkSession sparkSession = MissionControl.getSparkSession();
Dataset<Row> payload = sparkSession
        .read()
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

### Entity Types, Associations, and Properties

Entity Types are like schemas for your datasets. They represent how your data will be formatted and what fields they will have. Using the data model details provided to you by your Loom Administrator, update the Entity Set Names, Types, Keys, and Aliases defined at the top of your script. 

```java
// Entity Set (Dataset) and Entity Type Details
public static String            PEOPLE_ENTITY_SET_NAME = "samplejailsubjects";
public static FullQualifiedName PEOPLE_ENTITY_SET_TYPE = new FullQualifiedName( "sample.person" );
public static FullQualifiedName PEOPLE_ENTITY_SET_KEY1  = new FullQualifiedName( "general.firstname" );
public static FullQualifiedName PEOPLE_ENTITY_SET_KEY2  = new FullQualifiedName( "general.lastname" );
public static String            PEOPLE_ALIAS           = "people";

public static String            BOOKINGS_ENTITY_SET_NAME = "samplejailbookings";
public static FullQualifiedName BOOKINGS_ENTITY_SET_TYPE = new FullQualifiedName( "sample.bookings" );
public static FullQualifiedName BOOKINGS_ENTITY_SET_KEY1  = new FullQualifiedName( "publicsafety.datebooked" );
public static FullQualifiedName BOOKINGS_ENTITY_SET_KEY2  = new FullQualifiedName( "publicsafety.datereleased" );
public static String            BOOKINGS_ALIAS           = "bookings";

public static String            WAS_BOOKED_IN_ENTITY_SET_NAME = "samplejailsubjectappearsin";
public static FullQualifiedName WAS_BOOKED_IN_ENTITY_SET_TYPE = new FullQualifiedName( "sample.subjectappearsin" );
public static FullQualifiedName WAS_BOOKED_IN_ENTITY_SET_KEY  = new FullQualifiedName( "publicsafety.bookingid" );
public static String            WAS_BOOKED_IN_ALIAS           = "appearsin";
```

And define your dataset properties:

```java
// Properties
public static FullQualifiedName LAST_NAME_FQN    = new FullQualifiedName( "general.lastname" );
public static FullQualifiedName FIRST_NAME_FQN   = new FullQualifiedName( "general.firstname" );
public static FullQualifiedName HOME_ADDRESS_FQN = new FullQualifiedName( "general.homeaddress" );
public static FullQualifiedName RACE_FQN         = new FullQualifiedName( "general.race" );
public static FullQualifiedName DOB_FQN          = new FullQualifiedName( "general.dob" );

public static FullQualifiedName DATE_BOOKED_FQN          = new FullQualifiedName( "publicsafety.datebooked" );
public static FullQualifiedName BOOKING_ID_FQN       = new FullQualifiedName( "publicsafety.bookingid" );
public static FullQualifiedName DATE_RELEASED_FQN      = new FullQualifiedName( "publicsafety.datereleased" );
```

Notice that even though you may be integrating only 1 table, you could have multiple entity types that represent your dataset. For example, this example dataset has 2 different entity types and 1 association that relates the entity types together:

| Entity Type | Properties included in this type                                                            |
|-------------|---------------------------------------------------------------------------------------------|
| Person      | First name, Last name, Address, Race, Date Of Birth |
| Bookings    | Date Booked, Date Released                                                  |
| Appears In  | Booking ID                                                |

### Create Your Flight Path

Loom's [Shuttle API](/api/) maps your data to the Loom data model and uploads
your data to Loom's servers. Replace the properties and column names with the values that correspond with your dataset.

**Full Flight code:**

```java
// Each flight stores data from 1 table or CSV
// Add each flight to flights to integrate data from multiple CSVs or tables
Map<Flight, Dataset<Row>> flights = Maps.newHashMap();
Flight flight = Flight.newFlight()
        .createEntities()

        .addEntity( PEOPLE_ALIAS )
        .ofType( PEOPLE_ENTITY_SET_TYPE )
        .to( PEOPLE_ENTITY_SET_NAME )
        .key( PEOPLE_ENTITY_SET_KEY1, PEOPLE_ENTITY_SET_KEY2 )
        .addProperty( LAST_NAME_FQN )
        .value( row -> row.getAs( "Last Name" ) ).ok()
        .addProperty( FIRST_NAME_FQN )
        .value( row -> row.getAs( "First Name" ) ).ok()
        .addProperty( HOME_ADDRESS_FQN )
        .value( row -> row.getAs( "Address" ) ).ok()
        .addProperty( RACE_FQN )
        .value( row -> row.getAs( "Race" ) ).ok()
        .addProperty( DOB_FQN )
        .value( row -> standardizeDate2( row.getAs( "DOB" ) ) ).ok().ok()

        .addEntity( BOOKINGS_ALIAS )
        .ofType( BOOKINGS_ENTITY_SET_TYPE )
        .to( BOOKINGS_ENTITY_SET_NAME )
        .key( BOOKINGS_ENTITY_SET_KEY1, BOOKINGS_ENTITY_SET_KEY2 )
        .addProperty( DATE_BOOKED_FQN )
        .value( row -> standardizeDate( row.getAs( "Date Booked" ) ) ).ok()
        .addProperty( DATE_RELEASED_FQN )
        .value( row -> standardizeDate( row.getAs( "Date Released" ) ) ).ok()

        .ok().ok()
        .createAssociations()

        .addAssociation( WAS_BOOKED_IN_ALIAS )
        .ofType( WAS_BOOKED_IN_ENTITY_SET_TYPE )
        .to( WAS_BOOKED_IN_ENTITY_SET_NAME )
        .key( WAS_BOOKED_IN_ENTITY_SET_KEY )
        .fromEntity( PEOPLE_ALIAS )
        .toEntity( BOOKINGS_ALIAS )
        .addProperty( BOOKING_ID_FQN )
        .value( row -> row.getAs( "Booking ID" ) ).ok().ok().ok()
        .done();
```

Note that the example code corresponds to 1 flight. You could integrate multiple tables (and add multiple flights) in one integration.

```java
// At this point, your flight contains 1 table's worth of data
// If you want to integrate more tables, create another flight (flight2) and
// add the flight to flights
flights.put( flight, payload );

// Send your flight plan to Shuttle and complete your integration!
Shuttle shuttle = new Shuttle( Environment.PRODUCTION, jwtToken );
shuttle.launch( flights );
```

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
