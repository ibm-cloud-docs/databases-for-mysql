---
copyright:
  years: 2021
lastupdated: "2022-01-28"

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

Various options exist to migrate data from existing MySQL databases to {{site.data.keyword.databases-for-mysql_full}}. We recommend two options: `mysqldump` and `mydumper`. Which tool is best for you depends on certain conditions, including network connection, the size of your data set, and intermediate schema needs. 

## Before you begin
{: #migrating-before-begin}

Before getting started with your data migration, you will need MySQL installed locally so you have the `mysql` and `mysqldump` tools. 

[MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-admin-export-import-management.html) also provides a graphical tool for working with MySQL servers and databases. While not strictly required, the {{site.data.keyword.databases-for}} [CLI](/docs/databases-cli-plugin) also makes it easy to connect and restore to a new {{site.data.keyword.databases-for-mysql}} deployment. 

## Tuning InnoDB Configurable Variables
{: #migrating-config-variables}

You can configure the MySQL InnoDB options to tune performance based on machine capacity and database workload. 

### innodb_buffer_pool_size_percentage
{: #migrating-config-variables_buffer_pool}

The `innodb_buffer_pool_size_percentage value` parameter defines your database container's dedicated memory amount. As your database itself uses a given amount of memory, if the `innodb_buffer_pool_size_percentage value` parameter is configured too high, then your database memory requirements + `innodb_buffer_pool_size_percentage` can become higher than available memory limits, resulting in an out of memory state (OOM).

The `innodb_buffer_pool_size_percentage` parameter value will differ based on the size of your database. The default value is `70%`, which is safe for databases of all sizes. Configure the value as needed; if you encounter OOM then the value is set too high and you should lower it. 

### innodb_flush_log_at_trx_commit
{: #migrating-config-variables-flush-log}

- Description: Controls the balance between strict ACID compliance for commit operations and higher performance

- Default setting: 2

The default setting of 2 is not fully ACID-compliant (Default setting of 1 is required for full ACID compliance), but it is more performant and still is safe.

- Max: 2
- Min: 0
- Requires restart: False

For more information, consult the MySQL innodb_flush_log_at_trx_commit [documentation](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_flush_log_at_trx_commit).

### innodb_log_buffer_size
{: #migrating-config-variables-log-buffer-size}

### innodb_log_file_size
{: #migrating-config-variables-log-file-size}

### innodb_lru_scan_depth
{: #migrating-config-variables-lru-scan-depth}

### innodb_read_io_threads
{: #migrating-config-variables-read-io-threads}

### innodb_write_io_threads
{: #migrating-config-variables-write-io-threads}

### net_read_timeout
{: #migrating-config-variables-net-read-timeout}

### net_write_timeout
{: #migrating-config-variables-net-write-timeout}


## mysqldump
{: #migrating-mysqldump}

This native MySQL client utility installs by default and can perform logical backups, reproducing table structure and data, without copying the actual data files. mysqldump dumps one or more MySQL databases for backup or transfer to another MySQL server. For more information, see the [mysqldump documentation](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html).

mysqldump is appropriate to use under the following conditions:
- The data set is smaller than 10 GB. 
- Migration time is not critical, and the cost of re-trying the migration is very low.
- You don’t need to do any intermediate schema or data transformations.

We don't recommend using mysqldump if any of the following conditions are met: 
- Your data set is larger than 10GB. 
- The network connection between the source and target databases is unstable or very slow.

Follow these steps to perform full data load using the mysqldump tool:

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

### Restoring mysqldump's output
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

## mydumper
{: #migrating-mydumper}

mydumper, and its paired logical backup tool myloader, use multithreading capabilities to perform data migration similarly to mysqldump; however, mydumper provides many improvements such as parallel backups, consistent reads, and easier to manage output. Parallelism allows for better performance during both the import and export process, while output can be easier to manage because individual tables get dumped into separate files. 

For details and step-by-step instructions on installation and necessary developer environment, see the [mydumper project](https://github.com/maxbube/mydumper).

mydumper is appropriate to use under the following conditions:
- The data set is larger than 10 GB. 
- The network connection between source and target databases is fast and stable.
- You need to do intermediate schema or data transformations.

We don't recommend using mysqldump if any of the following conditions are met: 
- Your data set is smaller than 10GB. 
- The network connection between the source and target databases is unstable or very slow.

Here is a basic script for performing a full data load using the mydumper tool:

```shell
#!/bin/sh
file=stack9.tar
mysql --login-path=local -e "DROP DATABASE stackoverflow9 IF EXISTS;"
mysql --login-path=local -e "CREATE DATABASE stackoverflow9"
bucket=my-walg-218
resource="/${bucket}/${file}" 
contentType="application/gzip" 
dateValue=`date -R`
stringToSign="GET\n\n${contentType}\n${dateValue}\n${resource}" 
s3Key=a48508d4a3394a738cf24e6285627ded 
s3Secret=19ede00909c28c6b0c80c8ea7212ebdb5e22da50650a54f1 
signature=`/bin/echo -en "$stringToSign" | openssl sha1 -hmac ${s3Secret} -binary | base64`
curl -H "Host: ${bucket}.s3.private.us.cloud-object-storage.appdomain.cloud" \
 -H "Date: ${dateValue}" \
 -H "Content-Type: ${contentType}" \
 -H "Authorization: AWS ${s3Key}:${signature}" \
 --limit-rate 100M \
 https://${bucket}.s3.private.us.cloud-object-storage.appdomain.cloud/${file} | tar -xf stack9.tar
 myloader --user ibm --socket /data/mysql/5/mysqld.sock --directory=/data/export-20220113-111633
 ```

### Restoring mydumper's output
{: #migrating-mydumper-restore}

As noted in the Connecting with mysql documentation, the {{site.data.keyword.databases-for}} CLI plug-in simplifies connecting. The previous mysql import can be performed using a command like:

```shell
myloader --user ibm --socket /data/mysql/5/mysqld.sock --directory=/data/export-20220113-111633 --verbose 3 --threads=16 --queries-per-transaction=500
```
