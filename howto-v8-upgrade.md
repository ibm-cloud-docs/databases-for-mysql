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

## MySQL 8 New Deployments
{: #mysql8-ga-new-deployments}

New clients using MySQL 8 on {{site.data.keyword.cloud_notm}} Databases should be able to take a backup of existing version 5.7/8 database instances and import them into version 8. 
You can use mysqldump or mydumper to export your 5.7/8 databases. For more information, see [Migrating to Databases for MySQL](https://cloud.ibm.com/docs/databases-for-mysql?topic=databases-for-mysql-migrating).

## Existing IBM Cloud MySQL 5.7 deployments
{: #mysql8-ga-existing-deployments}

At present, in-place upgrades are not supported for clients running MySQL v5.7. However, {{site.data.keyword.cloud_notm}} Databases is working on providing additional documentation that will provide necessary guidance on a migration path. Until such time, clients running v5.7 should take a backup of existing databases and import them into new MySQL 8 instances. 

As a best practice, retain your current v5.7 backups until the version 8 import is evaluated. Use mysqldump or mydumper to export your 5.7.x databases. For more information, see [Migrating to Databases for MySQL](https://cloud.ibm.com/docs/databases-for-mysql?topic=databases-for-mysql-migrating).

Once your upgrade is complete, changes cannot be reverted. The changes are incompatible, and you cannot use the data directory from MySQL 8.0 on MySQL 5.7. Clients should retain the backup of MySQL 5.7, as it helps to restore it on a MySQL 5.7 instance, should the changes be reversed.{: .note}

### Upgrade Prerequisites
{: #mysql8-ga-existing-deployments}

Before attempting any upgrades, please refer to MySQL's [Preparing Your Installation for Upgrade](https://dev.mysql.com/doc/refman/8.0/en/upgrade-prerequisites.html){: .external} to ensure upgrade readiness.
