---
layout: page
title: Data Integrations
description: Get started and integrate your data with Loom.
---

1. TOC
{:toc}

**Data integrations** take your data and map it to Loom's data model. Once you have completed this process, you'll be able to start linking and sharing your data with other members in your organization.

## Request Entity and Property Types

Before you can begin integrating your data, you'll need to submit a request to a Loom administrator to create Property and Entity Types that match your dataset schema.

> **How do I do this?** List your header types and descriptions in this [this spreadsheet](/files/DatasetColumnHeaderSubmission.xlsx) and email it to us at [{{site.email}}](mailto:{{site.email}}). A Loom administrator will complete this process and send you a confirmation email within 24 hours, along with further instructions.

## Add your datasource
The confirmation you receive will list the new Property and Entity Types created for your dataset. Once you receive this information, go to the **Datasources** tab and create your datasource using the email instructions.

{% include image.html caption="Create datasource in Loom" path="guides/integrations-create-datasource.png" %}

## Create an integration account

Next, you'll need login credentials to an account that has write access to your dataset. To do this, we recommend creating a new Loom account (only to be used for data integrations) and use these account credentials to perform the integration.

> **Why do I need to make a separate account?** For security reasons, we recommend creating a separate Loom account for your organization, and only granting write-access to your organization's account, and not to individual accounts.

Log out of your individual account and create a separate Loom account for your organization. *Save the credentials for this account in a safe place, since they will be used in your integration script.*

