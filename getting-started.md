---

copyright:
  years: 2021, 2023
lastupdated: "2023-04-28"

keywords: mysql workbench, mysql gui, mysql

subcollection: databases-for-mysql

content-type: tutorial
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Getting Started
{: #getting-started}
{: toc-content-type="tutorial"}
{: toc-completion-time="30m"}

This tutorial is a short introduction to using an {{site.data.keyword.databases-for-mysql_full}} deployment. To get started, create a connection to [MySQL Workbench](https://www.mysql.com/products/workbench/){: .external}, an open source administration platform for MySQL that provides many tools for managing your data and databases. [Download and install](https://dev.mysql.com/downloads/workbench/){: .external} the version that is appropriate to your environment and then consult MySQL's [documentation](https://dev.mysql.com/doc/workbench/en/wb-mysql-connections.html){: .external} to connect and manage your {{site.data.keyword.databases-for-mysql}} deployment.


## {{site.data.keyword.databases-for-mysql_full}}
{: #mysql-product-description}

{{site.data.keyword.databases-for-mysql_full}} is a serverless, cloud database service that is fully integrated into the IBM Cloud environment. Use {{site.data.keyword.databases-for-mysql}} as a cloud database system without purchasing and setting up your own hardware, installing your own database software, or managing the database yourself.

{{site.data.keyword.databases-for-mysql_full}} requires no software, infrastructure, network, or OS administration. IBM continuously provides fully automated and automatic updates to the service, such as security patches and minor version upgrades. A database instance is deployed by default as highly available across multiple data centers in an {{site.data.keyword.cloud_notm}} Multi-Zone region with [semisynchronous replication](https://dev.mysql.com/doc/mysql-replication-excerpt/8.0/en/replication-semisync.html){: .external}. Connect to a single database endpoint and IBM automatically manages the failover between Availability Zones. {{site.data.keyword.databases-for-mysql_full}} allows you to horizontally scale your MySQL instance with Read Replicas in region or cross-regionally. {{site.data.keyword.databases-for-mysql}} Read Replicas can be easily transformed into fully functioning {{site.data.keyword.databases-for-mysql}} instances, an especially useful feature for online cross-regional disaster recovery strategies.

Additionally, {{site.data.keyword.databases-for-mysql}} provides independent scaling of disk, RAM, and vCPU, as well as auto-scaling capabilities and hourly billing. These features help provide greatly increase granularity on right-sizing database use for application workload.

{{site.data.keyword.databases-for-mysql}} is a multi-tenant offering by design and you have multiple levers for increased isolation that is detailed in our [Security and Compliance section](/docs/cloud-databases?topic=cloud-databases-manage-security-compliance). For example, configuring a database with vCPUs (referred to as Dedicated Cores) introduces hypervisor-level isolation. Alternatively, if that particular lever of isolation is not necessary, you can configure databases so you pay solely for RAM and disk capacity. There are no restrictions on movement between these modes and it is an online activity to introduce or remove your usage of Dedicated Cores.

## Before you begin
{: #mysql-begin}
{: step}

- You need an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration).
- You need a {{site.data.keyword.databases-for-mysql}} deployment. Provision one from the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog/services/databases-for-mysql). Give your deployment a memorable name that appears in your account's Resource List.
- Set the [Admin Password](/docs/databases-for-mysql?topic=databases-for-mysql-user-management&interface=ui#user-management-set-admin-password-ui) for your deployment.
- Install [MySQL Workbench](https://dev.mysql.com/downloads/workbench/){: .external}.
- Review the [`Getting to production`](/docs/cloud-databases?topic=cloud-databases-best-practices) documentation for general guidance on setting up a basic {{site.data.keyword.databases-for-mysql_full}} deployment.

## Connect to your database with the CLI
{: #mysql-connect-db-cli}
{: step}

For the appropriate commands to connect to your database from the CLI, see [Cloud Databases CLI Reference](https://cloud.ibm.com/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference) and [Connecting with mysql](/docs/databases-for-mysql?topic=databases-for-mysql-connecting-mysql).

The `ibmcloud cdb deployment-connections` command handles everything that is involved in creating a command-line client connection. For example, to connect to a deployment named "example-mysql", use a command like:

```sh
ibmcloud cdb deployment-connections example-mysql --start
```
{: pre}

The command prompts for the admin password and then runs the `mysql` command-line client to connect to the database. For more information, see [Connecting with mysql](/docs/databases-for-mysql?topic=databases-for-mysql-connecting-mysql).

## Connect with MySQL Workbench
{: #mysql-connect-db-workbench}

For more information, see [Connections in MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-mysql-connections.html){: .external}.

## Next Steps
{: #mysql-next-steps}

If you are using MySQL for the first time, see the [MySQL 5.7 Reference Manual](https://dev.mysql.com/doc/refman/5.7/en/){: .external}. 

You can connect, manage your databases, and manage data with MySQL's command-line interface (CLI) tool [`mysql`](/docs/databases-for-mysql?topic=databases-for-mysql-connecting-mysql).

Looking for more tools on managing your deployment? Connect to your deployment with the [IBM Cloud CLI](/docs/cli?topic=cli-install-ibmcloud-cli), the [Cloud Databases CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference), or the [Cloud Databases API](https://cloud.ibm.com/apidocs/cloud-databases-api).

If you plan to use {{site.data.keyword.databases-for-mysql}} for your applications, check out [Connecting an external application](/docs/databases-for-mysql?topic=databases-for-mysql-external-app) and [Connecting an IBM Cloud application](/docs/databases-for-mysql?topic=databases-for-mysql-ibmcloud-app).

To ensure the stability of your applications and your database, check out [High-Availability](/docs/databases-for-mysql?topic=cloud-databases-ha-dr) and [Performance](/docs/databases-for-mysql?topic=databases-for-mysql-performance).
