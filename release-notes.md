---

copyright:
  years: 2018, 2026
lastupdated: "2026-01-07"

keywords: databases-for-mysql release notes

subcollection: databases-for-mysql

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes
{: #mysql-relnotes}

Use these release notes to learn about the latest updates to {{site.data.keyword.databases-for-mysql_full}} that are grouped by _date_ or _build number_.
{: shortdesc}


## 6 January 2026
{: #databases-for-mysql-6jan2026}
{: release-note}

Introducing Databases for MySQL v8.4 (Preview)
:   Database for MySQL v8.4 is now available in [Preview](docs/cloud-databases?topic=cloud-databases-versioning-policy#version-tags){: external}. This release includes the transition from semi-synchronous to asynchronous replication, along with notable feature updates and deprecations. For more information, see [Upgrading to a new major version](/docs/databases-for-mysql?topic=databases-for-mysql-mysql-upgrading&interface=ui) and [MySQL 8.4 GA](/docs/databases-for-mysql?topic=databases-for-mysql-mysql8.4-ga&interface=ui).


## 10 April 2025
{: #databases-for-mysql-10apr2025}
{: release-note}

{{site.data.keyword.monitoringfull_notm}} now includes the following metrics for {{site.data.keyword.databases-for-mysql}}:

   - Replica state and Replica lag metrics to monitor leader-replica synchronization.
   - Connection usage, Total active connections, and Maximum permitted client connection metrics enable monitoring of deployment connection usage.

For more information, see [MySQL metrics](/docs/databases-for-mysql?topic=databases-for-mysql-monitoring&interface=ui#metrics-by-plan-mysql).

## 10 March 2025
{: #databases-for-mysql-10mar2025}
{: release-note}

{{site.data.keyword.databases-for-mysql}} {{site.data.keyword.satelliteshort}} plan is deprecated
:   {{site.data.keyword.satellitelong}} is now deprecated due to changes in market expectations, client fit, and lack of adoption. As of March 10 2025, all documentation relating to Satellite has been removed, as well as the ability to select {{site.data.keyword.databases-for-mysql}} {{site.data.keyword.satelliteshort}} plan in the Cloud console.

## 15 November 2024
{: #databases-for-mysql-15nov2024}
{: release-note}

{{site.data.keyword.databases-for}} logs and events are now available on {{site.data.keyword.logs_full}}
: {{site.data.keyword.databases-for}} has onboarded {{site.data.keyword.logs_full_notm}}, a scalable logging service that persists logs and provides users with capabilities for querying, tailing, and visualizing logs. Customers are expected to use {{site.data.keyword.logs_full_notm}} to review their database logs and events starting **November 15, 2024**. For more information, see [Set up logging and monitoring](/docs/databases-for-mysql?topic=databases-for-mysql-getting-started-cdb-logging-monitoring&interface=ui) and [About IBM Cloud Logs](/docs/cloud-logs?topic=cloud-logs-about-cl).

## 16 September 2024
{: #databases-for-mysql-16sept2024}
{: release-note}

Private endpoints as new default
:  To ensure best possible security for your databases, private endpoints are now the default in the {{site.data.keyword.cloud}} console. CLI and Terraform now require the endpoint type to be provided as part of creating an instance.

## 1 May 2024
{: #databases-for-mysql-01may2024}
{: release-note}

New hosting models
:  You can choose between two hosting models: Isolated Compute and Shared Compute. Isolated Compute is a secure single-tenant offering for complex, highly performant enterprise workloads. Shared Compute is a flexible multi-tenant offering for dynamic, fine-tuned, and decoupled capacity selections. For more information, see [Hosting models](/docs/databases-for-mysql?topic=databases-for-mysql-hosting-models&interface=ui){: external}.

## 05 December 2023
{: #databases-for-mysql-05dec2023}
{: release-note}

`max_prepared_stmt_count` added to documentation
:  `max_prepared_stmt_count` specifies the total number of prepared statements on the server. For more information, see [Changing your Configuration](/docs/databases-for-mysql?topic=databases-for-mysql-changing-configuration){: external}.

## 22 November 2023
{: #databases-for-mysql-22nov2023}
{: release-note}

Monitoring Integration documentation updated
:  Monitoring Integration documentation now lists metrics for all {{site.data.keyword.databases-for}} services. For more information, see [Monitoring Integration](/docs/cloud-databases?topic=cloud-databases-monitoring){: external}.

## 12 October 2023
{: #databases-for-mysql-12oct2023}
{: release-note}

{{site.data.keyword.databases-for-mysql}} end of life dates
:  End of life announcement: Version 5.7 reaches end of life 26 April 2024. We recommend that you upgrade following our [backup and restore process](/docs/cloud-databases?topic=cloud-databases-dashboard-backups){: external} before the EOL date of your version.

For more information, see [Upgrading to a new Major Version](/docs/databases-for-mysql?topic=databases-for-mysql-mysql-upgrading){: external}.

## 03 July 2023
{: #databases-for-mysql-03july2023}
{: release-note}

Upgrading to a new Major Version
:  Once a major version of a database is at its End Of Life (EOL), upgrade to a current major version. For more information, see [Upgrading to a new Major Version](/docs/databases-for-mysql?topic=databases-for-mysql-mysql-upgrading).

## 22 May 2023
{: #databases-for-mysql-22may2023}
{: release-note}

Setting up disk alerts for disk utilization tutorial
:  In this tutorial, you use the {{site.data.keyword.cloud_notm}} API and the [{{site.data.keyword.cloud_notm}} CLI](https://cloud.ibm.com/docs/cli?topic=cli-getting-started){: external} to set up an alert that emails you whenever the disk utilization of your database exceeds 90%. This specific example creates an alert on a {{site.data.keyword.databases-for-elasticsearch}} deployment, but it is applicable to all the databases in the IBM {{site.data.keyword.databases-for}} catalog. For more information, see [Setting up disk alerts for disk utilization](/docs/databases-for-mysql?topic=databases-for-mysql-disk-util-alert-tutorial).

## 09 March 2023
{: #databases-for-mysql-09mar2023}
{: release-note}

Configuring Your {{site.data.keyword.databases-for-mysql_full}} time zone settings
:  At provisioning, a {{site.data.keyword.databases-for}} deployment is configured to Coordinated Universal Time. Reconfiguring your time zone is a persistent change, which must be undertaken for each of your {{site.data.keyword.databases-for}} deployments. Configure your time zone with the {{site.data.keyword.databases-for}} [API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#introduction) or the [CLI](/docs/databases-cli-plugin). For more information, see [{{site.data.keyword.databases-for-mysql_full}} time zone settings](/docs/databases-for-mysql?topic=databases-for-mysql-changing-configuration&interface=cli#mem-settings).

## 19 October 2022
{: #databases-for-mysql-19oct2022}
{: release-note}

Deploying and Connecting a Cloud Databases Instance Tutorial
:  This tutorial guides you through the process of deploying a {{site.data.keyword.databases-for}} instance and connecting it to a web front end by creating a webpage that allows visitors to input a word and its definition. These values are then stored in a database running on {{site.data.keyword.databases-for}}. You install the database infrastructure by using Terraform and your web application uses the popular Express framework. The application can then be run locally, or by using Docker. For more information, see [Deploying and connecting a {{site.data.keyword.databases-for}} instance](/docs/databases-for-mysql?topic=databases-for-mysql-create-instance-tutorial).

## 11 October 2022
{: #databases-for-mysql-11oct2022}
{: release-note}

Protecting {{site.data.keyword.databases-for-mysql_full}} resources with context-based restrictions
:  Context-based restrictions (CBR) give account owners and administrators the ability to define and enforce access restrictions for {{site.data.keyword.cloud}} resources based on the context of access requests. Access to {{site.data.keyword.databases-for}} resources can be controlled with CBR and identity and access management (IAM) policies. For more information, see [Protecting Cloud Databases resources with context-based restrictions](/docs/databases-for-mysql?topic=databases-for-mysql-cbr&interface=ui).

## 28 June 2022
{: #databases-for-mysql-28june2022}
{: release-note}

{{site.data.keyword.databases-for-mysql_full}} version 8 is now available on {{site.data.keyword.databases-for}}
:  MySQL 8.0 is the latest version of the world's most popular open-source database. This release features improvements across the board, offering enhancements for better performance, reliability, security, and manageability, with significant improvements to the data dictionary. For information on upgrading your current {{site.data.keyword.databases-for-mysql_full}} deployment, or deploying a new instance, see [MySQL 8 GA](https://cloud.ibm.com/docs/databases-for-mysql?topic=databases-for-mysql-mysql8-ga). See blog post announcement [here](https://www.ibm.com/cloud/blog/announcements/ibm-cloud-databases-for-mysql-now-supports-version-8).

## 05 January 2022
{: #databases-for-mysql-05jan2022}
{: release-note}

General Availability of {{site.data.keyword.databases-for-mysql_full}}
:  {{site.data.keyword.databases-for-mysql_full}} added to the [IBM Cloud Databases](https://www.ibm.com/cloud/databases) family. See blog post announcement [here](https://www.ibm.com/cloud/blog/announcements/general-availability-of-ibm-cloud-databases-for-mysql).
