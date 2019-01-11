---
layout: page
title: FAQs
permalink: /faq/
description: Learn about OpenLattice's HIPAA compliance, permissions, pricing, and more.
menu: true
---

* TOC
{:toc}

## Privacy & Security

### Is OpenLattice HIPAA and CJIS compliant?

Yes. OpenLattice infrastructure is compliant with the **Health Insurance Portability and Accountability Act (HIPAA)** and **Criminal Justice Information Services (CJIS) Security Policy**.

In accordance with HIPAA Security Rule, we ensure Protected Health Information (PHI) is encrypted in transit (TLS 1.2+)and at rest using an industry-standard AES encryption. We maintain detailed audit logs and support disaster recovery by securely storing live snapshots of the database system that we can recover quickly from backup.

{% include related.html content="
* [OpenLattice's HIPAA Compliance Technical Overview](/info/hipaa/)
* [U.S. Dept. of Health and Human Services: Data Driven Justice and HIPAA FAQ](http://www.naco.org/sites/default/files/documents/DDJ%20HIPPA%20FAQs.pdf)
" %}

### Can OpenLattice sign a Business Associate Agreement (BAA)?

Yes. We recommend using a BAA, in addition to a **Qualified Service Organization Agreement (QSOA)** if substance use data will be included.

{% include related.html content="
* [U.S. Dept. of Health and Human Services: Sample BAA ](https://www.hhs.gov/hipaa/for-professionals/covered-entities/sample-business-associate-agreement-provisions/index.html)
* [NY Office of Alcoholism & Substance Abuse Services: Sample BAA + QSOA](https://www.oasas.ny.gov/JC/documents/QSOAAgreement.pdf)
" %}

### How does OpenLattice comply with laws around mental health and substance use data?

For information on confidentiality for mental health data, please refer to our
HIPAA resources above.

For alcohol and substance use data, we can sign a joint BAA and QSOA that includes provisions for **42 CFR Part 2**. OpenLattice also gives dataset owners full control over the permissions on their dataset and its properties, so they can hide PII data that could be used to identify an individual.

{% include related.html content="
* [Substance Abuse and Mental Health Services Administration: Confidentiality FAQ](https://www.samhsa.gov/about-us/who-we-are/laws/confidentiality-regulations-faqs)
* [NY Office of Alcoholism & Substance Abuse Services: Sample BAA + QSOA](https://www.oasas.ny.gov/JC/documents/QSOAAgreement.pdf)
" %}

### How does OpenLattice comply with state-level confidentiality laws?

Laws vary from state-to-state, but we will do our best to answer questions about
how the OpenLattice platform can work within your state.
Please reach out to us at [{{site.email}}](mailto:{{site.email}}).

### Who owns the data once it has been integrated?

**You have full ownership of your data once it's integrated into OpenLattice.**

OpenLattice provides a secure data sharing platform built on HIPAA security compliant infrastructure with AWS. Amazon AWS also complies with this policy, as stated in their AWS Data Privacy FAQ:
> "Customers maintain ownership of their customer content and select which AWS services process, store and host their customer content. We do not access or use customer content for any purpose other than as legally required and for maintaining the AWS services and providing them to our customers and their end users. We never use customer content or derive information from it for marketing or advertising."

{% include related.html content="
* [Visit the AWS Data Privacy FAQ for more details on Amazon's data privacy polices](https://aws.amazon.com/compliance/data-privacy-faq/)
" %}

## Using OpenLattice

### Who controls the data once it has been integrated?

**You have full control over permissions for your dataset and its properties.**

Newly integrated datasets are hidden by default. No one can read, write, link, or search for your dataset unless if you, the dataset owner, explicitly set the permissions to allow it. You can add and delete your datasets from the system at any time.

### How do I give permission to an authorized individual to access the data?

You can grant dataset permissions to an individual the following ways:
* The individual can send you a **Permission Request** for the properties they need to access.
* You can manually grant permission to the individual by email address.
* If you need to grant access to a _team_ of individuals, you can assign each individual in that team a custom **Role** and give permission to the role you assigned them.

Assigning roles requires creating an **Organization** and adding those members to your organization.

{% include related.html content="
* [How to request, assign, and revoke user permissions](/guides/permissions/)
* [How to create an Organization and assign user Roles](/guides/organizations/)
" %}

### Can I de-identify the data before sharing data with researchers?

Yes. We support de-identification so you can easily share data with outside stakeholders, such as research organizations and universities.

### Who performs the data integrations?

You can either have someone
from your team integrate the data (with our assistance as needed), or we can have someone from our team can do the integration and we can transfer ownership to you.

### What data formats can be integrated into OpenLattice?

We support any data format supported by Apache Spark including:
* CSV
* Apache Parquet
* PostgreSQL
* MySQL
* Any Java (JDBC) DataSource

If you need to integrate data from a format that's not on this list, send us an [email](mailto:{{site.email}}) and we'll see what we can do to help.

{% include related.html content="
* [How to integrate your data](/guides/integrations/)
" %}

### Are there any sample datasets that I can use to test the platform?

Yes. We created sample datasets to help you explore features of the OpenLattice platform:

{% include sampledata.html %}

To access the sample datasets, visit the **Catalog** and search
"sample". Everyone with a OpenLattice account can **Read** and **Link** the sample datasets and their properties.

{% include related.html
  content="
  * [How to search datasets](/guides/search/)
  * [How to link datasets](/guides/linking/)
  * [How to visualize your data](/guides/visualizations/)
"%}

### How much does OpenLattice cost?

In order to help jurisdictions leverage technology to improve human outcomes, **we made all of OpenLattice's core services completely free.**

* Unlimited number of data integrations
* Making datasets available for download
* Phonetically searchable datasets
* Linking and merging data across multiple datasets
* Removing PII through de-identification
* Graphing and basic visualizations
* Retrieving data on top utilizers from a dataset

This list of core services is continually growing as we work with early adopters to create new features that better serve your needs. Schedule a demo for you and your team to learn more about how we can help you solve your data challenges.

## Next Steps

### How can I request a demo?

[Email](mailto:{{site.email}}) our Customer Success Manager if you're interested in scheduling a phone call or webinar for your team. We also have some of our demo videos available online that you can [view here](/demos/).

### What datasets should I ask for?

This depends on your needs! But if you're not sure where to begin, we recommend first reaching out to your local police department for datasets on jail booking and calls for service. We generally suggest requesting 5-10 years worth of data, but it may be helpful to begin with a smaller time frame and expanding it at a later date.

EMS data is often managed state-wide. We recommend reaching out to your state EMS or ambulence authority to learn more.

### Do you have any resources or handouts I can share with stakeholders?

Yes! Visit our [Demos](/demos/) page for editable slide decks, printouts, and more. We also take requests if a resource you need is not currently available.

### Do you have any sample BAAs or QSOAs? 

We have sample documents in our [Privacy & Security FAQ](/faq/#privacy--security) you can use as a reference. Please reach out to <a href="mailto:help@OpenLattice.com">help@OpenLattice.com</a> for more information.

### I have a dataset ready, can I integrate it now?

Yes! Please refer to our [Integrations guide](/guides/integrations/) and feel free to reach out to <a href="mailto:help@OpenLattice.com">help@OpenLattice.com</a> for further assistance.

