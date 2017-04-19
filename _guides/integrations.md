---
layout: page
title: Integrations Part 1 – Data Model & Permissions
description: Create a new dataset and configure permissions for your data integration. This guide is recommended for dataset owners who are preparing datasets for new integrations.
weight: 1
---

* TOC
{:toc}

## 1. Create your data model

**Data integrations** take your data and map it to Loom's standardized data model. You will need to submit a request to a Loom administrator to create the data model for your dataset.

> **How do I submit a request?** List your column headers, descriptions, and data types in this [this spreadsheet](/files/DatasetColumnHeaderSubmission.xlsx) and email it to us at [{{site.email}}](mailto:{{site.email}}). A member of the Loom team will reply within 24 hours with further instructions.

## 2. Create your dataset

Once you receive the the details for your data model, go to the **Datasets** tab and create a new dataset with the information provided.

{%
  include image.html
  caption="Example: Creating a dataset for crime index dataset"
  path="guides/integrations/create-datasource.png"
%}

## 3. Identify who will perform the data integration

In order for a data integration to be completed, Loom verifies that the credentials provided correspond to an individual who has been granted write permissions to the dataset. 

**Identify a trusted member of your team (most likely the individual who is currently managing your data) and grant them write permissions to your new dataset. Dataset owners can also perform data integrations themselves, but it is advised to use an account with more _limited_ permissions to perform the data integration.**

<span class="bad">Not Recommended:</span> _Dataset owners use their own accounts to perform data integrations_

> Dataset owners have read, write, and link permissions to datasets they create. However, they also have administrative permissions such as adding and removing permissions from users and other dataset owners. Dataset owners _can_ use their personal login credentials to perform a data integration, but this is not advised.

<span class="good">Recommended:</span> _Dataset owners use a separate account with limited permissions to perform data integrations_

> Create a separate Loom account with a chosen username and password (not Google Auth) and use those credentials for data integrations. Or, have a member of your team (someone who currently manages your data in your organization) perform the data integration for you using their own account.

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