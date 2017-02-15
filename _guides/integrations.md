---
layout: page
title: Data Integrations
description: Get started and integrate your data with Loom.
---

1. TOC
{:toc}

**Data integrations** take your data and map it to Loom's data model. Once you have completed this process, you'll be able to start linking and sharing your data with other members in your organization.

## Request Entity and Property Types

Before you can begin integrating your data, you'll need to submit a request to create Property and Entity Types in Loom that match your dataset schema.

> **How do I do this?** List your data header types in this [this spreadsheet](/files/DatasetColumnHeaderSubmission.xlsx) and email it to us at [{{site.email}}](mailto:{{site.email}}). An administrator will create your Property and Entity Types for you and send you a confirmation email within 24 hours, along with further instructions.

## Add your datasource
Your confirmation email will list the Property and Entity Types we created for you. Once you receive this information, go to the **Datasources** tab and create your datasource using the details in the email.

{% include image.html caption="Create datasource in Loom" path="guides/integrations-create-datasource.png" %}

## Create an integration account

Next, you'll need to give an account write-access to the datasource you created and use those account credentials to perform the integration. **For security reasons, we recommend creating a separate Loom account for your organization, and only grant write-access to your organization's account, and not to individual accounts**.

Log out and create a separate Loom account for your organization. *This organization account will be used to integrate your data. Save the credentials for this account in a safe place.*

{% include image.html caption="Create an integration account" path="guides/integrations-login.png" %}

Once you have login credentials for your organization, log back into your individual account and go to the **Catalog tab**. Browse for your Entity Set by selecting its Entity Type.

{% include image.html caption="Search by Entity Type" path="guides/integrations-search-entity-type.png" %}

Then, go to **Actions > View Details > Manage Permissions > EMAILS** and search for your organization account's email address, and grant it **WRITE** permissions to your datasource.

{% include image.html caption="Manage Permissions for Integration account" path="guides/integrations-write-permission.png" %}

## Customize template directory

The integration file will be written in **Java**. You will need Java and a Java IDE such as **IntelliJ** installed.

* [Install Java](https://java.com/en/download/help/index_installing.xml)
* [Install IntelliJ](https://www.jetbrains.com/idea/download/)

Next, download and unzip the [template integration project](/files/template.zip):

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

| Template File     | Your File       |
|-------------------|-----------------|
| `ExampleOrg.java` | `DataLoom.java` |
| `/exampleorg`     | `/dataloom`     |

<br>
Once you're ready, **import the template project into IntelliJ** and let's begin editing the gradle files to build and run this integration.

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
```shell
# Add the name of your csv file and organization account's login credentials
./exampleorg-v0.0.1/bin/exampleorg ./crime_index.csv test@example.com examplepassword
```

#### settings.gradle
```shell
# Optional: Change to match the name of your project's root directory
# If you didn't change this, it should still be called template
rootProject.name='template'
```

### Define your Integration

The code in `ExampleOrg.java` will define your data integration. Start by changing the package name at the top of the file, so it matches with the location of ExampleOrg.java:

```java
// This should match file location of ExampleOrg.java file
package com.dataloom.integrations.exampleorg;
```

Modify `ExampleOrg.class` if needed, and set the `ENTITY_SET_NAME` to the Entity Set name from your confirmation email. Loom uses `Logger` and `LoggerFactory` to output useful log messages during the integration.

```java
// Change ExampleOrg.class if you changed the organization name
public class ExampleOrg {
  private static final Logger logger = LoggerFactory.getLogger( ExampleOrg.class );
```

```java
// Set to Entity Set Name given in your confirmation email
public static String ENTITY_SET_NAME = "crimeindex";
```

### Using the Flight & Shuttle APIs

The **Flight API** defines the Property and Entity Set mappings, and the **Shuttle API** pushes your data to our servers. Create a new `Flight`, add your Entity Set, and specify your primary key.

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
## Optional: Create custom functions

Sometimes, your data might need custom functions to parse data in your dataset. For example, if your data had a **Name** column, but you wanted to split **Name** into **firstname** and **lastname**, you could write custom functions to extract that data.

<div class="caption">
Example: Custom functions for parsing Name column
</div>

```java
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

TODO: With IntelliJ

## Set/Request permissions

Congratulations! Once you've completed the integration, you can go ahead and set the permissions, so only accepted members can view the data.

* [Learn how to set and request permissions in Loom](/guides/permissions/)

{% include construction.html %}
