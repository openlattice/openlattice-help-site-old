---
layout: page
title: How to integrate your data
description: Get started integrating your data in Loom.
---
**This guide is under construction. Please email [{{site.email}}](mailto:{{site.email}}) with any questions or feedback.**

<hr><br>

1. TOC
{:toc}

**Data integrations** take your data and map it to Loom's data model. Once you have completed this process, you'll be able to start linking and sharing your data with other members in your organization.

## Request Entity and Property Types

First, submit a request to create Property and Entity Types in Loom that match your dataset schema.

List your data header types in this [this spreadsheet](/files/DatasetColumnHeaderSubmission.xlsx) and email it to us at [{{site.email}}](mailto:{{site.email}}) and an administrator will create your Property and Entity Types for you.

**We will send you a confirmation email within 24 hours once the types have been created, along with further instructions.**

## Add your datasource
Your confirmation email will list the Property and Entity Types we created for you. Once you receive this information, go to the **Datasources** tab and create your datasource using the details in the email.

<div class="flex-container">
  <div class="item">
  <div class="caption">Create datasource in Loom</div>
  <img src="/assets/guides/integrations-create-datasource.png" alt="Create datasource in Loom">
  </div>
</div>

## Create an integration account

Next, you'll need to give an account write-access to the datasource you created and use those account credentials to perform the integration. **For security reasons, we recommend creating a separate Loom account for your organization, and only grant write-access to your organization's account, and not to individual accounts**.

Log out and create a separate Loom account for your organization. *This organization account will be used to integrate your data. Save the credentials for this account in a safe place.*

<div class="flex-container">
  <div class="item">
  <div class="caption">Create an integration account</div>
  <img src="/assets/guides/integrations-login.png" alt="Create an integration account">
  </div>
</div>

Once you have login credentials for your organization, log back into your individual account and go to the **Catalog tab. Browse for your Entity Set by selecting its Entity Type**.

<div class="flex-container">
  <div class="item">
  <div class="caption">Search by Entity Type</div>
  <img src="/assets/guides/integrations-search-entity-type.png" alt="Search by Entity Type">
  </div>
</div>

Then, go to **Actions > View Details > Manage Permissions > EMAILS** and search for your organization account's email address, and grant it **WRITE** permissions to your datasource.

<div class="flex-container">
  <div class="item">
  <div class="caption">Manage Permissions for Integration account</div>
  <img src="/assets/guides/integrations-write-permission.png" alt="Manage Permissions for Integration account">
  </div>
</div>

## Customize template directory

The integration file will be written in **Java**. You will need Java and a Java IDE such as **IntelliJ** installed.

* [Install Java](https://java.com/en/download/help/index_installing.xml)
* [Install IntelliJ](https://www.jetbrains.com/idea/download/)

Next, download and unzip the [template integration project](http://google.com/):

```text
build.gradle
gradle/wrapper
	gradle-wrapper.jar
	gradle-wrapper.properties
gradlew
gradlew.bat
run.sh
settings.gradle
src/main/java/com/dataloom/integrations/exampleorg
	ExampleOrg.java
src/main/java/resources
	jetty.yaml
	log4j.properties
	log4j2.properties
	rhizome.yaml
```

The template project contains integration code for an example organization (**exampleorg**) to integrate a CSV of crime index ranking data. Feel free to change all instances of `exampleorg` to your organization name, but this step is not necessary.

<div class="caption">
Optional: Rename example organization to custom name
</div>

| Template File     | Your File       |
|-------------------|-----------------|
| `ExampleOrg.java` | `DataLoom.java` |
| `/exampleorg`     | `/dataloom`     |

<br>
Go ahead and **import the template project into IntelliJ**.

### Gradle
Gradle is a build tool that you need to configure for your project. You will need to make changes to the following files:
* `build.gradle`
* `settings.gradle`
* `run.sh`

#### build.gradle

```java
// Change the description for your Entity Set
description = "Crime Index Data for Nation's Largest Cities"

// Modify mainClassName (if you renamed exampleorg)
mainClassName = "com.dataloom.integrations.exampleorg.ExampleOrg"

// Add the name of your csv file and organization account's login credentials
run.args = ["crime_index.csv","test@example.com","examplepassword"]
```

#### run.sh
```
// Add the name of your csv file and organization account's login credentials
./exampleorg-v0.0.1/bin/exampleorg ./crime_index.csv test@example.com examplepassword
```

#### settings.gradle
```
// Optional: Change to match the name of your project's root directory
// If you didn't change this, it should still be called template
rootProject.name='template'
```

## Define your Integration

The code in `ExampleOrg.java` will define your data integration. Start by changing the package name at the top of the file, so it matches with the location of ExampleOrg.java:

```java
// This should match file location of ExampleOrg.java file
package com.dataloom.integrations.exampleorg;
```

Modify `ExampleOrg.class` if needed, and set the `ENTITY_SET_NAME` to the Entity Set name from your confirmation email. Loom uses `Logger` and `LoggerFactory` to output useful log messages during the integration.

```java
// Change ExampleOrg.class if you changed the organization name
private static final Logger logger = LoggerFactory.getLogger( ExampleOrg.class );

// Set to Entity Set Name given in your confirmation email
public static String ENTITY_SET_NAME = "crimeindex";
```

### Flight & Shuttle APIs

The **Flight API** defines the Property and Entity Set mappings, and the **Shuttle API** pushes your data to Loom's servers. Create a new `Flight`, add your Entity Set, and specify your primary key.

If your dataset had a primary key defined, specify the name of the Property that corresponds to the primary key.

```java
Flight flight = Flight.newFlight()
  .addEntity().to( ENTITY_SET_NAME )
  .as( new FullQualifiedName( "publicsafety.crimeindex" ) )
  .key( new FullQualifiedName( "general.city" ) )

// ... add properties below
```

If your dataset did not originally have a primary key defined, use **GUID** as your key.

```java
Flight flight = Flight.newFlight()
  .addEntity().to( ENTITY_SET_NAME )
  .as( new FullQualifiedName( "publicsafety.crimeindex" ) )
  .key( new FullQualifiedName( "general.guid" ) )

//... add properties below
```

Replace the Property names in the template code with the Property names given in your confirmation email:

```java
// Adds a property and sets the value of the property to a function that
// takes in a row and gets the value of "Rate" for that row
.addProperty()
.value( row -> row.getAs( "Rate" ) )
.as( new FullQualifiedName( "publicsafety.rate" ) )
.ok()
```

<div class="caption">
Example: Full Flight code for crime index entity set and its properties
</div>

```java
Flight flight = Flight.newFlight()
  .addEntity().to( ENTITY_SET_NAME )
  .as( new FullQualifiedName( "publicsafety.crimeindex" ) )
  .key( new FullQualifiedName( "general.city" ) )
  .addProperty()
  .value( row -> row.getAs( "Year") )
  .as( new FullQualifiedName( "general.year" ) )
  .ok()
  .addProperty()
  .value( row -> row.getAs( "Crime Index Ranking" ) )
  .as( new FullQualifiedName( "publicsafety.crimeindexranking" ) )
  .ok()
  .addProperty()
  .value( row -> row.getAs( "City" ) )
  .as( new FullQualifiedName( "general.city" ) )
  .ok()
  .addProperty()
  .value( row -> row.getAs( "Rate" ) )
  .as( new FullQualifiedName( "publicsafety.rate" ) )
  .ok()
  .ok()
  .done();
```
