---
layout: page
title: Linking Entity Sets
description: Link and merge data across your Entity Sets.
---

* TOC
{:toc}

## Step 1: Choose entity sets to link

In the dropdown menu, select the two (or more) entity sets you want to link.

> **Note:** Existing linked datasets can appear in the menu, since once you link a dataset, you can continue to link it with other datasets.

## Step 2: Choose property types to link

Select the properties you want to use for the linking. For example, you may want to link two datasets by matching First and Last Names.

Select the property, and then choose at least 2 entity sets you want to link with that property using the dropdown menu. Click "Add Links".

> **Note:** If some of your chosen entity sets are not appearing in the list, it means the property does not exist in that entity set.

## Step 3: Define the linked entity type to create

Use the following naming conventions to define your entity type:

| Field       | Purpose                                                  | Naming Convention         | Example                                                           |
|-------------|----------------------------------------------------------|---------------------------|-------------------------------------------------------------------|
| Namespace   | Used as a category for your entity set                   | All lowercase / No spaces | iowa                                                              |
| Name        | The (short) name of your entity set                      | All lowercase / No spaces | cfsjaildemo                                                       |
| Title       | Longer title for your entity set                         | Titlecase Capitalization  | Demo of Linking Jail Data and CFS Data                            |
| Description | Longer description about the contents of your entity set | Sentence Capitalization   | Full demo of linking Jail and CFS Data for the month of 1-17-2017 |

<br>
Check the boxes for the properties you want to output in the generated linked dataset.

> **Note:** You could link using the Date of Birth property, and then choose to not output the Date of Birth property in the output. This is entirely customizable to your needs.

Click "Create Linked Entity Type" to move onto the next step.

## Step 4: Define your linked entity set

Using the same naming and description you listed in Step 3, fill out the Name, Title, and Description for your new Entity Set and click "Link" to create your Linked Entity Set.
