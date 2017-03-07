---
layout: page
title: FAQs
permalink: /faq/
description: Learn about Loom's HIPAA compliance, permissions, pricing, and more.
menu: true
---

* TOC
{:toc}

## Is Loom HIPAA and CJIS compliant?

Loom infrastructure is compliant with the Health Insurance Portability and Accountability Act (HIPAA) and Criminal Justice Information Services (CJIS). We are also capable of signing HIPAA Business Associate Agreements (BAAs) for storing data. We comply with HIPAA Security Rule by ensuring PHI is encrypted in transit and at rest with industry-standard standard encryption. We also support disaster recovery by securely storing live snapshots of the database system that we can recover quickly from backup.

{% include related.html content="
* [Loom's HIPAA Compliance Technical Overview](/info/hipaa/)
* [Data Driven Justice and HIPAA (.pdf)](http://www.naco.org/sites/default/files/documents/DDJ%20HIPPA%20FAQs.pdf)
* [HHS.gov: Sample Business Associate Agreement ](https://www.hhs.gov/hipaa/for-professionals/covered-entities/sample-business-associate-agreement-provisions/index.html)
" %}

## Can I control what data I share?

Yes, you retain ownership of the data you upload and have full control over the permissions on the dataset and property types. Individuals can request access to read your datasets, or you can automatically assign permissions using Roles you define in your organization. We also support de-identification so you can easily share data with outside stakeholders.

{% include related.html content="
* [Learn how to assign and revoke user permissions](/guides/permissions/)
" %}

## What data formats can I integrate into Loom?

We support any data format supported by Apache Spark including:
* CSV
* Parquet
* Postgres
* Any JDBC datasources

> **Contact Us:** If you need to integrate data from a format that's not on this list, [please email us]({{site.email}}) and we'll see what we can do to help.

## How much does Loom cost?
All of Loom's core services are **free**. This includes:

* Unlimited number of data integrations
* Making datasets available for download
* Removing PII through de-identification
* Linking and Merging across multiple datasets
* Creating Timelines and Visualizations

> **Contact Us:** If you need additional services outside of our core functionality, [please email us](mailto:{{site.email}}) and we'll see what we can do to help.

## Are there any sample datasets that I can view?

Yes! When you first log in, visit **Catalog** and search
"sample" for example datasets you can view and search in our systems.

{% include related.html
  content="
  * [Learn more about searching datasets in Loom](http://help.thedataloom.com/guides/search/)
"%}


## How can I schedule a demo?

Feel free to [reach out](mailto:{{site.email}}) to our Customer Success Manager if you're interested in scheduling a phone call or webinar for your team. We also have some of our demo videos available online that you can [view here](/demos/).
