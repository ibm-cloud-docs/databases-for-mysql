---

copyright:
  years: 2021 
lastupdated: "2021-12-02"

keywords: mysql, databases, monitoring, scaling, autoscaling, resources, connection limits

subcollection: databases-for-mysql

---

{:external: .external target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Performance
{: #performance}

{{site.data.keyword.databases-for-mysql_full}} deployments can be manually [scaled to your usage](/docs/databases-for-mysql?topic=databases-for-mysql-resources-scaling#resources-scaling-consider) or configured to [autoscale](/docs/databases-for-mysql?topic=databases-for-mysql-autoscaling-mysql) under certain resource conditions. There are a few factors to consider if you are tuning the performance of your deployment.

## Monitoring your deployment
{: #monitor-deployment}

{{site.data.keyword.databases-for-mysql}} deployments offer an integration with the [{{site.data.keyword.monitoringfull}} service](/docs/databases-for-mysql?topic=databases-for-mysql-monitoring) for basic monitoring of resource usage on your deployment. Many of the available metrics, like disk usage and IOPS, are presented to help you configure [autoscaling](/docs/databases-for-mysql?topic=databases-for-mysql-autoscaling) on your deployment. Observing trends in your usage and configuring the autoscaling to respond to them can help alleviate performance problems before your databases become unstable due to resource exhaustion.

## Disk IOPS
{: #disk-iops}

The number of Input-Output Operations Per Second (IOPS) is limited by the type of storage volume. Storage volumes for {{site.data.keyword.databases-for-mysql}} deployments are provisioned on [Block Storage Endurance Volumes in the 10 IOPS per GB tier](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage&interface=ui). If your operational load saturates or exceeds the IOPS limit, database requests and operations are delayed until the disk can catch up. Extended periods of heavy-load can cause your deployment to be unable to process queries and become effectively unavailable. If you experience delayed responses and failing operations, you could be exceeding the disk's IOPS limit. You can increase the number IOPS available to your deployment by increasing disk space.

## Connection Limits 
{: #connection-limits-performance}

{{site.data.keyword.databases-for-mysql}} sets the maximum number of connections to your MySQL database to **200**. It is recommended to leave some connections available, as a number of them are reserved internally to maintain the state and integrity of your database. After the connection limit has been reached, any attempts at starting a new connection results in an error. To prevent overwhelming your deployment with connections, use connection pooling, or scale your deployment and increase its connection limit. For more information, see the [Managing MySQL Connections](/docs/databases-for-mysql?topic=databases-for-mysql-managing-mysql-connections) page.
