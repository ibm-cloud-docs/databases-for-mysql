---
copyright:
  years: 2021
lastupdated: "2021-12-01"

keywords: mysql, databases

subcollection: databases-for-mysql

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Connecting with `mysql`
{: #connecting-mysql}

You can access your MySQL database directly from its command line client, `mysql`. You can use `mysql` for direct interaction and monitoring of the data structures created within the database. It is also useful for testing and monitoring the queries and performance, installing and modifying scripts, and other management activities.

You have to set the admin password before you use it to connect to the database. For more information, see the [Setting the Admin Password](/docs/databases-for-mysql?topic=databases-for-mysql-admin-password) page.
{: .tip}

## Installing `mysql`
{: #installing-mysql}

Install the command line client for MySQL, `mysql`. To use `mysql`, the MySQL client tools need to be installed on the local system. They can be installed with the full MySQL package that is provided from [mysql.com](https://www.mysql.com/downloads/), or as a [package from your operating system's package manager](https://dev.mysql.com/doc/mysql-installation-excerpt/5.7/en/). 

For more information about `mysql`, see the [MySQL documentation](https://dev.mysql.com/doc/refman/5.7/en/).

## `mysql` Connection Strings
{: #mysql-conn-strings}

Connection strings are displayed in the _Endpoints_ panel of your deployment's _Overview_, and can also be retrieved from the [cloud databases CLI plugin](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-connections), and the [API](https://{DomainName}/apidocs/cloud-databases-api#discover-connection-information-for-a-deployment-f-e81026).

The information that you need to make a connection with `mysql` is in the "cli" section of your connection strings. The table contains a breakdown for reference.

Field Name|Index|Description
----------|-----|-----------
`Bin`||The recommended binary to create a connection; in this case it is `mysql`.
`Composed`||A formatted command to establish a connection to your deployment. The command combines the `Bin` executable, `Environment` variable settings, and uses `Arguments` as command line parameters.
`Environment`||A list of key/values you set as environment variables.
`Arguments`|0...|The information that is passed as arguments to the command shown in the Bin field.
`Certificate`|Base64|A self-signed certificate that is used to confirm that an application is connecting to the appropriate server. It is base64 encoded.
`Certificate`|Name|The allocated name for the self-signed certificate.
`Type`||The type of package that uses this connection information; in this case `cli`. 
{: caption="Table 1. mysql/cli connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

## Connecting
{: #mysql-connecting}

The `ibmcloud cdb deployment-connections` command handles everything that is involved in creating a command line client connection. For example, to connect to a deployment named  "example-mysql", use the following command.

```shell
ibmcloud cdb deployment-connections example-mysql --start
```
{: pre}

Or
```shell
ibmcloud cdb cxn example-mysql -s
```
{: pre}

The command prompts for the admin password and then runs the `mysql` command line client to connect to the database.

If you have not installed the cloud databases plug-in, connect to your MySQL databases using `mysql` by giving it the "composed" connection string. It provides environment variables `MYSQL_PWD` and `--ssl-ca=<cert_name>`. Set `MYSQL_PWD` to the admin's password and `--ssl-ca=<cert_name>` to the path or file name for the self-signed certificate. 

```shell
MYSQL_PWD=$MYSQL_PWD --ssl-ca=<cert_name>=0b22f14b-7ba2-11e8-b8e9-568642342d40 mysql 'host=4a8148fa-3806-4f9c-b3fc-6467f11b13bd.8f7bfd7f3faa4218aec56e069eb46187.databases.appdomain.cloud port=32325 dbname=ibmclouddb user=admin sslmode=verify-full'
```

## Using the self-signed certificate
{: #mysql-using-ssc}

1. Copy the certificate information from the _Endpoints_ panel or the Base64 field of the connection information. 
2. If needed, decode the Base64 string into text. 
3. Save the certificate  to a file. (You can use the Name that is provided or your own file name).
4. Provide the path to the certificate to the `--ssl-ca=<cert_name>` environment variable.

You can display the decoded certificate for your deployment with the CLI plug-in with the command:
```shell
ibmcloud cdb deployment-cacert "your-service-name"
```
{: pre}

It decodes the base64 into text. Copy and save the command's output to a file and provide the file's path to the `--ssl-ca=<cert_name>` environment variable.
