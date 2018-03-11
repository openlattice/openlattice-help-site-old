---
layout: page
title: Integrations Part 3 – Sending Data directly to OpenLattice's Servers
description: Bypass manual transfer of .csv files by connecting your database directly to OpenLattice. This guide is recommended for individuals who are interested in setting up integrations into the OpenLattice platform where data is transferred directly and securely from database to database. Reach out to support@openlattice.com if you need any assistance with your integration.
weight: 1
---

* TOC
{:toc}

## 1. Overview 
The previous tutorial, [Integrations Part 2 – Integration Scripts & Reading Data from .csv files](/guides/integrations-2/), walked one through how to run integrations from one's own computer from individual .csv files, one by one, into the OpenLattice platform. However, it may often be more desirable to transfer data, via either whole tables or subsets of tables, directly from your computer or SQL server and onto an OpenLattice server for later integration. 

Your data would be stored in a private database accessible only to you - please contact <a href="mailto:support@topenlattice.com">support@openlattice.com</a> for arrangements. Later, one can still write your own integration script if desired and also put it onto OpenLattice servers to run automatically. A tutorial for this final step is being developed. 

Some advantages of this workflow include:
* Speeding up integrations by transferring the computing power needed away from client-side computer servers over to OpenLattice servers.
* If OpenLattice is assisting with your integrations, security is enhanced by reducing the number of software platforms in the workflow process, via eliminating the need to utilize a third-party system (e.g., Dropbox) for transfer of individual files over to OpenLattice. 
* One maintains full control over the data to be integrated from your server into the OpenLattice platform, including any changes over time.
* Eliminating the need for client-side creation and storage of extra .csv files to send to OpenLattice.

To give an overview of the process, a temporary table is copied over from a client-side server into OpenLattice's "Athena" production server. This is done through a one-time vpn into a client-side computer by OpenLattice to set up the process, with your IT team's presence and support. OpenLattice then runs all data integrations on our side, on these temporary tables. For those that want continuous integrations (for instance, to add to and update CAD police data or behavioral health reports as they come in), a task can be set up on the client-side computer that runs at a set time period and over-writes the table that exists on	 the OpenLattice server, which is then integrated in parallel. These overwrites also ensure that the tables copied onto the OpenLattice side are temporary.

## 2. Setting up your data transfer 
To copy over a table from your server onto Athena, the first step is to create a configuration file using [YAML](https://en.wikipedia.org/wiki/YAML). This configuration file will reside on your computer. It tells your server which table(s) to collect, where to put it on OpenLattice's platform (inside a specific database that will be set up for your jurisdiction), and what to name the table on OpenLattice's side. All of these factors are under your control. 

To create your .yaml file, use a text editor such as NotePad, TextEdit, or [**Sublime**](https://www.sublimetext.com/). 

<span class="bad">NOTE:</span> _A.yaml file must be formatted very exactly, down to the number of spaces there are away from the left margin in each line. Please follow the instructions below exactly._

One can either (1) connect to your servers and copy over various tables, or (2) copy over .csv files.  

### Connecting to a SQL server via JDBC driver

To connect your server, download the YAML file example [**HERE**](/assets/guides/integrations/integration_tutorial.yaml), and change the text between the `<>` characters in each line as needed to fit your data parameters. Below is an explanation of what to write in each line of the file. 

```yaml
name: "<Name of integration for recordkeeping only (not stored anywhere in tables).>"
description: "<Description for recordkeeping (not stored anywhere in tables).>"
integrations:
  - name: <The name of the table that is to be copied to OpenLattice>
    source:
      url: "<the url to the client-side server>"
      sql: "<SQL query that defines the data subset>"
      user: "<username for entry to client-side server>"
      password: "<password to client-side server>"
      driver: org.postgresql.Driver
      fetchSize: <a parameter that dictates how many rows are written in to OpenLattice at a times>
    destination:
      writeUrl: "jdbc:postgresql://athena.openlattice.com:30001/YOUR_DATABASE_NAME_HERE?ssl=true&sslmode=require"
      writeDriver: org.postgresql.Driver
      writeTable: <name of table copy that will be put on OpenLattice server>
      hikari: 
        user: "<your_username to your OpenLattice database>"
        password: "<your_password to your OpenLattice database>"
```

Below is an example of how parameters would be filled out to transfer a file onto the OpenLattice platform. As an example we use our sample healthcare demo data, which is synthetic and does not contain any real people. If one wanted to copy over the entire table, in the `sql:` line one would say `SELECT * FROM demo_health`. Here we show a simple subset of data being pulled out, if for instance one wanted to copy over only records for people named "Jennifer". We are copying it into a training database on the OpenLattice server calld "example_integration". 

NOTE in this example the table is being read from and copied to the same database. In a real situation, the `source` and `destination` URLs will differ.

```yaml
name: "example_filetransfer"
description: "Copying over data from demo health table into OpenLattice server"
integrations:
  - name: demo_health
    source:
      url: jdbc:postgresql://athena.openlattice.com:30001/example_integration?ssl=true&sslmode=require"
      sql: "select * from demo_health where 'FirstName' ='Jennifer'"
      user: "example_user"
      password: "examplepassword"
      driver: org.postgresql.Driver
      fetchSize: 20000
    destination:
      writeUrl: "jdbc:postgresql://athena.openlattice.com:30001/example_integration?ssl=true&sslmode=require"
      writeDriver: org.postgresql.Driver
      writeTable: demo_health_subset_OLcopy
      hikari: 
        user: "example_user"
        password: "examplepassword"
```  


### Connecting to & uploading .csv files
To connect to one or more .csv files on your computer, the YAML file paramters differ only slightly. One can simply put n/a in for the `sql:`, source server `user` and `password` lines since one is not connecting to a server. In the line specifying `url:`, type in the pathway on your computer to the files to be uploaded. Under `driver:`, type in `com.openlattice.launchpad.Csv`. Below illustrates what YAML parameters would be for uploading a sample dataset.  

```yaml
name: "csv_tutorialtransfer"
description: "transferring csv files to OL servers"
integrations:
  - name: demo_justice
    source:
      url: "/Users/kimengie/Dev/loomhelp/assets/guides/integrations/demo_justice.csv"
      sql: "n/a"
      user: "n/a"
      password: "n/a"
      driver: com.openlattice.launchpad.Csv
      fetchSize: 20000
    destination:
      writeUrl: "jdbc:postgresql://athena.openlattice.com:30001/example_integration?ssl=true&sslmode=require"
      writeDriver: org.postgresql.Driver
      writeTable: demo_justice
      hikari: 
        user: "example_user"
        password: "examplepassword"
```



<div style="color:black; border: 1px solid black; padding: 10px; background-color: yellow; border-radius:5px; text-align: center;">Once your YAML file is created, one simply needs to run it against OpenLattice's <a href="https://github.com/openlattice/launchpad">launchpad</a> java script to read in the desired files to the OpenLattice platform. Please contact <a href="mailto:support@topenlattice.com">support@openlattice.com</a> for custom help with these next steps. </div><br>



