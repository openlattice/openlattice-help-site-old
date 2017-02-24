---
layout: page
title: Permissions
description: Learn how to request, grant, and revoke user permissions to your dataset. Simplify user permissions with roles.
---

* TOC
{:toc}

## Permission Types

Permissions can be set for entity sets and properties. If a user has **Read** access
to an entity set, they will be able to search for the entity set and view a list
of properties contained in the entity set. However, they will not be able to
read the values of each property unless they are also given **Read** access to each
of the properties listed.

{% include permission-types.html %}

## Requesting permissions

Anyone who signs up on Loom can request access to any non-hidden dataset.
All permission requests are sent to the dataset owner to approve or reject.

Any property you do not have permission to view will be marked with a lock icon:

{%
  include image.html
  caption="No access to properties"
  path="guides/permissions-lock-icons.png"
%}

You can request permission to view certain properties through the Actions menu:

{%
  include image.html
  caption="Actions Menu"
  path="guides/permissions-request.png"
%}

Just select which properties you want access to:

{%
  include image.html
  caption="Requesting Permissions"
  path="guides/permissions-request-properties.png"
%}

Once the owner approves the request, you will be able to view data for those
properties. The owner can also revoke permission at any time.

## Granting permissions

When a user requests permission to view an Entity Set you own, a Permissions
Request will appear
at the top of the Details page for your Entity Set.

{%
  include image.html
  caption="Permission Requests"
  path="guides/permissions-grant-request.png"
%}

Accepting the permissions request will give read access to the user requesting
the data. You can also review the properties they requested before deciding to
allow permission.

{%
  include image.html
  caption="Properties Requested"
  path="guides/permissions-properties-requested.png"
%}

## Revoking permissions

Permissions can be revoked by the dataset owner at anytime by navigating to your
dataset and clicking **Manage Permissions**. Permissions can be added or revoked
on a per-dataset or per-property basis.

{%
  include image.html
  caption="Revoke permissions from individual"
  path="guides/permissions-revoke.png"
%}

## Using Roles

If you've created an organization, you can assign permissions to a **Role** and
all the users with that assigned role will be able to access the dataset with
those permissions.

{%
  include image.html
  caption="Assign permissions to a role"
  path="guides/permissions-roles.gif"
%}

{%
  include related.html
  content="
  * [Learn how to create an organization and assign user roles](/guides/organizations/)
  " 
%}
