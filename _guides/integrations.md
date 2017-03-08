---
layout: page
title: How to integrate your data into Loom
description: Create a dataset and integrate your existing data.
weight: 1
---

1. TOC
{:toc}

## Create Property and Entity Types

**Data integrations** take your data and map it to Loom's data model. At this time, you need a Loom administrator to create these for you.

> **How do I submit a request?** List your column headers and descriptions in this [this spreadsheet](/files/DatasetColumnHeaderSubmission.xlsx) and email it to us at [{{site.email}}](mailto:{{site.email}}). An administrator will reply within 24 hours with further instructions.

## Add a new Dataset
Once you receive the property and entity types for your dataset, go to
the **Datasets** tab and create the dataset with the information from your email.

{%
  include image.html
  caption="Example: Creating a dataset for crime index dataset"
  path="guides/integrations/create-datasource.png"
%}

## Create an Integration account

You need an account with **write permissions** to add data to a dataset. At this
time, there are a couple of ways you could do this:

<span class="bad">Not Recommended:</span> _Dataset owners use their personal login_

> Owners have write permissions by default, but can also manage user permissions for a dataset, change the dataset owner, and even delete a dataset. Dataset owners _can_ use their personal login credentials to perform a data integration, but this is not recommended.

<span class="good">Recommended:</span> _Create a separate Integration Account_

> Create a new Loom account and use those credentials for your data integrations.
Integration Accounts are no different
than regular user accounts, except it will only be granted write permissions, not owner permissions, to your dataset. **Anyone with the login credentials to your Integration Account
will have permissions to add data to your dataset, so it's important to keep these credentials in a safe place.**

You can skip this step if you've performed a data integration and already have an
integration account.

## Grant your account Write permissions

To grant your integration account write permissions, log into your dataset owner account and go to **Datasets**. From the **Actions** menu, click **View Details**.

{%
  include image.html
  caption="View Details Action Menu"
  path="guides/integrations/view-details.png"
%}

Under **Manage Permissions** for the dataset and each of its properties, go to **Emails**, select **Write** permissions, and search for your integration account email.

{%
  include image.html
  caption="Manage permissions for Dataset and Properties" path="guides/integrations/manage-permissions.gif"
%}

{%
  include related.html
  content="
* [How to set, request, and grant/revoke permissions](/guides/permissions/)
* [How to sign up for a new account](/guides/signups/)
" %}

## Install Java

The template integration script is written in [Java](https://java.com/en/download/help/index_installing.xml). You can check if Java is already installed on your system by typing `java -version` into the command line:

```shell
$ java -version
java version "1.8.0_121"
```

## Download template integration script

We have provided you with a template integration script to help get you started. This integration
script was used to integrate the [Sample OpenFlights Airport Data](https://thedataloom.com/gallery/#/entitysets/514e6a5f-2bcd-4206-bb1c-448a9dbcf06d).

Download and unzip the [tutorial integration script](/files/tutorial.zip). Replace the demo CSV with your own data.

```text
airports.csv (DEMO CSV DATA)
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

### Configure Gradle
[Gradle](https://gradle.org/) is a build tool used to configure and run your project. You only need to modify the following files:
* `build.gradle`
* `settings.gradle`

**build.gradle:** Change the Entity Set `description` and `run.args`.
```gradle
// This should match the title of your dataset
description = "Sample OpenFlights Airport Data"

// This should match your CSV data and Integration Account credentials
run.args = ["airports.csv","test@example.com","examplepassword"]
```

**settings.gradle:** Change to match the name of your project's root directory. Default is `tutorial`.
```gradle
// This should match the name of your project's root directory
// If you didn't change this, keep it as 'tutorial'
rootProject.name='tutorial'
```

### Define your Integration

In `DataIntegration.java`, set the values of `ENTITY_SET_NAME`, `ENTITY_SET_TYPE`, and `ENTITY_SET_KEY`.
```java
// Set your Entity Set Name, Type, and Primary Key
public static String ENTITY_SET_NAME = "openflightsdata";
public static FullQualifiedName ENTITY_SET_TYPE = new FullQualifiedName( "sample.openflightsdata" );
public static FullQualifiedName ENTITY_SET_KEY = new FullQualifiedName( "aviation.id" );
```

### Use Shuttle API

[Loom's Shuttle API](/api/) will complete the mapping and push your data to
Loom's servers. Your `Flight` code should be started for you, since you already
defined your values at the top of the script:

```java
Flight flight = Flight.newFlight()
    .addEntity( ENTITY_SET_TYPE )
        .to( ENTITY_SET_NAME )
        .key( ENTITY_SET_KEY )
        // Add Properties here
    .ok()
.done();
```

Properties are constructed using the following format:

```java
new FullQualifiedName( "aviation.name" )
```

Just pass the constructor for each property into `.addProperty()`:

```java
.addProperty( new FullQualifiedName( "aviation.name" ) )
    .value( row -> row.getAs( " Name" ) ).ok()
```

Notice how `row -> row.getAs( " Name" )` has a space before `" Name"`. If your
CSV has spaces in between the column headers like this:

```text
Airport ID, Name, City, Country, IATA, ICAO...
```

You will need to include the spaces in the column header for `row.getAs()` in order for the data to be properly parsed.

## Full Flight Code

Here is an example of the full `Flight` code from the tutorial:

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
## Optional: Create custom functions

Sometimes, your data might need custom functions to parse data in your dataset.
For example, if your data had a `Name` column, but you wanted to split `Name`
into `firstname` and `lastname`, you could write custom functions to extract that
data.

Here is an example of two custom functions used to parse a `Name` field into
`firstname` and `lastname`.

```java
// CUSTOM FUNCTIONS DEFINED BELOW
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

## Run your Integration

Using the command line, navigate to the template project directory, and use the following commands to run your script using Gradle:

**Mac/Linux:**
```
$ ./gradlew run
```

**Windows:**
```
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
