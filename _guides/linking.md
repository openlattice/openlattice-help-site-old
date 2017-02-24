---
layout: page
title: Linking Entity Sets
description: Link and merge data across your Entity Sets.
---

1. TOC
{:toc}

## Where is Loom's linking feature?

Loom's linking feature is currently located [here](https://thedataloom.com/gallery/#/link). Please let us know of any feedback you have on the linking workflow.

{% include video.html id="mJjywiSf5Ak" %}

## Choose entity sets to link

In the dropdown menu, select the two (or more) entity sets you want to link and click **Define the links to join the entity sets** to continue to the next step.

{%
  include image.html
  caption="Choose entity sets to link"
  path="/guides/linking/step1.gif"
%}

> **Note:** Existing linked datasets can appear in the menu, since once you link a dataset, you can continue to link it with other datasets.

## Choose property types to link

Select the properties you want to use for the linking. For example, you may want to link two datasets by offense code. Select the property, and then choose at least 2 entity sets you want to link with that property using the dropdown menu. Click "Add Links".

{%
  include image.html
  caption="Choose entity sets to link"
  path="/guides/linking/step2.gif"
%}

> **Note:** If some of your chosen entity sets are not appearing in the list, it means the property does not exist in that entity set.

## Define the linked entity type to create

If you want to only include de-identified properties in the output, select **Deidentify**. Review the list of property types that will be included in the output before clicking **Create linked entity type**.

{%
  include image.html
  caption="Define the linked entity type"
  path="/guides/linking/example.png"
%}

Use the following naming conventions when you are defining your new linked entity type.

{% include naming-conventions.html %}

> **Note:** It is possible to have linking permissions for a given property, but not have read permissions.

{%
  include related.html
  content="
  * [Learn more about setting permissions in Loom](/guides/permissions/)
  "
%}

## Define your linked entity set

Using the same naming and description from Step 3, fill out the Name, Title, and Description for your new Entity Set and click "Link" to create your Linked Entity Set.

{%
  include image.html
  caption="Create the linked entity set"
  path="/guides/linking/step4.png"
%}
