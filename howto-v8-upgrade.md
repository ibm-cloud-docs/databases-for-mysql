---

copyright:
  years: 2021, 2025
lastupdated: "2025-12-19"

keywords: mysql 8.4, mysql

subcollection: databases-for-mysql

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.databases-for-mysql}} 8.4 GA
{: #mysql8.4-ga}

As part of our commitment to offering a rich and mature cloud database portfolio, {{site.data.keyword.databases-for} is releasing MySQL version 8.4. 

## New Databases for MySQL deployments
{: #mysql8.4-ga-new-deployments}

For new {{site.data.keyword.databases-for} deployments, take a backup of your existing v8.0 instance and import it into a v8.4 deployment. Use tools such as mysqldump or mydumper to import into {{site.data.keyword.databases-for-mysql}} v8.4 deployment. For more information, see [Migrating to Databases for MySQL](/docs/databases-for-mysql?topic=databases-for-mysql-migrating).

## Existing {{site.data.keyword.databases-for-mysql}} v8.0 deployments
{: #mysql8.4-ga-existing-deployments}

To move existing v8.0 deployments on {{site.data.keyword.databases-for-mysql_full}} to the new v8.4 major version, you must perform a major version upgrade. The recommended path is to [restore a backup](/docs/databases-for-mysql?topic=databases-for-mysql-dashboard-backups&interface=ui#restore-backup) of your existing v8.0 deployment into a new deployment running MySQL v8.4. You can initiate this upgrade process using the UI, CLI, or API. For detailed, step-by-step instructions on performing this major version upgrade, see [Upgrading to a new major version](https://cloud.ibm.com/docs/databases-for-mysql?topic=databases-for-mysql-mysql-upgrading&interface=ui).

Once your upgrade is complete, changes cannot be reverted. The changes are incompatible, and you cannot use the data directory from MySQL 8.4 on MySQL 8.0. Retain your MySQL v8.0 backup, as it helps to restore it on a MySQL v8.0 instance should the changes need to be reversed.
{: .note}

### Upgrade prerequisites
{: #mysql8.4-ga-existing-deployments}

Before attempting any upgrades, refer to MySQL's [Preparing your installation for upgrade](https://dev.mysql.com/doc/refman/8.4/en/upgrade-prerequisites.html){: .external} to ensure upgrade readiness.

## Introduction to MySQL 8.4 LTS
{: #mysql8.4-ga-intro-lts}

MySQL 8.4 is the new Long-Term Support (LTS) release. As an LTS version, its features are stable and locked after the initial 8.4.0 release. With MySQL 8.0 reaching End-of-Life, migrating to MySQL 8.4 is crucial for continued support, bug fixes, and security updates.

### Key feature changes in {{site.data.keyword.databases-for-mysql}} 8.4
{: #mysql8.4-ga-key-feature-changes}

{{site.data.keyword.databases-for-mysql}} introduces the following new features for database instances running MySQL version 8.4:

* **Authentication:** MySQL 8.4 mandates caching_sha2_password as the default authentication plugin, removing the default_authentication_plugin variable entirely. Customers currently using mysql_native_password must migrate their accounts to caching_sha2_password to prevent connection failures. For more information, see [Section 8.4.1.1, *Native pluggable authentication*](https://dev.mysql.com/doc/refman/8.4/en/native-pluggable-authentication.html){: .external}.
* **MySQL 8.4: Replication terminology updates:** MySQL 8.4 enforces inclusive language by removing non-inclusive terms, such as *Master* and *Slave* from the SQL syntax, meaning deprecated replication SQL statements are no longer supported and will cause syntax errors. For example, use `SHOW BINARY LOG STATUS` instead of `SHOW MASTER STATUS` and use `SHOW REPLICA STATUS` instead of `SHOW SLAVE STATUS`. Automation and configurations should be updated to use these new names. A full list of statement changes is here: [What Is New in MySQL 8.4 since MySQL 8.0](https://dev.mysql.com/doc/refman/8.4/en/mysql-nutshell.html){: .external}.
* **Parameter behavior changes:**
  * **binlog_transaction_compression** defaults to ON to reduce network transfer time and storage usage.
  * **replica_parallel_workers** defaults to 200 to significantly increase concurrency for faster transaction replay on replicas.
  * **innodb_flush_log_at_trx_commit** defaults to 1 to ensure maximum data durability on the primary node.
  * **binlog_transaction_compression_level_zstd** defaults to 3 to balance compression efficiency with CPU overhead.
* **Deprecated or removed parameters:**
  * `AUTO_INCREMENT`: MySQL 8.4 no longer supports `AUTO_INCREMENT` on `FLOAT` or `DOUBLE` columns. Before upgrading, you must modify any tables using this combination to prevent upgrade failures.
  * Removed replication statements and variables: A wide array of *MASTER*/SLAVE* commands and status variables have been removed. It means that any scripts or tools that rely on these status variables will no longer function. For a full list of removed statement and variables, see [What is new in MySQL 8.4 since MySQL 8.0](https://dev.mysql.com/doc/refman/8.4/en/mysql-nutshell.html){: .external}.
  * Authentication variables: The default_authentication_plugin variable is now removed.
* **Notable changes:**
  * New reserved keywords: MySQL 8.4 reserves keywords such as `MANUAL`, `PARALLEL`, `QUALIFY`, and `TABLESAMPLE`. If your schema uses these as unquoted identifiers, queries will fail after the upgrade. Audit and quote these identifiers before upgrading.
  * Spatial Index Upgrade Requirement: A known issue in the upgrade path can cause corruption in spatial indexes. To ensure data integrity, we recommend dropping all spatial indexes before the upgrade and re-creating them immediately after the migration is complete.
  * Automatic histogram updates: MySQL 8.4 introduces automatic histogram updates, enhancing query optimization by removing the need for manual updates. To enable automatic update on a specific histogram, simply run `ANALYZE TABLE UPDATE HISTOGRAM` with the `AUTO UPDATE` option.
  * Privilege enhancements: The `SET_USER_ID` privilege has been replaced by `SET_ANY_DEFINER` and `ALLOW_NONEXISTENT_DEFINER`.
  * GTIDs: Introduces the ability to tag transaction groups with GTIDs for enhanced tracking.

For a list of all MySQL 8.4 features and changes, see [What Is New in MySQL 8.4 since MySQL 8.0](https://dev.mysql.com/doc/refman/8.4/en/mysql-nutshell.html){: .external} in the MySQL documentation.

## Semi-sync to async transition
{: #mysql8.4-ga-semi-sync-async}

When upgrading from {{site.data.keyword.databases-for-mysql}} v8.0 to v8.4, the default replication architecture is transitioned from semi-synchronous to asynchronous replication. This change improves write performance and resilience by ensuring that the primary node does not stall waiting for replica acknowledgments, making your database environment more robust and consistently fast. This enhances high availability by reducing failover times and improving overall system responsiveness. For more information, see [Understanding high availability and disaster recovery for {{site.data.keyword.databases-for-mysql}}](/docs/databases-for-mysql?topic=databases-for-mysql-mysql-ha-dr&interface=cli).
