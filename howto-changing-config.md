---
copyright:
  years: 2021, 2023
lastupdated: "2023-12-04"

keywords: mysql, databases, config, mysql configuration, mysql time zone, configuration schema

subcollection: databases-for-mysql

---

{{site.data.keyword.attribute-definition-list}}

# Changing your Deployment Configuration
{: #changing-configuration}

{{site.data.keyword.databases-for-mysql_full}} allows you to change some of the MySQL configuration settings so you can tune your MySQL databases to your use case. To make permanent changes to the database configuration, use the {{site.data.keyword.databases-for}} [CLI-plugin](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-configuration) or [API](https://{DomainName}/apidocs/cloud-databases-api#change-your-database-configuration) to write the changes to the configuration file for your deployment.

The configuration is defined in a schema. To make a change, send a JSON object with the settings and their new values to the API or the CLI. For example, in the CLI or API, set the `max_connections` to 150 using a command like:

```sh
{"configuration":{"max_connections":150}}
```
{: .codeblock}

For more information, see [Managing MySQL Connections](/docs/databases-for-mysql?topic=databases-for-mysql-managing-mysql-connections){: .external}.

## How to calculate the MySQL `max_connections` Variable
{: #changing-configuration-calculate-max-connections}

`max_connections` is a configuration parameter in MySQL that determines the maximum number of concurrent connections that can be established with the database server.

### MySQL `max_connections` basic formula
{: #changing-configuration-calculate-max-connections-formula}

The basic formula for calculating `max_connections` is:

```text
Available RAM = Global Buffers + (Thread Buffers x `max_connections`)
```

To retrieve a list of buffers and their values, use a command like:

```sh
SHOW VARIABLES LIKE '%buffer%';
```
{: pre}

## Using the {{site.data.keyword.databases-for}} CLI plug-in
{: #using-cli}

Check the current configuration of your deployment by using a command like:

```sh
ibmcloud cdb deployment-configuration-schema <deployment name or CRN>
```
{: pre}

To change your configuration through the {{site.data.keyword.databases-for}} CLI-plugin, use `deployment-configuration` command:

```sh
ibmcloud cdb deployment-configuration <deployment name or CRN> [@JSON_FILE | JSON_STRING]
```
{: pre}

The command reads the changes that you would like to make from the JSON object or a file. For more information, see the [CLI reference page](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-configuration).

## Using the {{site.data.keyword.databases-for}} API
{: #using-api}

There are two deployment-configuration endpoints, one for viewing the configuration schema and one for changing the configuration. To view the configuration schema, send a `GET` request to `/deployments/{id}/configuration/schema`.

To change the configuration, send the settings that you would like to change as a JSON object in the request body of a `PATCH` request to `/deployments/{id}/configuration`.

For more information, see [API Reference](https://cloud.ibm.com/apidocs/cloud-databases-api#change-your-database-configuration){: external}.

## {{site.data.keyword.databases-for-mysql}} time zone settings
{: #mem-settings}

The time zone for {{site.data.keyword.databases-for-mysql}} deployments is always Coordinated Universal Time. Configure your time zone with the {{site.data.keyword.databases-for}} API or the CLI change your time zone to a named time zone (recommended) or an offset of a time zone.

You are required to configure the time zone again on both restored instances and read-replicas. Although the time zone tables are restored (in the case of a restore) and replicated (in the case of a read-replica), the `@@global.time_zone` value is not. To set this value, use the same API calls as before, but with the new CRNs.
{: note}

### Configuring your {{site.data.keyword.databases-for-mysql}} time zone settings
{: #mem-settings-config}

At provisioning, a {{site.data.keyword.databases-for}} deployment is configured to Coordinated Universal Time. Reconfiguring your time zone is a persistent change, which must be undertaken for each of your {{site.data.keyword.databases-for}} deployments.

Configuring your time zone sets the global time zone within your MySQL instance. In the instance that a failover occurs, your time zone setting is propagated as part of replication, as the time zone setting is written to the MySQL config file. The exception to this is if you restore an instance to a point in time before you configured your preferred time zone.

If you configure your time zone to one that features Daylight Saving Time, adjustments are part of the configuration. No action is necessary on your part.
Using a specific time zone is better than using an offset time.

{{site.data.keyword.databases-for}} validates the `time_zone` parameter value. If you configure your deployment with an invalid value, the configuration fails. The valid named `time_zone values` can be found [here](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones){: external}.
{: note}

### Changing the time zone in the {{site.data.keyword.databases-for}} API
{: #change-time-zone-api}
{: api}

Example offset:
```sh
curl -v -XPATCH -H "Authorization: Bearer $token" -H "Content-Type: application/json" https://api.<region>.databases.cloud.ibm.com/v5/ibm/deployments/<crn>/configuration -d '{"configuration": {"time_zone": "<EXAMPLE OFFSET"}}'
```
{: pre}

Example named time zone:
```sh
curl -v -XPATCH -H "Authorization: Bearer $token" -H "Content-Type: application/json" https://api.<region>.databases.cloud.ibm.com/v5/ibm/deployments/<crn>/configuration -d '{"configuration": {"time_zone": "<EXAMPLE TIME ZONE"}}'
```
{: pre}

### Changing the time zone in the {{site.data.keyword.databases-for}} CLI
{: #change-time-zone-cli}
{: cli}

Example named time zone
```sh
ibmcloud cdb deployment-configuration <crn> '{"time_zone": "US/Pacific"}'
```
{: pre}

## Available {{site.data.keyword.databases-for-mysql}} Configuration settings
{: #available-config-settings}

[`default_authentication_plugin`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_default_authentication_plugin){: .external}

- Default - `sha256_password`
- Allowable values: `sha256_password`, `mysql_native_password`
- Restarts database? - `true`

Unless strictly necessary, don't use `mysql_native_password`. {: note}

[`innodb_buffer_pool_size_percentage`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_buffer_pool_size){: .external}

- Description: The percentage of memory to use for `innodb_buffer_pool_size`. The default value of 50% is a conservative value and works for databases of any size. If your database requires more RAM, this value can be increased. Setting this value too high can exceed your database's memory limits, which can cause it to crash.
- Default: `50`
- Minimum: `10`
- Maximum: `100`
- Restarts database? - `true`

[`innodb_flush_log_at_trx_commit`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_flush_log_at_trx_commit){: .external}

- Description: Controls the balance between strict [ACID](https://dev.mysql.com/doc/refman/8.0/en/mysql-acid.html){: .external} compliance for [commit](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_commit) operations and higher performance that is possible when commit-related I/O operations are rearranged and done in batches. You can achieve better performance by changing the default value but then you can lose transactions in a crash.
- Default: `2`
- Minimum: `0`
- Maximum: `2`
- Restarts database? - `false`

[`innodb_log_buffer_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_log_buffer_size){: .external}

- Description: The size in bytes of the buffer that InnoDB uses to write to the [log files](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_log_file){: .external} on disk.
- Default: `33554432`
- Minimum: `1048576`
- Maximum: `4294967295`
- Restarts database? - `true`

[`innodb_log_file_size`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_log_file_size){: .external}

- Description: The size in bytes of each [log file](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_log_file){: .external} in a [log group](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_log_group){: .external}.
- Default: `67108864`
- Minimum: `4194304`
- Maximum: `274877906900`
- Restarts database? - `true`

[`innodb_lru_scan_depth`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_lru_scan_depth){: .external}

- Description: A parameter that influences the algorithms and heuristics for the [flush](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_flush){: .external} operation for the InnoDB [buffer pool](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_buffer_pool){: .external}. A setting smaller than the default is generally suitable for most workloads. A value that is much higher than necessary might impact performance. Consider increasing the value only if you have spare I/O capacity under a typical workload.
- Default: `256`
- Minimum: `128`
- Maximum: `2048`
- Restarts database? - `false`

[`innodb_write_io_threads`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_write_io_threads){: .external}

- Description: The number of I/O threads for write operations in InnoDB.
- Default: `4`
- Minimum: `1`
- Maximum: `64`
- Restarts database? - `true`

[`max_allowed_packet`](https://dev.mysql.com/doc/refman/8.0/en/packet-too-large.html){: .external}

- Default - `16777216`
- Minimum - `1024`
- Maximum - `1073741824`
- Restarts database? - `false`

[`max_prepared_stmt_count`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_max_prepared_stmt_count)

- Default - `16382`
- Minimum - `0`
- Maximum - (version ≤ 8.0.17) `1048576`, (version ≥ 8.0.18) `4194304`
- Restarts database? - `false`

[`max_connections`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_max_connections){: .external}

- Default - `200`
- Restarts database? - `false`

 You might need to [scale before you increase max connections](/docs/databases-for-mysql?topic=databases-for-mysql-high-availability#connection-limits-ha).{: note}

[`mysql_max_binlog_age_sec`](https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html){: .external}

- Default - `1800`
- Restarts database? - `false`

[`net_write_timeout`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_net_write_timeout){: .external}

- Description: The number of seconds to wait for a block to be written to a connection before aborting the write.
- Default: `60`
- Minimum: `1`
- Maximum: `7200`
- Restarts database? - `false`

[`sql_mode`](https://dev.mysql.com/doc/refman/8.0/en/sql-mode.html){: .external}

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
- Restarts database? - `false`

[`time_zone`](https://dev.mysql.com/doc/refman/8.0/en/time-zone-support.html#time-zone-variables){: .external}

- Description: The time zone that is currently set on the server is '+00:00' (Coordinated Universal Time) by default. However, it can also be set to a specific offset from Coordinated Universal Time in the format of [H]H:MM, with a + or - prefix, for example '+10:00', '-6:00', or '+05:30'. Named time zones like 'MET' or 'US/Pacific' can also be used.
- Default: `+00:00`
- Type: `string`
- Restarts database? - `false`

[`wait_timeout`](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_wait_timeout){: .external}

- Description: The number of seconds the server waits for activity on a noninteractive connection before closing it.
- Default: `28800`
- Minimum: `1`
- Maximum: `31536000`
- Restarts database? - `false`