{% include related.html content="
* [Sign Up for a New Account](/guides/signups/)
" %}

## Manage Permissions

Log back into your individual account, go to **Catalog**, and browse for your Entity Set. Select **View Details** in the **Actions** menu.

{% include image.html caption="View Details Action Menu" path="guides/integrations-view-details.png" %}

Give your organization account write access through the **Manage Permissions** menu for the Entity Set and each of its properties.

{% include image.html caption="Manage permissions for Entity Set and Properties" path="guides/integrations-manage-permissions.png" %}

Search for your organization account's email address and grant it **WRITE** permissions to your datasource.

{% include image.html caption="Manage Permissions for Integration account" path="guides/integrations-write-permission.png" %}

{% include related.html content="
* [Setting and requesting permissions](/guides/permissions/)
" %}

## Install Java

The template integration script is written in [Java](https://java.com/en/download/help/index_installing.xml). You can check if Java is already installed on your system by typing `java -version` into the command line:

```shell
$ java -version
java version "1.8.0_121"
```

## Customize template integration script

The template project contains code for integrating a CSV of crime index ranking data for the largest cities in 2009. You can use this template script as a starting point for your own data integrations.

Download and unzip the [template integration script](/files/template.zip). Go ahead and replace the demo csv with your own.

```text
crime_index.csv (DEMO CSV DATA)
build.gradle
/gradle/wrapper
    gradle-wrapper.jar
    gradle-wrapper.properties
gradlew
gradlew.bat
run.sh
settings.gradle
/src/main/java/com/dataloom/integrations/exampleorg
    ExampleOrg.java
/src/main/java/resources
    jetty.yaml
    log4j.properties
    log4j2.properties
    rhizome.yaml
```

> **Note:** All instances of `exampleorg` refer to your organization name. Customizing this is not necessary, but if you choose to, make sure to match capitalization. For example: `ExampleOrg.java` would become `MyCustomName.java`

### Configure Gradle
[Gradle](https://gradle.org/) is a build tool used to configure and run your project. You only need to modify the following files:
* `build.gradle`
* `settings.gradle`
* `run.sh`

#### build.gradle

```gradle
// Change the description for your Entity Set
description = "Crime Index Data for Nation's Largest Cities"

// Modify mainClassName (if you renamed exampleorg)
mainClassName = "com.dataloom.integrations.exampleorg.ExampleOrg"

// Change to name of your csv file and organization account's login credentials
run.args = ["crime_index.csv","test@example.com","examplepassword"]
```

#### run.sh
```shell
# Change to name of your csv file and organization account's login credentials
./exampleorg-v0.0.1/bin/exampleorg ./crime_index.csv test@example.com examplepassword
```

#### settings.gradle
```gradle
// Optional: Change to match the name of your project's root directory
// If you didn't change this, it should still be called template
rootProject.name='template'
```

### Define your Integration

Start by verifying the package name at the top of `ExampleOrg.java`:

```java
// This should match file location of ExampleOrg.java file
// src/.../com/dataloom/integrations/exampleorg/ExampleOrg.java
package com.dataloom.integrations.exampleorg;
```

And, modify the name of the `ExampleOrg` class and `ExampleOrg.class` as needed.

```java
// Change ExampleOrg.class if you changed the organization name
// Logger is used to output useful debugging messages to the console
public class ExampleOrg {
  private static final Logger logger = LoggerFactory.getLogger( ExampleOrg.class );
```

Next, set the values of `ENTITY_SET_NAME`, `ENTITY_SET_TYPE`, and `ENTITY_SET_KEY`.
```java
// SET YOUR ENTITY NAME, TYPE, and PRIMARY KEY
public static String ENTITY_SET_NAME = "crimeindex";
public static FullQualifiedName ENTITY_SET_TYPE = new FullQualifiedName( "publicsafety.crimeindex" );
public static FullQualifiedName ENTITY_SET_KEY = new FullQualifiedName( "general.city" ); // or use "general.guid"
```

Finally, define all your property types.

```java
// SET YOUR PROPERTY TYPES
public static FullQualifiedName PT_YEAR = new FullQualifiedName( "general.year" );
public static FullQualifiedName PT_INDEX = new FullQualifiedName( "publicsafety.crimeindexranking" );
public static FullQualifiedName PT_CITY = new FullQualifiedName( "general.city" );
public static FullQualifiedName PT_RATE = new FullQualifiedName( "publicsafety.crimerate" );
```

### Using the Shuttle API

[Loom's Shuttle API](/api/) maps the columns in your CSV to their corresponding Loom property types and pushes the data to our servers. Your `Flight` code should be started for you, since you already defined your values at the top of the script:

```java
Flight flight = Flight.newFlight()
    .addEntity( ENTITY_SET_TYPE )
        .to( ENTITY_SET_NAME )
        .key( ENTITY_SET_KEY )
        // Add Properties here
    .ok()
.done();
```

Finally, replace the column and Property names in the template code with the  names given in your confirmation email:

```java
// "Year" is the column header from your CSV
.addProperty( PT_YEAR )
    .value(row -> row.getAs( "Year" )).ok()
```

<div class="caption">
Example: Full Flight code for crime index entity set and its properties
</div>

```java
Flight flight = Flight.newFlight()
    .addEntity( ENTITY_SET_TYPE )
        .to( ENTITY_SET_NAME )
        .key( ENTITY_SET_KEY )
        .addProperty( PT_YEAR )
            .value(row -> row.getAs( "Year" )).ok()
        .addProperty( PT_INDEX )
            .value( row -> row.getAs( "Crime Index Ranking" ) ).ok()
        .addProperty( PT_CITY )
            .value( row -> row.getAs( "City" ) ).ok()
        .addProperty( PT_RATE )
            .value( row -> row.getAs( "Rate" ) ).ok()
    .ok()
.done();
```
## Optional: Create custom functions

Sometimes, your data might need custom functions to parse data in your dataset. For example, if your data had a `Name` column, but you wanted to split `Name` into `firstname` and `lastname`, you could write custom functions to extract that data.

<div class="caption">
Example: Custom functions for parsing Name column
</div>

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

Congratulations! Once you've completed the integration, you'll be able to view and search your dataset through the Loom platform.

{% include image.html caption="Example: Search your dataset" path="/guides/integrations-search-screenshot.png" %}

{% include related.html content="
* [Setting and requesting permissions](/guides/permissions/)
* [Linking datasets](/guides/linking/)
" %}
