---
layout: page
title: Integrations Part 1 – Data Model & Permissions
description: Create a new dataset and configure permissions for your data integration. This guide is recommended for dataset owners who are preparing datasets for new integrations.
weight: 1
---

* TOC
{:toc}

<div style="color:black; border: 1px solid black; padding: 10px; background-color: yellow; border-radius:5px; text-align: center;">Resources used in this tutorial: 
<a href="files/tutorial_11-2-17.zip">Sample Jail Dataset</a></div>

## 1. Create your data model

**Data integrations** take your data and map it to OpenLattice's standardized data model. You will need to submit a request to a OpenLattice administrator to create the data model for your dataset.

> **How do I submit a request?** List your column headers, descriptions, and data types in this [this spreadsheet](/files/DatasetColumnHeaderSubmission.xlsx) and email it to us at [{{site.email}}](mailto:{{site.email}}). Please provide as many details as you can in the spreadsheet. We will contact you if further information is needed and will send you the details for your data model within 24 hours.

## 2. Create your dataset

**Note: You can skip this step if a member of the OpenLattice team already created the required datasets for you.**

OpenLattice's database can be understood as a [graph database](https://en.wikipedia.org/wiki/Graph_database), which stores data as nodes, properties, and edges. All three are objects, or `entities`. In our model nodes are commonly what one might think of as nouns: people, addresses, arrests, or health records. Edges connect nodes similar to the way that verbs connect nouns in a sentence. 'People' can 'appear in' a police 'incident', or 'live at' an 'address'. 'Appear in' and 'lives at' are both edges, or what we call `associations`. Both nodes and edges have user-defined `properties` such as dates, IDs, and other details. 

{%
  include image.html
  caption="Draft of part of OpenLattice's graph database, as applied to a healthcare patient screening assessment. . Entities are represented as boxes and associations as diamonds. The figure illustrates how representations of a single individual can be separated based on the associated properties (e.g., person vs. patient) to enable selective, restricted viewing of specific properties once records are linked."
  path="guides/integrations/graph_database2.jpg"
%}

OpenLattice's __Entity Data Model (EDM)__ is a blueprint meant to standardize data from all jurisdictions and counties on our platform, consisting of `entity types` (i.e., nodes), `property types`, and `associations` (i.e. edges). One can think of the EDM as the blueprint of the house you want to build, and your Entity Datasets as the houses you have built using the blueprint.  For each entity type in your dataset, an Entity Dataset must be created. For example, our [Sample Jail](files/tutorial_11-2-17.zip)  dataset is one flat file, but has 3 different entity types and 2 associations that relates the entity types together:

| Entity Type | Properties included in this type                                                            |
|-------------|---------------------------------------------------------------------------------------------|
| Person      | First name, Last name, Race, Date Of Birth |
| Address     | Street Address |
| Jail Stay   | Date Booked, Date Released                                                  |
| Booked In   | Booking ID  |
| Lives At    | Address |    


Using either the data model details provided to you by your OpenLattice Administrator or your own interpretation of your dataset and the Entity Types that it maps to in the OpenLattice data model, found [here](https://staging.openlattice.com/edm/#/entityTypes), identify the Entity Sets you need and create them in OpenLattice's gallery dashboard. Go to the **Datasets** tab and create the new datasets with the information provided. 

{%
  include image.html
  caption="Example: Creating a dataset of court cases in Anytown, USA"
  path="guides/integrations/create-datasource.png"
%}

## 3. Identify who will perform the data integration

In order for a data integration to be completed, OpenLattice verifies that the credentials provided correspond to an individual who has been granted write permissions to the dataset. 

**Identify a trusted member of your team (most likely the individual who is currently managing your data) and grant them write permissions to your new dataset. Dataset owners can also perform data integrations themselves (as long as they posess the username and password for the account), but it is advised to use an account with _limited_ permissions to perform the data integration.**

<span class="bad">Not Recommended:</span> _Dataset owners use their own accounts to perform data integrations_

> Dataset owners have read, write, and link permissions to datasets they create. However, they also have administrative permissions such as adding and removing permissions from users and other dataset owners. Dataset owners _can_ use their personal login credentials to perform a data integration, but this is not advised.

<span class="good">Recommended:</span> _Dataset owners use a separate account with limited permissions to perform data integrations_

> Create a separate OpenLattice account with a chosen username and password (not Google Auth) and use those credentials for data integrations. Or, have a member of your team (someone who currently manages your data in your organization) perform the data integration for you using their own account.

## 4. Configure permissions

To configure permissions for your dataset, go to the **Actions** menu and select **View Details**.

{%
  include image.html
  caption="View Details Action Menu"
  path="guides/integrations/view-details.png"
%}

For the dataset _and_ each of its properties, go to **Manage Permissions > Emails > Write** and add the account of the individual performing the integration. Once these permissions are set, the individual will be ready to start the integration.

{%
  include image.html
  caption="Manage permissions for Dataset and Properties" path="guides/integrations/grant-write-permissions.png"
%}

{%
include continued.html
title="Integrations Part 2 – Integration Scripts"
url="/guides/integrations-2/"
%}