---
layout: page
title: How to find high utilizers in your data
description: Discover individuals in need who are associated with recurring events across multiple datasets.
weight: 0
---

* TOC
{:toc}

## What are high utilizers?

High utilizers (or super utilizers) are individuals in need, who are associated with recurring events across multiple sectors such as health care and public safety.

## How can OpenLattice help you find high utilizers in your data?

OpenLattice gives you the ability to link across disparate datasets, such as EMS, Calls for Service, or Jail Stays/Records, so you can create merge records that give you a more complete picture of a specific individual. Once the data is merged, you then have the ability to rank the individuals based on the number events they are associated with.

## What do I need to do before I can generate a high utilizer list?

### Associations

First, you need to integrate a dataset with **associations** defined. Associations define relationships between data contained in separate tables. 

> **Question: What if the dataset I want to integrate is just one table with many columns? Can I still calculate high utilizers using this dataset?** 
>
> Absolutely! A dataset will usually have many fields that correspond to different entities. For example, here are the column names used for our [Sample Jail dataset](https://thedataloom.com/gallery/#/entitysets/2cb51b29-b06e-4760-973a-aa664ee70538):
* First Name
* Last Name
* Date Of Birth
* Race
* Home Address
* Booking ID
* Date Booked
* Date Released
>
> This sample dataset was originally a single CSV with 8 columns. The integration process began with sorting the columns into 2 categories: People information (demographics) and Jail Stay information (Date Booked, Date Released, and Booking ID). This will result in 2 integrated tables with an "booked in" relationship defined between them since a _Person booked in a Jail Stay_.

The associations you define for your datasets are completely up to you. If you would like assistance creating associations or determining how to group your dataset fields, please contact <a href="mailto:support@openlattice.com">support@openlattice.com</a>.

{% include related.html content="
* [How to Integrate your data Part 1 and 2](/guides/integrations/)
"
%}


## How do I generate my own high utilizer list?

### Generating High Utilizers Lists

 Visit your dataset and under the **Actions** menu, click on "Find Top Utilizers". If you don't have a dataset integrated, but would like to try out the high utilizers UI, feel free to visit our [Demo Suspects](https://openlattice.com/gallery/#/entitysets/04e6f62f-4052-48f7-8b16-8b9f19ea920a) dataset in the OpenLattice platform.

{% include image.html 
caption="Finding top utilizers through the Actions menu"
path="guides/high-utilizers/high-utilizers-actions-menu.png"
%}

### Search Parameters

The associations you set will determine what search parameters you can define.

{% include image.html 
caption="Search parameters for Demo Suspects high utilizers"
path="guides/high-utilizers/search-params.png"
%}

For the Demo Suspects dataset, each subject _appears in_ an incident. This allows you to rank your high utilizers list by number of incidents:

{% include image.html 
caption="Use the Demo Suspects dataset try this search parameter!"
path="guides/high-utilizers/sample-incidents-params.png"
%}

### Multiple Search Parameters

You can continue to add search parameters if you have additional associations or datasets related to your Person dataset.

{% include image.html 
caption="Use the Demo Suspects dataset try this search parameter!"
path="guides/high-utilizers/search-params-2.png"
%}


