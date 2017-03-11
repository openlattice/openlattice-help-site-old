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

### Is Loom HIPAA and CJIS compliant?

Loom infrastructure is compliant with the **Health Insurance Portability and
Accountability Act (HIPAA)** and **Criminal Justice Information Services (CJIS)** Security Policy.
We are capable of signing **Business Associate Agreements (BAAs)** and we comply with
HIPAA Security Rule by ensuring PHI is encrypted
in transit and at rest with industry-standard encryption. We also
support disaster recovery by securely storing live snapshots of the database
system that we can recover quickly from backup.

{% include related.html content="
* [Loom's HIPAA Compliance Technical Overview](/info/hipaa/)
* [U.S. Dept. of Health and Human Services: Data Driven Justice and HIPAA FAQ](http://www.naco.org/sites/default/files/documents/DDJ%20HIPPA%20FAQs.pdf)
* [U.S. Dept. of Health and Human Services: Sample Business Associate Agreement ](https://www.hhs.gov/hipaa/for-professionals/covered-entities/sample-business-associate-agreement-provisions/index.html)
" %}

### What about privacy laws for mental health or substance abuse data?

For information on confidentiality for mental health data, please refer to our
HIPAA resources above.

For alcohol and substance abuse data, we can sign a joint BAA and
**Qualified Service Organization Agreement (QSOA)** that includes
provisions for **42 CFR Part 2**. Loom also gives dataset owners full control
over the permissions on their dataset and its properties, so they can hide PII
data that could be used to identify an individual.

{% include related.html content="
* [NY Office of Alcoholism & Substance Abuse Services: Sample BAA + QSO Agreement](https://www.oasas.ny.gov/JC/documents/QSOAAgreement.pdf)
* [Substance Abuse and Mental Health Services Administration: Confidentiality FAQ](https://www.samhsa.gov/about-us/who-we-are/laws/confidentiality-regulations-faqs)
" %}

### How does Loom comply with additional confidentiality laws at the state level?

Laws vary from state-to-state, but we will do our best to answer questions about
how the Loom platform can work within your state.
Please reach out to us at [{{site.email}}](mailto:{{site.email}}).

## Using Loom

### Who controls the data once it's in Loom?

**You have full ownership over your datasets and full control over the permissions on the dataset and property types.**

No one can read, write, link, or search for your dataset unless if you, the dataset owner, explicitly set the permissions to allow it. If an individual needs access to your data, they must request permission and you are given the control to grant or revoke that permission. You can grant permission to individuals (by email address) or to **User Roles** you define.

If you have a team you need to grant permissions to, we recommend creating a user Role, assigning that role to members of your **organization**, and granting permission to that role. We also support de-identification so you can easily share data with outside stakeholders, such as research organizations and universities.

{% include related.html content="
* [How to assign and revoke user permissions](/guides/permissions/)
* [How to create an organization and User Roles](/guides/organizations/)
" %}

### What data formats can I integrate into Loom?

We support any data format supported by Apache Spark including:
* CSV
* Apache Parquet
* PostgreSQL
* Any Java (JDBC) DataSource

If you need to integrate data from a format that's not on this list, send us an [email]({{site.email}}) and we'll see what we can do to help.

{% include related.html content="
* [How to integrate your data](/guides/integrations/)
" %}

### Are there any sample datasets that I can view?

We created sample datasets to help you explore features of the Loom platform:

{% include sampledata.html %}

To access the sample datasets, visit the **Catalog** and search
"sample". Everyone with a Loom account can **Read** and **Link** the sample datasets and their properties. Feel free to use these datasets as you explore features of the platform.

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
* Linking and merging across multiple datasets
* Graphing and visualizing your data

If you need additional services outside of our core functionality, please [email](mailto:{{site.email}}) us and we'll see what we can do to help.

### How can I schedule a demo?

[Email](mailto:{{site.email}}) our Customer Success Manager if you're interested in scheduling a phone call or webinar for your team. We also have some of our demo videos available online that you can [view here](/demos/).
