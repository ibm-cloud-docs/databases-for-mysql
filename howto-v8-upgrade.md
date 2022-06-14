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

As part of our commitment to offering a rich and mature cloud database portfolio, ICD is releasing MySQL version 8. 

## MySQL 8 New Deployments
{: #mysql8-ga-new-deployments}

New customers using MySQL 8 on IBM Cloud should be able to take a backup of their existing version 5.7.X or version 8 database instances and import them into Version 8 on IBM Cloud. 
Customers can use mysqldump or mydumper to export their 5.7/8 databases. For more information, see [Migrating to Databases for MySQL](https://cloud.ibm.com/docs/databases-for-mysql?topic=databases-for-mysql-migrating).

## Existing IBM Cloud MySQL 5.7 deployments
{: #mysql8-ga-existing-deployments}

In-place upgrades are not supported for IBM customers running v5.7 at the time of MySQL 8 offering. However, IBM is working on providing additional documentation and will follow up with the necessary guidance on the migration path. Until such time, we recommend that clients running v5.7 take a backup of their existing database and import it into a new MySQL 8 instance. As a best practice, clients should retain their current v5.7 backups until the version 8 import is evaluated.
Clients should use mysqldump or mydumper to export their 5.7.x databases. For more information, see [Migrating to Databases for MySQL](https://cloud.ibm.com/docs/databases-for-mysql?topic=databases-for-mysql-migrating).

Once your upgrade is complete, changes cannot be reverted. The changes are incompatible, and customers cannot use the data directory from MySQL 8.0 on MySQL 5.7. Clients should retain the backup of MySQL 5.7, as it helps to restore it on a MySQL 5.7 instance, should the changes be reversed.{: .note}

### Upgrade Prerequisites
{: #mysql8-ga-existing-deployments}

Before attempting any upgrades, please refer to MySQL's [Preparing Your Installation for Upgrade](https://dev.mysql.com/doc/refman/8.0/en/upgrade-prerequisites.html){: .external} to ensure upgrade readiness.
