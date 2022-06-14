---

copyright:
  years: 2021, 2022
lastupdated: "2022-06-14"

keywords: mysql 8, mysql

subcollection: databases-for-mysql

---

{:shortdesc: .shortdesc}
{:external: .external target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}

# MySQL 8 GA
{: #mysql8-ga}

As part of our commitment to offering a rich and mature cloud database portfolio, {{site.data.keyword.cloud_notm}} Databases is releasing MySQL version 8. 

## New {{site.data.keyword.databases-for-mysql_full}} Deployments
{: #mysql8-ga-new-deployments}

For new clients deploying on {{site.data.keyword.cloud_notm}} Databases, take a backup of your existing v5.7/8 database instances and import them into version 8. 
You can then use mysqldump or mydumper to import to {{site.data.keyword.cloud_notm}} Databases. For more information, see [Migrating to Databases for MySQL](https://cloud.ibm.com/docs/databases-for-mysql?topic=databases-for-mysql-migrating).

## Existing {{site.data.keyword.databases-for-mysql_full}} v5.7 deployments
{: #mysql8-ga-existing-deployments}

At present, in-place upgrades are not supported for clients running MySQL v5.7. {{site.data.keyword.cloud_notm}} Databases is creating additional documentation that will provide necessary guidance on a migration path. Until this documentation is released, clients with existing {{site.data.keyword.databases-for-mysql_full}} v5.7 deployments should take the following steps:

1. Create and export a v5.7x backup.
1. Migrate and prepare your backup locally to be compatible with 8.0.
1. Import into ICD MySQL 8.0. 

As a best practice, retain your current v5.7 backups until the version 8 import is evaluated. Use mysqldump or mydumper to export your 5.7.x databases. For more information, see [Migrating to Databases for MySQL](https://cloud.ibm.com/docs/databases-for-mysql?topic=databases-for-mysql-migrating).

Once your upgrade is complete, changes cannot be reverted. The changes are incompatible, and you cannot use the data directory from MySQL 8.0 on MySQL 5.7. Retain your MySQL v5.7 backup, as it helps to restore it on a MySQL 5.7 instance should the changes need to be reversed.{: .note}

### Upgrade Prerequisites
{: #mysql8-ga-existing-deployments}

Before attempting any upgrades, please refer to MySQL's [Preparing Your Installation for Upgrade](https://dev.mysql.com/doc/refman/8.0/en/upgrade-prerequisites.html){: .external} to ensure upgrade readiness.
