---

copyright:
  years: 2021, 2025
lastupdated: "2025-04-01"

keywords: troubleshooting MySQL, connect mysql

subcollection: databases-for-mysql

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}} 

# Why can’t I connect to my MySQL deployment?
{: #troubleshoot-connect}
{: troubleshoot}
{: support}

If you encounter errors when connecting to your {{site.data.keyword.databases-for-mysql_full}} deployment, review these common causes and resolutions.
{: shortdesc}

You receive an error message or fail to connect to a {{site.data.keyword.databases-for-mysql}} deployment. If reviewing application logs, you might see errors that mention intermittent connection timeouts or an inability to connect.
{: tsSymptoms}

Review the following information to troubleshoot and resolve common connectivity problems:
{: tsResolve}

* An unsecured connection is a common cause of connectivity errors.  All {{site.data.keyword.databases-for-mysql}} connections use TLS/SSL encryption; {{site.data.keyword.databases-for-mysql}} rejects unsecured connections. To avoid errors, make sure you configured a secure connection.  Refer to [Getting started](/docs/databases-for-mysql?topic=databases-for-mysql-getting-started) for an example of a secure connection.
* If you are using a private endpoint, make sure that you specify connection strings that contain the private endpoint (see [Credentials for private endpoints](/docs/databases-for-mysql?topic=databases-for-mysql-service-endpoints&interface=ui#credentials-for-private-endpoints)) and that you followed the steps in [Connecting through private endpoints](/docs/databases-for-mysql?topic=databases-for-mysql-service-endpoints&interface=ui#private-endpoint-connections).
* If your application log captures a short connection interruption, that behavior is expected as a normal part of operations for this managed service. You want to design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}. However, if you experience several minutes of connection interruption check the Cloud Status for the service. For more information, see [Application-level high-availability](/docs/databases-for-mysql?topic=databases-for-mysql-high-availability#application-level-high-availability).
