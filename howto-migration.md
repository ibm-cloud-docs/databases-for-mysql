---
copyright:
  years: 2021
lastupdated: "2022-01-06"

keywords: mysql, databases, migrating

subcollection: databases-for-mysql

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Migrating to {{site.data.keyword.databases-for-mysql}}
{: #migrating}

Various options exist to migrate data from existing MySQL databases to {{site.data.keyword.databases-for-mysql_full}}. We focus on the simplest and most effective. To get started, you need MySQL installed locally so you have the `mysql` and `mysqldump` tools. [MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-admin-export-import-management.html) also provides a graphical tool for working with MySQL servers and databases. While not strictly required, the {{site.data.keyword.databases-for}} CLI also makes it easier to connect and restore to a new {{site.data.keyword.databases-for-mysql}} deployment. 

## mysqldump
{: #migrating-mysqldump}

On your source database run `mysqldump` to create an SQL file, which can be used to re-create the database. At a minimum, migrating `mysql` using the CLI requires the following arguments:
- host name (`-h` flag)
- port number (`-P` flag)
- user name (`-u` flag) 
- [--ssl-mode=VERIFY_IDENTITY](https://dev.mysql.com/doc/refman/5.7/en/connection-options.html#option_general_ssl-mode) (clients will require an encrypted connection and will perform verification against the server CA certificate and against the server host name in its certificate)
- [--ssl-ca](https://dev.mysql.com/doc/refman/5.7/en/connection-options.html#option_general_ssl-ca) (the path name of the Certificate Authority (CA) file, which can be found within the Endpoints CLI tab of the *Overview* page in the UI.)
- database name
- result file (`-r` flag) 

Your CLI command will look like this:

```shell
mysqldump -h <host_name> -P <port_number> -u <user_name> --ssl-mode=VERIFY_IDENTITY --ssl-ca=mysql.crt --set-gtid-purged=OFF -p ibmclouddb -r dump.sql
```
{: pre}

For more information on using MySQL Replication with Global Transaction Identifiers (GTIDs), consult the [Using GTIDs for Failover and Scaleout](https://dev.mysql.com/doc/refman/5.7/en/replication-gtids-failover.html) in the MySQL Reference Manual.
{: .note} 

The `mysql` command has many options; [consult the official documentation](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html#mysqldump-syntax) and [command reference](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html#mysqldump-option-summary) for a fuller view of its capabilities.

## Restoring mysqldump's output
{: #migrating-mysqldump-restore}

The resulting output of `mysqldump` can then be uploaded into a new {{site.data.keyword.databases-for-mysql}} deployment. As the output is SQL, it can simply be sent to the database through the `mysql` command. We recommend that imports be performed with the admin user. 

See the [Connecting with `mysql`](/docs/databases-for-mysql?topic=databases-for-mysql-connecting-mysql) documentation for details on connecting as admin using `mysql`. To connect with the `mysql` command, you need the admin user's connection string and the TLS certificate, which can both be found in the UI. The certificate needs to be decoded from the base64 and stored as an arbitrary local file. To import the previously created `dump.sql` into a database deployment named `example-mysql`, the `mysql` command can be called with `-f dump.sql` as a parameter. The parameter tells `mysql` to read and execute the SQL statements in the file. 

As noted in the [Connecting with `mysql`](/docs/databases-for-mysql?topic=databases-for-mysql-connecting-mysql) documentation, the {{site.data.keyword.databases-for}} CLI plug-in simplifies connecting. The previous `mysql` import can be performed using a command like: 

```shell
mysql -h <host_name> -P <port_number> -u admin --ssl-mode=VERIFY_IDENTITY --ssl-ca=mysql.crt -p ibmclouddb < dump.sql
```
{: pre}

If no user is specified, the command automatically uses the admin user and interactively prompts for the password. The TLS certificate is automatically retrieved and used.

While the restore process is running, a number of messages are emitted regarding changes being made to the database deployment.
