---
copyright:
  years: 2021, 2022
lastupdated: "2022-06-08"

keywords: mysql, databases, config, mysql configuaration

subcollection: databases-for-mysql

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Changing the MySQL Configuration
{: #changing-configuration}

{{site.data.keyword.databases-for-mysql_full}} allows you to change some of the MySQL configuration settings so you can tune your MySQL databases to your use-case. To make permanent changes to the database configuration, use the {{site.data.keyword.databases-for}} [cli-plugin](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-configuration) or [API](https://{DomainName}/apidocs/cloud-databases-api#change-your-database-configuration) to write the changes to the configuration file for your deployment.

The configuration is defined in a schema. To make a change, you send a JSON object with the settings and their new values to the API or the CLI.  For example, to set the `max_connections` setting to 150, you would supply 

```sh
{"configuration":{"max_connections":150}}
```
{: .codeblock}

to either the CLI or to the API. 

For more information, see the [Managing MySQL Connections](/docs/databases-for-mysql?topic=databases-for-mysql-managing-mysql-connections){: .external} documentation. 

## Using the CLI
{: #using-cli}

You can check the current configuration of your deployment with 
```sh
ibmcloud cdb deployment-configuration-schema <deployment name or CRN>
```
{: pre}

To change your configuration through the {{site.data.keyword.databases-for}} cli-plugin, use `deployment-configuration` command. 
```sh
ibmcloud cdb deployment-configuration <deployment name or CRN> [@JSON_FILE | JSON_STRING]
```
{: pre}

The command reads the changes that you would like to make from the JSON object or a file. For more information, see the [reference page](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-configuration).

## Using the API
{: #using-api}

There are two deployment-configuration endpoints, one for viewing the configuration schema and one for changing the configuration. To view the configuration schema, send a `GET` request to `/deployments/{id}/configuration/schema`.

To change the configuration, send the settings that you would like to change as a JSON object in the request body of a `PATCH` request to `/deployments/{id}/configuration`.

For more information, see the [API Reference](https://cloud.ibm.com/apidocs/cloud-databases-api#change-your-database-configuration). 


## Available Configuration settings
{: #available-config-settings}

[`max_connections`](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_max_connections){: .external}

- Default - `200`
- Restarts database? - **false**
  
 You might need to [scale before you increase max connections](/docs/databases-for-mysql?topic=databases-for-mysql-high-availability#connection-limits-ha). {: note}

[`mysql_max_binlog_age_sec`](https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html){: .external}

- Default - `1800`
- Restarts database? - **false**

[`default_authentication_plugin`](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html){: .external}

- Default - `sha256_password`
- Allowable values: `sha256_password`, `mysql_native_password`
- Restarts database? - **true**

Unless strictly necessary, don't use `mysql_native_password`. {: note}

[`max_allowed_packet`](https://dev.mysql.com/doc/refman/5.7/en/packet-too-large.html){: .external}

- Default - `16777216`
- Minimum - `1024`
- Maximum - `1073741824`
- Restarts database? - **false**

[`sql_mode`](https://dev.mysql.com/doc/refman/5.7/en/sql-mode.html){: .external}

- Allowable values: 
   - `ALLOW_INVALID_DATES`
   - `ANSI_QUOTES`
   - `ERROR_FOR_DIVISION_BY_ZERO`
   - `HIGH_NOT_PRECEDENCE`
   - `IGNORE_SPACE`
   - `NO_AUTO_CREATE_USER`
   - `NO_AUTO_VALUE_ON_ZERO`
   - `NO_BACKSLASH_ESCAPES`
   - `NO_DIR_IN_CREATE`
   - `NO_ENGINE_SUBSTITUTION`
   - `NO_FIELD_OPTIONS`
   - `NO_KEY_OPTIONS`
   - `NO_TABLE_OPTIONS`
   - `NO_UNSIGNED_SUBTRACTION`
   - `NO_ZERO_DATE`
   - `NO_ZERO_IN_DATE`
   - `ONLY_FULL_GROUP_BY`
   - `PAD_CHAR_TO_FULL_LENGTH`
   - `PIPES_AS_CONCAT`
   - `REAL_AS_FLOAT`
   - `STRICT_ALL_TABLES`
   - `STRICT_TRANS_TABLES`
- Restarts database? - **false**

[`innodb_buffer_pool_size_percentage`](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_buffer_pool_size){: .external}

- Description: The percentage of memory to use for `innodb_buffer_pool_size`. The defaul value of 50% is conservative value and works for databases of any size. If your database requires more RAM, this value can be increased. Setting this value too high can exceed your database's memory limits, which can cause it to crash. 
- Default: `50`
- Minimum: `10`
- Maximum: `100`
- Restarts database? - **true**

[`innodb_lru_scan_depth`](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_lru_scan_depth){: .external}

- Description: A parameter that influences the algorithms and heuristics for the [flush](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_flush){: .external} operation for the InnoDB [buffer pool](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_buffer_pool){: .external}. A setting smaller than the default is generally suitable for most workloads. A value that is much higher than necessary may impact performance. Only consider increasing the value if you have spare I/O capacity under a typical workload. 
- Default: `256`
- Minimum: `128`
- Maximum: `2048`
- Restarts database? - **false**

[`net_write_timeout`](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_net_write_timeout){: .external}

- Description: The number of seconds to wait for a block to be written to a connection before aborting the write.
- Default: `60`
- Minimum: `1`
- Maximum: `7200`
- Restarts database? - **false**

[`innodb_write_io_threads`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_write_io_threads){: .external}

- Description: The number of I/O threads for write operations in InnoDB.
- Default: `4`
- Minimum: `1`
- Maximum: `64`
- Restarts database? - **true**

[`innodb_log_file_size`](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_log_file_size){: .external}

- Description: The size in bytes of each [log file](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_log_file){: .external} in a [log group](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_log_group){: .external}.
- Default: `67108864`
- Minimum: `4194304`
- Maximum: `274877906900`
- Restarts database? - **true**

[`innodb_log_buffer_size`](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_log_buffer_size){: .external}

- Description: The size in bytes of the buffer that InnoDB uses to write to the [log files](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_log_file){: .external} on disk.
- Default: `33554432`
- Minimum: `1048576`
- Maximum: `4294967295`
- Restarts database? - **true**

[`innodb_flush_log_at_trx_commit`](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_flush_log_at_trx_commit){: .external}

- Description: Controls the balance between strict [ACID](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_acid){: .external} compliance for [commit](https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_commit) operations and higher performance that is possible when commit-related I/O operations are rearranged and done in batches. You can achieve better performance by changing the default value but then you can lose transactions in a crash.
- Default: `2`
- Minimum: `0`
- Maximum: `2`
- Restarts database? - **false**
