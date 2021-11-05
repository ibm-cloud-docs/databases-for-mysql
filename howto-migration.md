---
copyright:
  years: 2021
lastupdated: "2021-04-12"

keywords: mysql, databases, migrating

subcollection: databases-for-mysql

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Migrating to {{site.data.keyword.databases-for-mysql}}
{: #migrating}

Various options exist to migrate data from existing MySQL databases to {{site.data.keyword.databases-for-mysql_full}}. We focus on the simplest and most effective. To get started, you need MySQL installed locally so you have the `mysql` and `mysqldump` tools. [MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-admin-export-import-management.html) also provides a graphical tool for working with MySQL servers and databases. While not strictly required, the {{site.data.keyword.databases-for}} CLI also makes it easier to connect and restore to a new {{site.data.keyword.databases-for-mysql}} deployment. 

## mysqldump

On your source database run `mysqldump` to create an SQL file, which can be used to re-create the database. At a minimum, `mysql` takes a host name (`-h` flag), port number (`-p` flag), database name (`-d` flag), user name (`-U` flag), and a file (or directory name) to write the dump to (`-f` flag). 

For example, the following command dumps the MySQL "compose" database that is hosted on sl-eu-lon-2-portal.4.dblayer.com, port 17980, using the admin user and save the results in `dump.sql`.

```shell
mysqldump -h sl-eu-lon-2-portal.4.dblayer.com -p 17980 -d compose -U admin -f dump.sql
```
{: pre}

The `mysql` command has many options; you should [consult the official documentation](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html#mysqldump-syntax) and [command reference](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html#mysqldump-option-summary) for a fuller view of its capabilities.

## Restoring mysqldump's output

The resulting output of `mysqldump` can then be uploaded into a new {{site.data.keyword.databases-for-mysql}} deployment. As the output is SQL, it can simply be sent to the database through the `mysql` command. We recommend that imports be performed with the admin user. 

See the [Connecting with `mysql`](/docs/allowlist/databases-for-mysql?topic=databases-for-mysql-connecting-mysql) for details on how to connect as admin using `mysql`. To connect with the `mysql` command, you need the admin user's connection string and the TLS certificate. The certificate needs to be decoded from the base64 and stored as an arbitrary local file. To import the previously created `dump.sql` into a database deployment named `example-mysql`, the `mysql` command can be called with `-f dump.sql` as a parameter. The parameter tells `mysql` to read and execute the SQL statements in the file. 

As noted in that [Connecting with `mysql`](/docs/allowlist/databases-for-mysql?topic=databases-for-mysql-connecting-mysql) documentation, the {{site.data.keyword.databases-for}} CLI plug-in simplifies connecting. The previous `mysql` import can be performed as:

```shell
ibmcloud cdb deployment-connections example-mysql -s -- -f dump.sql
```
{: pre}

If no user is specified, the command automatically uses the admin user and interactively prompts for the password. The TLS certificate is automatically retrieved and used. The `-s` starts `mysql` (or whichever command has been configured) once the details are established from the API. Anything after the `--` is passed to the command.

While the restore process is running, a number of messages are emitted about changes being made to the database deployment.
