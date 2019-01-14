---
layout: page
title: HIPAA Compliance Technical Details
description: OpenLattice technical specifications for HIPAA compliance.
---

* TOC
{:toc}

## HIPAA Security Rule

OpenLattice complies with [HIPAA Security Rule](https://www.hhs.gov/sites/default/files/ocr/privacy/hipaa/administrative/securityrule/techsafeguards.pdf), which establishes technical specifications for applications that work with protected health information online.

### Access Control and Emergency Access Procedure

OpenLattice supports built-in, LDAP, SAML, and OAUTH for authentication. Organizations using Google Apps or Azure AD can simply have their users sign in once their organization is configured on the platform.

OpenLattice organization owners can enforce additional security requirements by specifying a minimum password length or requiring special characters in passwords. 

Users can control access to Entity Sets they own by assigning and revoking user permissions or by defining a user role. OpenLattice also allows the owner of the dataset to create a de-identified copy by stripping PII fields from the Entity Set before sharing the data with outside stakeholders.

{% include related.html content="
* [Guide: Permissions](/guides/permissions/)
" %}

### Encrypted Data and HIPAA Compliant infrastructure

OpenLattice's infrastructure is built on HIPAA and CJIS compliant Amazon services such as Amazon EBS (Elastic Block Storage), which uses virtual hard drives encrypted with Amazon KMS (Key Management System). All data is encrypted in transit (TLS 1.2+) and at rest with AES-256 encryption. We monitor the list of safe ciphers periodically for extra layer of security. 

All employees who have access to AWS infrastructure have been CJIS-background checked and completed CJIS level 4 training.  Client data is isolated using fine grained access control rules that are propagated down into the database and ElasticSearch cluster.

{% include related.html content="
* [Amazon Web Services (AWS) HIPAA Compliance](https://aws.amazon.com/compliance/hipaa-compliance/)
* [Amazon Web Services (AWS) CJIS Compliance](https://aws.amazon.com/compliance/cjis/)
" %}

### Data Security

Amazon has near immediate notification for security vulnerabilities. Our policy is to report breaches of unsecured PHI without unreasonable delay no later than required by statutory limits.	
We perform annual third-party penetration tests against OpenLattice application and hosted environment.

### Support

Technical support for issues involving Protected Health Information (PHI) is provided by trained staff members. 

### Audit Controls

Our platform maintains cryptographically tamper resistant audit logs that record data access through every time a user reads or accesses the data. We use Osquery to perform security audits of our servers. Account activity is also logged by Amazon AWS. 

### Disaster Recovery

We store periodic snapshots of the state of the Postgres database clusters, so the information can be used to recover the system in the event of a system failure. We take daily encrypted snapshots to a private S3 bucket with weekly transfers into Glacier. Services are horizontally scalable and auto-balancing across multiple AWS availability zones. All data centers are located in the continental US. 

