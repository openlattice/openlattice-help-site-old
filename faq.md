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

Loom infrastructure is compliant with the **Health Insurance Portability and Accountability Act (HIPAA)** and Criminal Justice Information Services (CJIS). We are also capable of signing HIPAA **Business Associate Agreements (BAAs)** for storing data. We comply with HIPAA Security Rule by ensuring PHI is encrypted in transit and at rest with industry-standard standard encryption. We also support disaster recovery by securely storing live snapshots of the database system that we can recover quickly from backup.

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
* [How to assign and revoke user permissions](/guides/permissions/)
* [How to create an organization and User Roles](/guides/organizations/)
" %}

## Using Loom

### What data formats can I integrate into Loom?

We support any data format supported by Apache Spark including:
* CSV
* Parquet
* Postgres
* Any JDBC datasources

{% include related.html content="
* [How to integrate your data](/guides/integrations/)
" %}

> **Contact Us:** If you need to integrate data from a format that's not on this list, [please email us]({{site.email}}) and we'll see what we can do to help.

### Are there any sample datasets that I can view?

Yes, here are a few of them:

* [Sample OpenFlights Airport Data](https://thedataloom.com/gallery/#/entitysets/514e6a5f-2bcd-4206-bb1c-448a9dbcf06d)
* [Sample Stolen Cars](https://thedataloom.com/gallery/#/entitysets/b65b0068-5bf1-4e36-9c68-2e19afedd76b)
* [Crime Index Data for Nation's Largest Cities (2009)](https://thedataloom.com/gallery/#/entitysets/4afe7e03-5dc0-4178-951f-affc47ab28de)
* [Sample Jail Data](https://thedataloom.com/gallery/#/entitysets/e81e19dd-970e-4e8e-84e6-50c5afdb7bcf)
* [Sample Health Data](https://thedataloom.com/gallery/#/entitysets/8e9638e3-f554-4d12-bf2f-c5b3502c0d4c)
* [Sample Linked Jail and Health Data](https://thedataloom.com/gallery/#/entitysets/a55c1d21-52ec-4f3e-80ea-f21c64b7f4f0)

To access the sample datasets, visit **Catalog** and search
"sample" for example datasets you can view and search in our systems. Everyone with a Loom account can **Read** and **Link** the sample datasets and their properties. Feel free to use these datasets as you explore features of the platform.

{% include related.html
  content="
  * [How to search datasets](/guides/search/)
  * [How to link datasets](/guides/linking/)
  * [How to visualize your data](/guides/visualizations/)
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
