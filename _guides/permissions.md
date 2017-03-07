---
layout: page
title: How to manage permissions on your datasets
description: Request, grant, and revoke user permissions to your dataset. Use roles to assign permissions to a group of members in your organization.
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

Anyone who signs up on Loom can request access to a non-hidden dataset.
All permission requests are sent to the dataset owner to approve or reject.

Properties will be marked with a lock icon until the dataset owner approves
your request. The owner can also revoke access at any time.

{%
  include image.html
  caption="Requesting permission to view properties"
  path="guides/permissions/request.gif"
%}

## Granting permissions

When a user requests permission to view an Entity Set you own, a Permissions
Request will appear at the top of the Details page for your Entity Set. Accepting
the permissions request will give read access to that user.
You can also review the properties they requested before deciding to
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

If you've created an organization, you can assign permissions to a **Role** and
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
