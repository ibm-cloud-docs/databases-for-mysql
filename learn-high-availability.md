---

copyright:
  years: 2021
lastupdated: "2021-12-02"

keywords: mysql, databases, connection limits

subcollection: databases-for-mysql

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# High-Availability
{: #high-availability}

{{site.data.keyword.databases-for-mysql_full}} is a managed cloud database service that is fully integrated into the {{site.data.keyword.cloud_notm}} environment. The database, storage, and supporting infrastructure all run in {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.databases-for-mysql}} provides replication, fail-over, and high-availability features to protect your databases and data from infrastructure maintenance, upgrades, and failures. Deployments contain a cluster with three data members: a leader and two replicas. All members contain a copy of your data by using using Orchestrator to handle fail-overs. If the leader becomes unreachable, the cluster initiates a fail-over and a replica is promoted to leader. The replica rejoins the cluster and your cluster continues to operate normally. 

{{site.data.keyword.databases-for-mysql}} will, at times, do controlled switchovers under normal operation. These switchovers are no-data-loss events that result in resets of active connections. There is a short period where reconnections can fail. At times, unplanned fail-overs might occur due to unforeseen events on the operating environment. These can take a bit longer. In both cases, the potential exists for the downtime to be longer.

## Application-level High-Availability
{: #app-level-high-availability}

Applications that communicate over networks and cloud services are subject to transient connection failures. You want to design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}.

There are multiple possible reasons your database might experience downtime, including: 
- network outages
- Storage and volume-related issues
- High CPU usage
- High disk I/O
- Connection overloads

Because {{site.data.keyword.databases-for-mysql}} is a managed service, regular updates and database maintenance occurs as part of normal operations. In the event that both replicas are lost, writes to the leader will hang, due to the semi-synchronous replication process not having a follower. This can occasionally cause short intervals where your database is unavailable. It can also cause the database to trigger a graceful fail-over, retry, and reconnect. It takes a short time for the database to determine which member is a replica and which is the leader, so you might also see a short connection interruption. fail-overs generally take less than 30 seconds. To minimize interruptions, updates are applied to replicas first, and the leader last.

Your applications have to be designed to handle temporary interruptions to the database, implement error handling for failed database commands, and implement retry logic to recover from a temporary interruption.

Several minutes of database unavailability or connection interruption are not expected. Open a [support case](https://cloud.ibm.com/unifiedsupport/cases/add) with details if you have time periods longer than a minute with no connectivity so we can investigate.

## Connection Limits
{: #connection-limits-ha}

{{site.data.keyword.databases-for-mysql}} sets the maximum number of connections to your MySQL database to **200**. It is recommended to leave some connections available, as a number of them are reserved internally to maintain the state and integrity of your database. After the connection limit has been reached, any attempts at starting a new connection results in an error. To prevent overwhelming your deployment with connections, use connection pooling, or scale your deployment and increase its connection limit. For more information, see the [Managing MySQL Connections](/docs/databases-for-mysql?topic=databases-for-mysql-managing-mysql-connections) page.

## High availability, disaster recovery, and SLA resources
{: #high-availability-disaster-sla}

{{site.data.keyword.databases-for-mysql}} deployments conform to the {{site.data.keyword.cloud_notm}} Databases [HA, DR, and SLA](/docs/cloud-databases?topic=cloud-databases-ha-dr) information and terms.

