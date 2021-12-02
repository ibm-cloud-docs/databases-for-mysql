---

copyright:
  years: 2021
lastupdated: "2021-12-02"

keywords: mysql workbench, mysql gui

subcollection: databases-for-mysql

---

{:shortdesc: .shortdesc}
{:external: .external target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# {{site.data.keyword.databases-for-mysql_full}}
{: #mysql-product-description}

{{site.data.keyword.databases-for-mysql_full}} is a serverless, cloud database service that is fully integrated into the IBM Cloud environment. This offering lets users access and use a cloud database system without purchasing and setting up their own hardware, installing their own database software, or managing the database themselves.

{{site.data.keyword.databases-for-mysql_full}} requires no software, infrastructure, network, or OS administration. IBM continuously provides fully automated and automatic updates to the service, such as security patches and minor version upgrades. A database instance is deployed by default as highly available across {{site.data.keyword.cloud_notm}} Multi-Zone regions with synchronous replication.
Customers only need to connect to a single database endpoint and IBM manages the failover between Availability Zones automatically. {{site.data.keyword.databases-for-mysql_full}} provides the ability to horizontally scale the MySQL instance with Read Replicas in region or cross-regionally.

{{site.data.keyword.databases-for-mysql_full}} additionally provides independent scaling of disk, RAM, and vCPU, as well as auto-scaling capabilities and hourly billing. These features help provide greatly increase granularity on right-sizing database use for application workload.

{{site.data.keyword.databases-for-mysql_full}} is a multi-tenant offering by design and customers have multiple levers for increased isolation detailed in our [Security and Compliance section](/docs/cloud-databases?topic=cloud-databases-manage-security-compliance). For example, configuring a database with vCPUs (referred to as Dedicated Cores) introduces hypervisor-level isolation. Alternatively, if that particular lever of isolation is not necessary, customers can configure databases to pay solely for RAM and disk capacity. There are no restrictions on movement between these modes and it is an online activity to introduce or remove your usage of Dedicated Cores.

# Getting Started
{: #getting-started}

This tutorial is a short introduction to using an {{site.data.keyword.databases-for-mysql_full}} deployment. To get started, you will want to create a connection to [MySQL Workbench](https://www.mysql.com/products/workbench/), an open-source administration platform for MySQL that provides many tools for managing your data and databases. [Download and install](https://dev.mysql.com/downloads/workbench/) the version that is appropriate to your environment and then consult MySQL's [documentation](https://dev.mysql.com/doc/workbench/en/wb-mysql-connections.html) to connect and manage your {{site.data.keyword.databases-for-mysql}} deployment.

## Before you begin
{: #mysql-begin}

- You need to have an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}.
- You need a {{site.data.keyword.databases-for-mysql}} deployment. You can provision one from the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog/services/databases-for-mysql). Give your deployment a memorable name that appears in your account's Resource List.
- Set the [Admin Password](/docs/databases-for-mysql?topic=databases-for-mysql-admin-password) for your deployment.
- You need an installation of [MySQL Workbench](https://dev.mysql.com/downloads/workbench/).
- Review the [`Getting to production`](/docs/cloud-databases?topic=cloud-databases-best-practices) documentation for general guidance on setting up a basic {{site.data.keyword.databases-for-mysql_full}} deployment.

## Connecting to your database with the CLI
{: #mysql-connect-db-cli}

There are two main documentation locations that reference the appropriate commands to connect to your database from the CLI:
- The [Cloud Databases CLI Reference](https://cloud.ibm.com/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference) document. 
- The [Connecting with mysql](/docs/databases-for-mysql?topic=databases-for-mysql-connecting-mysql) document 

The `ibmcloud cdb deployment-connections` command handles everything that is involved in creating a command line client connection. For example, to connect to a deployment named  "example-mysql", use the following command:

```shell
ibmcloud cdb deployment-connections example-mysql --start
```
{: pre}

The command prompts for the admin password and then runs the `mysql` command line client to connect to the database. If you haven't installed the cloud databases plug-in, review the [Connecting with mysql](/docs/databases-for-mysql?topic=databases-for-mysql-connecting-mysql) documentation for more detailed connection information.

## Connecting with MySQL Workbench
{: #mysql-connect-db-workbench}

To connect with MySQL Workbench, refer to MySQL's [Connections in MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-mysql-connections.html) documentation.

## Next Steps
{: #mysql-next-steps}

If you are using MySQL for the first time, it is a good idea to take a tour through the [MySQL 5.7 Reference Manual](https://dev.mysql.com/doc/refman/5.7/en/). 

You can connect, manage your databases, and manage data with MySQL's command-line tool [`mysql`](/docs/databases-for-mysql?topic=databases-for-mysql-connecting-mysql).

Looking for more tools on managing your deployment? You can connect to your deployment with [IBM Cloud CLI](/docs/cli?topic=cli-install-ibmcloud-cli) and the [Cloud Databases CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference), or use the [Cloud Databases API](https://cloud.ibm.com/apidocs/cloud-databases-api).

If you are planning to use {{site.data.keyword.databases-for-mysql}} for your applications, check out some of our other documentation pages:
- [Connecting an external application](/docs/databases-for-mysql?topic=databases-for-mysql-external-app)
- [Connecting an IBM Cloud application](/docs/databases-for-mysql?topic=databases-for-mysql-ibmcloud-app)

Also, to ensure the stability of your applications and your database, check out the pages on: 
- [High-Availability](/docs/databases-for-mysql?topic=cloud-databases-ha-dr)
- [Performance](/docs/databases-for-mysql?topic=databases-for-mysql-performance)
