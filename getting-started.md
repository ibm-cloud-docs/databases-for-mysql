---

copyright:
  years: 2021
lastupdated: "2021-04-12"

keywords: pgAdmin, MySql gui

subcollection: databases-for-mysql

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}


# Getting Started
{: #getting-started}

This tutorial is a short introduction to using an {{site.data.keyword.databases-for-mysql_full}} deployment. [MySQL Workbench](https://www.mysql.com/products/workbench/) is an open-source administration platform for MySQL, and provides many tools for managing your data and databases. [Download and install](https://dev.mysql.com/downloads/workbench/) the version that is appropriate to your environment, and then follow the steps to connect it to your {{site.data.keyword.databases-for-mysql}} deployment.

## Before you begin

- You need to have an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){:new_window}.
- You need a {{site.data.keyword.databases-for-mysql}} deployment. You can provision one from the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog/services/databases-for-mysql). Give your deployment a memorable name that appears in your account's Resource List.
- [Set the Admin Password](/docs/databases-for-mysql?topic=databases-for-mysql-admin-password) for your deployment.
- An installation of [MySQL Workbench](https://dev.mysql.com/downloads/workbench/).

Review the [`Getting to production`](/docs/cloud-databases?topic=cloud-databases-best-practices) documentation for general guidance on setting up a basic {{site.data.keyword.databases-for-mysql_full}} deployment.
## Connecting to your database with the CLI

There are two main documentation locations that reference the appropriate commands to connect to your database from the CLI:
- The [Cloud Databases CLI Reference](https://cloud.ibm.com/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference) document. 
- The [Connecting with mysql](/docs/databases-for-mysql?topic=databases-for-mysql-connecting-mysql) document 

The `ibmcloud cdb deployment-connections` command handles everything that is involved in creating a command line client connection. For example, to connect to a deployment named  "example-mysql", use the following command:

```shell
ibmcloud cdb deployment-connections example-mysql --start
```
{: pre}

The command prompts for the admin password and then runs the `mysql` command line client to connect to the database. If you have not installed the cloud databases plug-in, review the [Connecting with mysql documentation here](/docs/databases-for-mysql?topic=databases-for-mysql-connecting-mysql) for more detailed connection information.

## Connecting with MySQL Workbench

## Next Steps

If you are just using MySQL for the first time, it is a good idea to take a tour through the [official MySQL documentation](https://dev.mysql.com/doc/). 

You can connect to and manage your databases and data with MySQL's command-line tool [`mysql`](/docs/databases-for-mysql?topic=databases-for-mysql-connecting-mysql).

Looking for more tools on managing your deployment? You can connect to your deployment with [IBM Cloud CLI](/docs/cli?topic=cli-install-ibmcloud-cli) and the [Cloud Databases CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference). Or use the [Cloud Databases API](https://cloud.ibm.com/apidocs/cloud-databases-api).

If you are planning to use {{site.data.keyword.databases-for-mysql}} for your applications, check out some of our other documentation pages.
- [Connecting an external application](/docs/databases-for-mysql?topic=databases-for-mysql-external-app)
- [Connecting an IBM Cloud application](/docs/databases-for-mysql?topic=databases-for-mysql-ibmcloud-app)

Also, to ensure the stability of your applications and your database, check out the pages on 
- [High-Availability](/docs/databases-for-mysql?topic=databases-for-mysql-high-availability)
- [Performance](/docs/databases-for-mysql?topic=databases-for-mysql-performance)


