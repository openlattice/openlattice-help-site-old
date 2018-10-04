---
layout: page
title: Integration Pipeline: Secure Data Transfers & Speed
description: Maximize both security and performance by using our **Launchpad** application to transfer data from your database directly and securely into OpenLattice. This is the recommended data pipeline for using the OpenLattice platform. Reach out to support@openlattice.com if you need any assistance with your integration.
weight: 1
---

* TOC
{:toc}

## 1. Balancing security & performance 
The desire to maximize security and maintain control over one's data should not prevent one from taking full advantage of OpenLattice's ability to perform fast and accurate data integrations for you.
* Writing and running integrations on your own server (integration tutorials 1 and 2) eliminates the need for any data transfers, but may impact your computer system's performance and memory depending on your processing power, especially for large files or real-time, recurring integrations.
* Running integrations on OpenLattice servers can be faster by orders of magnitude.

## 2. Launchpad
Data transfers are obviously integral to using the OpenLattice platform, and we take very seriously the need to provide the highest level of security and transparency. For these reasons we have developed a secure and completely open access solution.

Our [Launchpad](https://github.com/openlattice/launchpad) application eliminates the need for and insecurity of email file transfers by allowing you to transfer whole tables or subsets of tables, from your computer or SQL server directly to an OpenLattice server for later integration. Some advantages of this workflow include:
* Speeding up integrations by transferring the computing power needed away from client-side computer servers over to OpenLattice servers.
* If OpenLattice is assisting with your integrations, security is enhanced by reducing the number of software platforms in the workflow process, via eliminating the need to utilize a third-party system (e.g., Dropbox) for transfer of individual files over to OpenLattice. 
* One maintains full control over the data to be integrated from your server into the OpenLattice platform, including any changes over time.
* Eliminating the need for client-side creation and storage of extra .csv files to send to OpenLattice.

To give an overview of the process, a temporary table is copied over from a client-side server into OpenLattice's "Atlas" production server, which is set up on [AWS GovCloud](https://docs.aws.amazon.com/govcloud-us/latest/UserGuide/whatis.html).  OpenLattice then runs all data integrations on our side, on these temporary tables. For those that want continuous integrations (for instance, to add to and update CAD police data or behavioral health reports as they come in), the data transfer can be set up as a recurring task on the client-side computer to run at a set time period and over-write the table that exists on the OpenLattice server, which is then integrated in parallel. These overwrites also ensure that the tables copied onto the OpenLattice side are temporary.

Your data would be stored in a private database accessible only to you - **please contact <a href="mailto:support@topenlattice.com">support@openlattice.com</a> for arrangements.** Later, you can still write your own integration script if desired and also put it onto OpenLattice servers to run automatically. A tutorial for this final step is being developed. 

## 3. Setting up your data transfer 
First, one creates a short configuration file using [YAML](https://en.wikipedia.org/wiki/YAML). This configuration file will reside on your computer. It tells your server which table(s) to collect, where to put it on OpenLattice's platform (inside a specific database that will be set up for your jurisdiction), and what to name the table on OpenLattice's side. All of these factors are under your control. 

To create your .yaml file, use a text editor such as NotePad, TextEdit, [**Sublime**](https://www.sublimetext.com/) or [**Atom**](https://atom.io/). 

<span class="bad">NOTE:</span> _A.yaml file must be formatted very exactly, down to the number of spaces there are away from the left margin in each line. Please follow the instructions below exactly. We recommend downloading the sample, formatted yaml file [**HERE**](/assets/guides/integrations/integration_tutorial.yaml), and replacing the parameters as needed._

One can either (1) connect to your servers and copy over various tables, or (2) copy over .csv files.  

### Connecting to a SQL server via JDBC driver

To connect your server, download the YAML file example [**HERE**](/assets/guides/integrations/integration_tutorial.yaml), and change the text between the `<>` characters in each line as needed to fit your data parameters. Below is an explanation of what to write in each line of the file. 

```yaml
name: "<Name of integration for recordkeeping only (not stored anywhere in tables).>"
description: "<Description for recordkeeping (not stored anywhere in tables).>"
datasources:
- name: <the name of the remote database>
  url: "<the url to the client-side server>"
  username: "<username for entry to client-side server>"
  password: "<password to client-side server>"
  driver: org.postgresql.Driver
  fetchSize: <a parameter that dictates how many rows are written in to OpenLattice at a time>
destinations:
- name: <the name of the local database>
  url: "jdbc:postgresql://atlas.openlattice.com:30001/YOUR_DATABASE_NAME_HERE?ssl=true&sslmode=require"
  driver: org.postgresql.Driver
  username: "<your_username to your OpenLattice database>"
  password: "<your_password to your OpenLattice database>"
integrations:
  <The name of the remote database>:
    <The name of the local database>:
      - source: "<SQL query that defines the data subset - must be a table or a subquery>"
        destination: <name of table copy that will be put on OpenLattice server>
```

Below are two examples of how parameters would be filled out to transfer a file onto the OpenLattice platform. We use our sample healthcare and criminal justice demo datasets, which are available to view on the [**OpenLattice gallery**](https://openlattice.com); these data are synthetic and do not contain any real people. In this example we are copying these into a training database on the OpenLattice server calld "example_integration". The `sql:` lines must specify either an entire table, or a subquery. 
* If one wanted to copy over an entire table, in the `sql:` line one would simply specify the table name, such as `"demo_justice"` in the first example below. 
* If one wanted to query a subset of data, one needs to make it a subquery as is shown in the second example below, for a hypothetical situation where one wanted to copy over only records for people named "Jennifer". Note the `\"` needed surrounding a column name if it contains capital letters.  

NOTE in this example the table is being read from and copied to the same database. In a real situation, the `source` and `destination` URLs will differ.

```yaml
name: "example_filetransfer"
description: "Copying over data from demo health table into OpenLattice server"
datasources:
- name: example_integration
  url: "jdbc:postgresql://atlas.openlattice.com:30001/example_integration?ssl=true&sslmode=require"
  username: "example_user"
  password: "examplepassword"
  driver: org.postgresql.Driver
  fetchSize: 20000
destinations:
 - name: example_integration
   url: "jdbc:postgresql://atlas.openlattice.com:30001/example_integration?ssl=true&sslmode=require"
   driver: org.postgresql.Driver
   username: "example_user"
   password: "examplepassword"
integrations:
  example_integration:
    example_integration:
      - source: "demo_justice"
        destination: demo_justice_OLcopy
      - source: "( select * from demo_health where \"FirstName\" = 'Jennifer') dh"
        destination: demo_health_subset_OLcopy
```  


### Connecting to & uploading .csv files
To connect to one or more .csv files on your computer, the YAML file paramters differ only slightly. One can simply omit the `sql:`, source server `user` and `password` lines found above since one is not connecting to a server. In the line specifying `url:`, type in the pathway on your computer to the files to be uploaded. Under `driver:`, type in `com.openlattice.launchpad.Csv`. Below illustrates what YAML parameters would be for uploading a sample dataset.  

```yaml
name: "csv_tutorialtransfer"
description: "transferring csv files to OL servers"
datasources:
- name: demo_justice_csv
  url: "/Users/kimengie/Dev/loomhelp/assets/guides/integrations/demo_justice.csv"
  driver: com.openlattice.launchpad.Csv
destinations:
- name: example_integration
  url: "jdbc:postgresql://atlas.openlattice.com:30001/example_integration?ssl=true&sslmode=require"
  driver: org.postgresql.Driver
  username: "example_user"
  password: "examplepassword"
integrations:
  demo_justice_csv:
    example_integration:
      - source: ""
        destination: demo_justice
```


## 4. Running your configuration file
Once your YAML file is created, one simply needs to run it against OpenLattice's **"Launchpad"** configuration package to read in the desired files to the OpenLattice platform. To download Launchpad, click [**here**](https://s3.amazonaws.com/openlattice.com/launchpad/launchpad-1.1.0-SNAPSHOT.zip).  FYI, if you would like to view the source code, one can access our github repo [**here**](https://github.com/openlattice/launchpad). Then follow these steps:

### 4a. Unzip the launchpad files into a clean folder. 
Create a new folder anywhere on your computer, and NOT in your downloads folder. Unzip the launchpad files inside.
### 4b. Use launchpad to run your YAML configuration file.
* On a Mac: Go to Terminal, and navigate to the "bin" folder in the launchpad files. Then type: `./launchpad -file <path_to_yamlfile>`. Replace the text between the `<>` characters with the filepath to the .YAML file that you created above, being sure to also remove the `<>` characters. 
* On a PC: Open a command line, navigate to the "bin" folder in the launchpad files. Then type: `./launchpad.bat -file <path_to_yamlfile>`. Replace the text between the `<>` characters with the filepath to the .YAML file that you created above, being sure to also remove the `<>` characters. 



<div style="color:black; border: 1px solid black; padding: 10px; background-color: yellow; border-radius:5px; text-align: center;">Congratulations! You have successfully set up secure data transfers to OpenLattice's servers. It is now a straightforward pathway to setting up recurring, real-time data integrations of any data onto the OpenLattice platform, and for use in OpenLattice's products. Please contact <a href="mailto:support@topenlattice.com">support@openlattice.com</a> for custom help with these next steps, and any questions. </div><br>



