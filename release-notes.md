---

copyright:
  years: 2018, 2023
lastupdated: "2023-10-12"

keywords: databases-for-mysql release notes

subcollection: databases-for-mysql

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes for {{site.data.keyword.databases-for-mysql_full}}
{: #mysql-relnotes}

Use these release notes to learn about the latest updates to {{site.data.keyword.databases-for-mysql_full}} that are grouped by _date_ or _build number_.
{: shortdesc}

## 12 October 2023
{: #databases-for-mysql-12oct2023}
{: release-note}

{{site.data.keyword.databases-for-mysql}} end of life dates
:  End of life announcement: Version 5.7 reaches end of life 26 April 2024. All {{site.data.keyword.databases-for-mysql}} instances on deprecated versions that are still active will be upgraded in-place to the next major version. We recommend that you upgrade following our [backup and restore process](/docs/cloud-databases?topic=cloud-databases-dashboard-backups){: external} before the EOL date of your version.

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
:  This tutorial guides you through the process of deploying a {{site.data.keyword.databases-for}} instance and connecting it to a web front end by creating a webpage that allows visitors to input a word and its definition. These values are then stored in a database running on {{site.data.keyword.databases-for}}. You install the database infrastructure by using Terraform and your web application uses the popular Express framework. The application can then be run locally, or by using Docker. For more information, see [Deploying and Connecting a {{site.data.keyword.databases-for}} Instance](/docs/databases-for-mysql?topic=cloud-databases-create-instance-tutorial).

## 11 October 2022
{: #databases-for-mysql-11oct2022}
{: release-note}

Protecting {{site.data.keyword.databases-for-mysql_full}} resources with context-based restrictions
:  Context-based restrictions (CBR) give account owners and administrators the ability to define and enforce access restrictions for {{site.data.keyword.cloud}} resources based on the context of access requests. Access to {{site.data.keyword.databases-for}} resources can be controlled with CBR and identity and access management (IAM) policies. For more information, see [Protecting Cloud Databases resources with context-based restrictions](/docs/databases-for-mysql?topic=cloud-databases-cbr&interface=ui).

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
