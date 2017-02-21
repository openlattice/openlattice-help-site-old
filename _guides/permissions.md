---
layout: page
title: Permissions
description: Learn how to request, grant, and revoke user permissions to your dataset. Simplify user permissions with roles.
---

* TOC
{:toc}

## Permission Types

* **Hidden**: Dataset is not visible to anyone except you.
* **Discover**: Reserved for future use. **(Do Not Use)**
* **Read**: Allows user to read data in the dataset.
* **Write**: Allows user to add new data to the dataset. *This permission should only be used by owners of the data who are completing data integrations.*
* **Owner**: Allows a user full admin access to the dataset, including managing permissions for the dataset and data properties. By default, the user who integrates the data is classified as owner of the data.

## Requesting permissions

Anyone who signs up on Loom can request access to any non-hidden dataset. All permission requests are sent to the dataset owner to approve or reject.

Any property you do not have permission to view will be marked with a lock icon:

{% include image.html caption="No access to properties" path="guides/permissions-lock-icons.png" %}

You can request permission to view certain properties through the Actions menu:

{% include image.html caption="Actions Menu" path="guides/permissions-request.png" %}

Just select which properties you want access to:

{% include image.html caption="Requesting Permissions" path="guides/permissions-request-properties.png" %}

Once the owner approves the request, you will be able to view data for those properties. The owner can also revoke permission at any time.

## Granting permissions

When a user requests permission to view an Entity Set you own, a Permissions Request will appear
at the top of the Details page for your Entity Set.

{% include image.html caption="Permission Requests" path="guides/permissions-grant-request.png" %}

Accepting the permissions request will give read access to the user requesting the data. You can also review the properties they requested before deciding to allow permission.

{% include image.html caption="Properties Requested" path="guides/permissions-properties-requested.png" %}

## Revoking permissions

Permissions can be revoked by the dataset owner at anytime by navigating to your dataset and clicking **Manage Permissions**. Permissions can be added or revoked on a per-dataset or per-property basis.

{% include image.html caption="Revoke permissions from individual" path="guides/permissions-revoke.png" %}

## Using Roles

{% include construction.html %}
