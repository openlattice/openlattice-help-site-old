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

The template project contains integration code for an example organization (**exampleorg**) to integrate a CSV of crime index ranking data. Feel free to change all instances of exampleorg to your organization name, but this step is not necessary.

<div class="caption">
Optional: Rename example organization to custom name
</div>

| Template File     | Your File       |
|-------------------|-----------------|
| `ExampleOrg.java` | `DataLoom.java` |
| `/exampleorg`     | `/dataloom`     |

<br>
Go ahead and **import the template project into IntelliJ**.
