---

Copyright:
  years: 2021
lastupdated: "2021-04-12"

keywords: mysql, databases, connection limits, terminating connections, connection pooling

subcollection: databases-for-mysql

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Managing MySQL Connections
{: #managing-connections}

Connections to your {{site.data.keyword.databases-for-mysql_full}} deployment use resources, so it is important to consider how many connections you need when tuning your deployment's performance. MySQL uses a `max_connections` setting to limit the number of connections (and resources that are consumed by connections) to prevent runaway connection behavior from overwhelming your deployment's resources.

You can check the value of `max_connections` with your [admin user](/docs/databases-for-mysql?topic=databases-for-mysql-user-management#the-admin-user) and [`mysql`](/docs/databases-for-mysql?topic=databases-for-mysql-connecting-mysql).
```
ibmclouddb=> SHOW max_connections;
 max_connections
-----------------
 200
(1 row)
```
{: .codeblock}

## MySQL Connection Limits 

At provision, {{site.data.keyword.databases-for-mysql}} sets the maximum number of connections to your MySQL database to **200**. It is recommended to leave some connections available, as a number of them are reserved internally to maintain the state and integrity of your database. After the connection limit has been reached, any attempts at starting a new connection results in an error. To prevent overwhelming your deployment with connections, use connection pooling, or scale your deployment and increase its connection limit. For more information, see the [Managing MySQL Connections page](https://test.cloud.ibm.com/docs/databases-for-mysql?topic=databases-for-mysql-managing-connections).

```
FATAL: remaining connection slots are reserved for
non-replication superuser connections
```
Exceeding the connection limit for your deployment can cause your database to be unreachable by your applications.

### End Connections

If your deployment reaches the connection limit or you are having trouble connecting to your deployment and suspect that a high number of connections is a problem, you can disconnect (or end) all of the connections to your deployment. 

In the UI, on the _Settings_ tab, there is a button to `End Connections` to your deployment. Use caution, as it disrupts anything that is connected to your deployment.

The CLI command to end connections to the deployment is 
```
ibmcloud cdb deployment-kill-connections <deployment name or CRN>
```

You can also use the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api#kill-connections-to-a-MySql-deployment) to perform the end all connections operation.


