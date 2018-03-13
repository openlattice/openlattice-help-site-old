---
layout: page
title: How to manage permissions on your datasets
description: Request, grant, and revoke user permissions to your dataset. Use roles to assign permissions to a group of members in your organization.
---

* TOC
{:toc}

## Permission Types

Permissions can be set on your dataset and each of its properties. The owner
of a dataset will be given Write, Read, Link, and Discover permissions.

**New datasets will only grant permissions to the owner by default. All other users in OpenLattice must be explicitly granted permissions by the dataset owner.**

{% include image.html
path="/guides/permissions/permissions.png"
caption="Current permission types in OpenLattice: Owner, Write, Read, Link.<br> (Discover permissions are still under development, and do not grant any additional permissions at this time.)"
%}

{% include permission-types.html %}

## Requesting permissions

All permission requests are sent to the dataset owner to approve or reject. Granting
a permission request will grant **read** permissions to a user.

Properties will be marked with a lock icon until the dataset owner approves
your request. The owner can modify your request by removing properties you have
requested access to, and also revoke access at any time.

{%
  include image.html
  caption="Requesting permission to view properties"
  path="guides/permissions/request.gif"
%}

## Granting permissions

When a user requests **read** permission on a dataset you own, a **Permission
Request** will appear at the top of the **Details** page for your Entity Set. Accepting
the permissions request will give read access to that user.
You can also review and modify the properties they requested before deciding to
allow permission.

{%
  include image.html
  caption="Granting permission to a user"
  path="guides/permissions/allow-deny.gif"
%}

## Revoking permissions

Permissions can be revoked by the dataset owner at anytime by navigating to your
dataset and clicking **Manage Permissions**. Permissions can be added or revoked
on a per-dataset or per-property basis.

{%
  include image.html
  caption="Revoke permissions from individual"
  path="guides/permissions/revoke.png"
%}

## Using Roles

{% include video.html
  id="xcyJFcmDzSc"
%}

If you've created an organization, dataset owners can assign permissions to a **Role** and
all the users with that assigned role will be able to access the dataset with
those permissions.

<!-- {%
  include image.html
  caption="Assign permissions to a role"
  path="guides/permissions/roles.gif"
%} -->

{%
  include related.html
  content="
  * [Learn how to create an organization and assign user roles](/guides/organizations/)
  "
%}

## Setting permissions for linking datasets

If you are linking datasets, or someone you know has requested permission to link
datasets you own, follow these instructions to ensure permissions are set
properly to perform the linking.

### When you own both datasets

Owners already have **link** and **read** permissions on datasets and all of their properties.
**No additional permissions are needed if you own all the datasets required for
the linking.**

### When you own only one of the datasets

Contact the dataset owner directly and ask them to grant you **link** permissions
to the dataset and all of its properties. This step is required to complete the linking.

You will also need **read** permission to the dataset and any property that the
dataset owner will allow you to view values for. The dataset owner may choose to only grant
you read permission to non-PII properties. This decision is completely up to the dataset owner.

{% include image.html
  path="/guides/permissions/linking-permission-example.png"
  caption="Example of permission settings when you own one one dataset and need
  limited linking permissions to a dataset you do not own."%}

### When you don't own either of the datasets

Contact the dataset owners directly and ask them to grant you **link** permissions to the
datasets and all of their properties. This step is required to complete the linking.

You will also need **read** permission to each dataset and any properties that the dataset
owners will allow you to view values for. The dataset owners may choose to only
grant you read permission to non-PII properties. This decision is completely up to the dataset owner.

{% include image.html
  path="/guides/permissions/linking-permission-example2.png"
  caption="Example of permission settings when you do not own either dataset and need
  limited linking permissions to both."%}
