---
layout: page
title: FAQs
permalink: /faq/
description: Learn about Loom's HIPAA compliance, permissions, pricing, and more.
menu: true
---

* TOC
{:toc}

## Privacy & Security

### Is Loom HIPAA and CJIS Compliant?

Loom infrastructure is compliant with the Health Insurance Portability and Accountability Act (HIPAA) and Criminal Justice Information Services (CJIS). We are also capable of signing HIPAA **Business Associate Agreements (BAAs)** for storing data. We comply with HIPAA Security Rule by ensuring PHI is encrypted in transit and at rest with industry-standard standard encryption. We also support disaster recovery by securely storing live snapshots of the database system that we can recover quickly from backup.

{% include related.html content="
* [Loom's HIPAA Compliance Technical Overview](/info/hipaa/)
* [Data Driven Justice and HIPAA (.pdf)](http://www.naco.org/sites/default/files/documents/DDJ%20HIPPA%20FAQs.pdf)
* [HHS.gov: Sample Business Associate Agreement ](https://www.hhs.gov/hipaa/for-professionals/covered-entities/sample-business-associate-agreement-provisions/index.html)
" %}

### Who controls the data once it's in Loom?

**You have full ownership over your datasets and full control over the permissions on the dataset and property types.**

No one can read, write, link, or search for your dataset unless if you, the dataset owner, explicitly set the permissions to allow it. If an individual needs access to your data, they must request permission and you are given the control to grant or revoke that permission. You can grant permission to individuals (by email address) or to **User Roles** you define.

If you have a team you need to grant permissions to, we recommend creating a user Role, assigning that role to members of your **organization**, and granting permission to that role. We also support de-identification so you can easily share data with outside stakeholders, such as research organizations and universities.

{% include related.html content="
* [How-To: Assign and revoke user permissions](/guides/permissions/)
* [How-To: Create an organization and User Roles](/guides/organizations/)
" %}

## Using Loom

### What data formats can I integrate into Loom?

We support any data format supported by Apache Spark including:
* CSV
* Parquet
* Postgres
* Any JDBC datasources

{% include related.html content="
* [How-To: Integrate your data](/guides/integrations/)
" %}

> **Contact Us:** If you need to integrate data from a format that's not on this list, [please email us]({{site.email}}) and we'll see what we can do to help.

### Are there any sample datasets that I can view?

Yes!

When you first log in, visit **Catalog** and search
"sample" for example datasets you can view and search in our systems. Everyone who logs into Loom is granted **Read** permissions to these datasets and their properties. Feel free to use these datasets as you explore features of the platform.

{% include related.html
  content="
  * [How-To: Search datasets](/guides/search/)
  * [How-To: Link datasets](/guides/linking/)
  * [How-To: Visualize your data](/guides/visualizations/)
"%}

## General

### How much does Loom cost?
All of Loom's core services are **free**. This includes:

* Unlimited number of data integrations
* Making datasets available for download
* Removing PII through de-identification
* Linking and Merging across multiple datasets
* Creating Timelines and Visualizations

> **Contact Us:** If you need additional services outside of our core functionality, [please email us](mailto:{{site.email}}) and we'll see what we can do to help.

### How can I schedule a demo?

Feel free to [reach out](mailto:{{site.email}}) to our Customer Success Manager if you're interested in scheduling a phone call or webinar for your team. We also have some of our demo videos available online that you can [view here](/demos/).
