---
layout: page
title: HIPAA Compliance Technical Details
description: Loom technical specifications for HIPAA compliance.
---

* TOC
{:toc}

## HIPAA Security Rule

Loom complies with [HIPAA Security Rule](https://www.hhs.gov/sites/default/files/ocr/privacy/hipaa/administrative/securityrule/techsafeguards.pdf), which establishes technical specifications for applications that work with protected health information online.

### Access Control and Emergency Access Procedure

Users can control access to Entity Sets they own by assigning and revoking user permissions or by defining a user role. Loom supports de-identification by stripping PII fields from the Entity Set before sharing the data with outside stakeholders.

* [Tutorial: Permissions](/guides/permissions/)

### Encrypted Data and HIPAA Compliant infrastructure

Loom's infrastructure is built on HIPAA and CJIS compliant Amazon services such as Amazon EBS (Elastic Block Storage), which uses virtual hard drives encrypted with Amazon KMS (Key Management System). All data is stored securely at rest encrypted with AES-256.

* [Amazon Web Services (AWS) HIPAA Compliance](https://aws.amazon.com/compliance/hipaa-compliance/)
* [Amazon Web Services (AWS) CJIS Compliance](https://aws.amazon.com/compliance/cjis/)

### Audit Controls

We have tamper resistant audit logs that record every time someone reads or accesses your data.

### Disaster Recovery

We store periodic snapshots of the state of the Cassandra database clusters, so the information can be used to recover the system in the event of a system failure.
