---

copyright:
  years: 2021, 2022
lastupdated: "2022-10-03"

keywords: mysql workbench, mysql gui, mysql

subcollection: databases-for-mysql

---

{:shortdesc: .shortdesc}
{:external: .external target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Getting Started
{: #getting-started}

This tutorial is a short introduction to using an {{site.data.keyword.databases-for-mysql_full}} deployment. To get started, create a connection to [MySQL Workbench](https://www.mysql.com/products/workbench/){: .external}, an open source administration platform for MySQL that provides many tools for managing your data and databases. [Download and install](https://dev.mysql.com/downloads/workbench/){: .external} the version that is appropriate to your environment and then consult MySQL's [documentation](https://dev.mysql.com/doc/workbench/en/wb-mysql-connections.html){: .external} to connect and manage your {{site.data.keyword.databases-for-mysql}} deployment.


## {{site.data.keyword.databases-for-mysql_full}}
{: #mysql-product-description}

{{site.data.keyword.databases-for-mysql_full}} is a serverless, cloud database service that is fully integrated into the IBM Cloud environment. Use {{site.data.keyword.databases-for-mysql_full}} to use a cloud database system without purchasing and setting up your own hardware, installing your own database software, or managing the database yourself.

{{site.data.keyword.databases-for-mysql_full}} requires no software, infrastructure, network, or OS administration. IBM continuously provides fully automated and automatic updates to the service, such as security patches and minor version upgrades. A database instance is deployed by default as highly available across multiple data centers in an {{site.data.keyword.cloud_notm}} Multi-Zone region with [semisynchronous replication](https://dev.mysql.com/doc/mysql-replication-excerpt/8.0/en/replication-semisync.html){: .external}. Connect to a single database endpoint and IBM automatically manages the failover between Availability Zones. {{site.data.keyword.databases-for-mysql_full}} allows you to horizontally scale your MySQL instance with Read Replicas in region or cross-regionally. {{site.data.keyword.databases-for-mysql_full}} Read Replicas can be easily transformed into fully functioning {{site.data.keyword.databases-for-mysql_full}} instances, an especially useful feature for online cross-regional disaster recovery strategies.

Additionally, {{site.data.keyword.databases-for-mysql_full}} provides independent scaling of disk, RAM, and vCPU, as well as auto-scaling capabilities and hourly billing. These features help provide greatly increase granularity on right-sizing database use for application workload.

{{site.data.keyword.databases-for-mysql_full}} is a multi-tenant offering by design and you have multiple levers for increased isolation that is detailed in our [Security and Compliance section](/docs/cloud-databases?topic=cloud-databases-manage-security-compliance). For example, configuring a database with vCPUs (referred to as Dedicated Cores) introduces hypervisor-level isolation. Alternatively, if that particular lever of isolation is not necessary, you can configure databases so you pay solely for RAM and disk capacity. There are no restrictions on movement between these modes and it is an online activity to introduce or remove your usage of Dedicated Cores.

## Before you begin
{: #mysql-begin}

- You need to have an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration).
- You need a {{site.data.keyword.databases-for-mysql}} deployment. You can provision one from the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog/services/databases-for-mysql). Give your deployment a memorable name that appears in your account's Resource List.
- Set the [Admin Password](/docs/databases-for-mysql?topic=databases-for-mysql-admin-password) for your deployment.
- You need an installation of [MySQL Workbench](https://dev.mysql.com/downloads/workbench/){: .external}.
- Review the [`Getting to production`](/docs/cloud-databases?topic=cloud-databases-best-practices) documentation for general guidance on setting up a basic {{site.data.keyword.databases-for-mysql_full}} deployment.

## Connecting to your database with the CLI
{: #mysql-connect-db-cli}

There are two main documentation locations that reference the appropriate commands to connect to your database from the CLI:
- The [Cloud Databases CLI Reference](https://cloud.ibm.com/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference) document. 
- The [Connecting with mysql](/docs/databases-for-mysql?topic=databases-for-mysql-connecting-mysql) document 

The `ibmcloud cdb deployment-connections` command handles everything that is involved in creating a command-line client connection. For example, to connect to a deployment named "example-mysql", use the following command:

```sh
ibmcloud cdb deployment-connections example-mysql --start
```
{: pre}

The command prompts for the admin password and then runs the `mysql` command-line client to connect to the database. For more information, see [Connecting with mysql](/docs/databases-for-mysql?topic=databases-for-mysql-connecting-mysql).

## Connecting with MySQL Workbench
{: #mysql-connect-db-workbench}

For more information, see [Connections in MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-mysql-connections.html){: .external}.

## Next Steps
{: #mysql-next-steps}

If you are using MySQL for the first time, it is a good idea to take a tour through the [MySQL 5.7 Reference Manual](https://dev.mysql.com/doc/refman/5.7/en/){: .external}. 

You can connect, manage your databases, and manage data with MySQL's command-line interface (CLI) tool [`mysql`](/docs/databases-for-mysql?topic=databases-for-mysql-connecting-mysql).

Looking for more tools on managing your deployment? You can connect to your deployment with [IBM Cloud CLI](/docs/cli?topic=cli-install-ibmcloud-cli) and the [Cloud Databases CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference), or use the [Cloud Databases API](https://cloud.ibm.com/apidocs/cloud-databases-api).

If you are planning to use {{site.data.keyword.databases-for-mysql}} for your applications, check out some of our other documentation pages:
- [Connecting an external application](/docs/databases-for-mysql?topic=databases-for-mysql-external-app)
- [Connecting an IBM Cloud application](/docs/databases-for-mysql?topic=databases-for-mysql-ibmcloud-app)

Also, to ensure the stability of your applications and your database, check out the pages on: 
- [High-Availability](/docs/databases-for-mysql?topic=cloud-databases-ha-dr)
- [Performance](/docs/databases-for-mysql?topic=databases-for-mysql-performance)


## Creating an {{site.data.keyword.databases-for-mysql_full}} instance Tutorial
{: #mysql-create-instance-tutorial}

### Objectives
{: #mysql-create-instance-tutorial}

This tutorial guides you through the process of deploying an {{site.data.keyword.databases-for}} instance and connecting it to a web front-end. You will create a webpage that allows visitors to input a word and its definition whose values are then stored in a database or message queue running on {{site.data.keyword.databases-for}}. You will install the database infrastructure using [Terraform](https://www.terraform.io/){: external} and your web application will use the popular [Express](https://www.terraform.io/){: external} framework. The application can then be run locally, or by using [Docker](https://www.docker.com/){: external}.

### Getting productive 
{: #mysql-create-instance-tutorial-getting-started}

To begin the deployment process, install some must-have productivity tools:

* You need to have an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration).
* [Node.js and npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm){: external} - to install packages to and from public npm registries
* [Terraform](https://www.terraform.io/){: external} - to codify and deploy infrastructure
* *optional* [Docker](https://www.docker.com/){: external} - to run your application non-locally

### Step 1: Obtain an API key to deploy infrastructure to your account
{: #mysql-create-instance-tutorial-step-1}

Follow [these steps](https://cloud.ibm.com/docs/account?topic=account-userapikey&interface=ui#create_user_key) to create an {{site.data.keyword.cloud_notm}} API key.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.{.tip}

### Step 2: Clone the project
{: #mysql-create-instance-tutorial-step-2}

```sh
git clone https://github.com/IBM-Cloud/clouddatabases-helloworld-examples.git
```
{.pre}

### Step 3: Install the infrastructure
{: #mysql-create-instance-tutorial-step-2}

Go into the terraform folder in the of the database example you want to run (e.g. mysql).

Create a document called terraform.tfvars with the following fields:

```sh
ibmcloud_api_key = "<your_api_key_from_step_1>"
region = "eu-gb"
admin_password  = "<make_up_a_password>"
```
{.pre}

The terraform.tfvars document contains variables that you may want to keep secret so it is ignored by the GitHub repository.

Now install the infrastructure by typing:

```sh
terraform init 
terraform apply --auto-approve
```
{.pre}

The Terraform script will output some configuration data that will be needed to run the application, so copy it into the root folder:

```sh
terraform output -json >../config.json
```
{.pre}