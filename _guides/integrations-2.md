---
layout: page
title: Integrations Part 2 – Integration Scripts
description: Customize our provided template integration scripts. This guide is recommended for individuals who are completing data integrations. Reach out to help@thedataOpenLattice.com if you need any assistance with your integration.
weight: 1
---

<div style="color:black; border: 1px solid black; padding: 10px; background-color: yellow; border-radius:5px; text-align: center;">This guide is currently being updated for our new version of Shuttle API. Please email <a href="mailto:help@thedataOpenLattice.com">help@thedataOpenLattice.com</a> with any comments or feedback.</div><br>

* TOC
{:toc}


## 1. Set up your programming environment

The integration script we provide requries [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) to run. We also recommend that you use a programming text editor to edit the template integration files we provide you:

* [Notepad++](https://notepad-plus-plus.org/)
* [Atom](https://atom.io/)
* [Sublime Text 3](https://www.sublimetext.com/)


## 2. Download template integration script

Feel free to use our template integration script to get started. This integration
script was used to integrate the [Sample Jail](https://thedataOpenLattice.com/gallery/#/entitysets/514e6a5f-2bcd-4206-bb1c-448a9dbcf06d) datasets.

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
/src/main/java/com/openlattice/integrations/dataintegration
    DataIntegration2017.java
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

**In: `/src/main/java/com/openlattice/integrations/dataintegration/DataIntegration2017.java`**

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

In `DataIntegration2017.java`, there will be some code that tells Apache Spark what format your datasource is in:

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

In `DataIntegration2017.java` you will also need to define the entity types, relationships, and properties for your data.                                          |

### Create Your Flight Path

OpenLattice's [Shuttle API](/api/) maps and uploads your data to OpenLattice's servers. What we call the 'flight' lays out a recipe for translating your data tables, column by column, onto objects in our data model. 

Entity Types are like schemas for your datasets. They represent how your data will be formatted and what fields they will have. Notice that even though you may be integrating only 1 table, you could have multiple entity types that are represented in that table. For example, this example dataset has 3 different entity types and 2 associations that relates the entity types together:

| Entity Type | Properties included in this type                                                            |
|-------------|---------------------------------------------------------------------------------------------|
| Person      | First name, Last name, Race, Date Of Birth |
| Address     | Street Address |
| Bookings    | Date Booked, Date Released                                                  |
| Booked In   | Booking ID  |
| Lives At    | Address |    

Before writing your flight, it may be useful to first write out how your data maps on to the various OpenLattice Entity Types and Properties, in a similar table as what we show above. Using either the data model details provided to you by your OpenLattice Administrator or your own interpretation of your dataset and the Entity Types that it maps to in the OpenLattice data model, found [here](https://staging.openlattice.com/edm/#/entityTypes), fill in the Entity Set Names and Aliases, and replace the properties and column names with the values that correspond with your dataset.

**Full Flight template code:**

```java
// Each flight stores data from 1 table or CSV
// Add each flight to flights to integrate data from multiple CSVs or tables
Map<Flight, Dataset<Row>> flights = Maps.newHashMap();
Flight flight = Flight.newFlight()
        .createEntities()
                    .addEntity( "PEOPLE_ALIAS" )      //variable name within flight. Doesn't have to match anything anywhere else.
                        .to( "PEOPLE_ENTITY_SET_NAME" )       //name of entity set created in step 2 of tutorial, 'Integratons Part I'.
                            .addProperty( "LAST_NAME_FQN", "LAST_NAME_COL" )
                            .addProperty( "FIRST_NAME_FQN", "FIRST_NAME_COL" )
                            .addProperty( "RACE_FQN", "RACE_COL" )
                            .addProperty( "DOB_FQN") 
                                .value( row -> bdHelper.parse( row.getAs( "DOB" ) ) ) .ok()
                        .endEntity()
                    addEntity( "ADDRESS_ALIAS" )
                            .to( "ADDRESS_ENTITY_SET_NAME" )
                            .addProperty( "STREET_ADDRESS_FQN", "ADDRESS_COL" )
                            .endEntity()
                    .addEntity( "BOOKINGS_ALIAS" )
                            .to( "BOOKINGS_ENTITY_SET_NAME" )
                            .addProperty( "DATE_BOOKED_FQN" )
                                .value( row -> bdHelper.parse( row.getAs( "Date Booked" ) ) ).ok()
                            .addProperty( DATE_RELEASED_FQN )
                                .value( row -> bdHelper.parse( row.getAs( "Date Released" ) ) ).ok()
                            .endEntity()
                .endEntities()                
                
                .createAssociations()
                    .addAssociation( "LIVES_AT_ALIAS" )
                        .to( "LIVES_AT_IN_ENTITY_SET_NAME" )
                        .fromEntity( "PEOPLE_ALIAS" )
                        .toEntity( "ADDRESS_ALIAS" )
                        .endAssociation()
                    .addAssociation( "WAS_BOOKED_IN_ALIAS" )
                        .to( "WAS_BOOKED_IN_ENTITY_SET_NAME" )
                        .fromEntity( "PEOPLE_ALIAS" )
                        .toEntity( "BOOKINGS_ALIAS" )
                        .addProperty( "BOOKING_ID_FQN", "BOOKING_ID_COL" )
                        .endAssociation()
                .endAssociations()
                
                    .done();
```

**How to customize your flight**

Let's walk through how one would modify the above template for our sample dataset. Within the first `.addEntity` command, one would replace `"PEOPLE_ENTITY_SET_NAME"` with the name of the Entity Dataset that you created in step 2 of the first Integration tutorial ("Create Your Dataset"). Here, I simply call the entity alias `people` and let's assume that we previously created an Entity Dataset called `FakeJailPeople` to house the people in our data. Then, search for the "person" Entity in [OpenLattice's data model](https://staging.openlattice.com/edm/#/entityTypes) and replace `"LAST_NAME_FQN"` with fully qualified name of this Entity Type Property. At present, the FQN would be `nc.PersonSurName`. Finally, replace `"LAST_NAME_COL"` with the name of the column in *your* dataset that contains last names. In our sample jail data, this column is "Last Name". The finished java code for the first entity would thus look like this:

```
.addEntity( "people" )      
    .to( "FakeJailPeople" )       
        .addProperty( "nc.PersonSurName", "Last Name" )
        .addProperty( "nc.PersonGivenName", "First Name" )
        .addProperty( "nc.PersonRace", "Race" )
        .addProperty( "nc.PersonBirthDate") .value( row -> bdHelper.parse( row.getAs( "DOB" ) ) ) .ok()
.endEntity()
```

(You may notice that the java code for date of birth is different. We will address that below shortly!)

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

## 6. Create custom functions to parse your data

If any of your columns are not meant to be simple text fields, then the java code above needs just a tad more love.  This is true for instance, for any fields that are numbers or dates or in various other situations, such as if you need to map only part of a column to an OpenLattice Entity or Property. For example, if your data had a `Name` column containing full names like `John Doe`, but you wanted to separate out `firstname` and `lastname` properties, you could write custom functions to parse the data contained in your `Name` column. A few examples of ways to deal with these situations are below. Simply paste the custom functions above or below your flight code in Shuttle (see the sample java file in the tutorial materials for examples).

### Date Formatting
OpenLattice stores dates as [DateTime ISO8601](http://joda-time.sourceforge.net/apidocs/org/joda/time/DateTime.html). If your dates are not already in this format, please use the provided `DateTimeHelper.java` class to help parse any dates or times in your data. Modify the y/m/d code below to match the format in your own dataset:

**Example Implementation:**
```java
// Custom Functions for Standardizing Dates as JODA format
//the Offset hours are offset from UTC.
private static final DateTimeHelper bdHelper = new DateTimeHelper(DateTimeZone
            .forOffsetHours(-4), "YYYY-MM-dd");
```

```java
// Corresponding Flight Code
.addProperty( "nc.PersonBirthDate") 
    .value( row -> bdHelper.parse( row.getAs( "DOB" ) ) ) .ok()
```

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

### Number Parsing

You may have to convert a number, stored as a string in your .csv file, to a number field in the OpenLattice model (i.e., sentence term times). Here is an example of a custom function called `parseNumber` that allows one to do so:

```java
public static Integer parseNumber( String num ) {
        if (num == null) return null;
        try {
            Double d = Double.parseDouble( num );
            return d.intValue();
        } catch (NumberFormatException e) {}

        try {
            Integer i = Integer.parseInt( num );
            return i;
        } catch ( NumberFormatException e) {}

        return null;
    }
```

Then for a data column called `sentence term yrs` for instance, one would add this to your flight as a property like so:

```java
// Corresponding Flight Code
.addProperty(  "event.SentenceTermYears" )
    .value( row -> parseNumber( row.getAs( "sentence term yrs" ) ) .ok()
```


## 7. Run your Integration

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
on OpenLattice.

{%
  include related.html
  content="
* [Setting and requesting permissions](/guides/permissions/)
* [Linking datasets](/guides/linking/)
"
%}
