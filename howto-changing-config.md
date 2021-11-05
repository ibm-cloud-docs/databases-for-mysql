---
copyright:
  years: 2021
lastupdated: "2021-11-02"

keywords: mysql, databases, config

subcollection: databases-for-mysql

---

{:new_window: target="_blank"}
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

```shell
{"configuration":{"max_connections":150}}
```
{: .codeblock}

to either the CLI or to the API. 

For more information on checking the current value of `max_connections`, see the [Managing MySQL Connections](/docs/allowlist/databases-for-mysql?topic=databases-for-mysql-managing-mysql-connections) documentation. 

## Using the CLI
{: #using-cli}

You can check the current configuration of your deployment with 
```shell
ibmcloud cdb deployment-configuration-schema <deployment name or CRN>
```
{: pre}

To change your configuration through the {{site.data.keyword.databases-for}} cli-plugin, use `deployment-configuration` command. 
```shell
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

[`max_connections`](https://www.postgresql.org/docs/current/runtime-config-connection.html#GUC-MAX-CONNECTIONS)

- Default - `200`
- Restarts database? - **false**
  
 You might need to [scale before you increase max connections](/docs/allowlist/databases-for-mysql?topic=databases-for-mysql-high-availability#connection-limits-ha). {: note}

[`mysql_max_binlog_age_sec`](https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html)

- Default - `1800`
- Restarts database? - **false**

[`mysql_default_authentication_plugin`](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html)

- Default - `mysql_native_password`
- Allowable values: `sha256_password`, `mysql_native_password`
- Restarts database? - **false**

[`sql_mode`](https://dev.mysql.com/doc/refman/5.7/en/sql-mode.html)

- Default - `___`
- Allowable values: `___`, `___`
- Restarts database? - **___**
