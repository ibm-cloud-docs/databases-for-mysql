---

copyright:
  years: 2021, 2022
lastupdated: "2022-08-04"

keywords: troubleshooting MySQL, delay mysql, mysql configurable variables

subcollection: databases-for-mysql

content-type: troubleshoot

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:support: data-reuse='support'}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}
{:troubleshoot: data-hd-content-type='troubleshoot'}
 

# Why is there a delay when restoring my {{site.data.keyword.databases-for-mysql_full}} deployment?
{: #troubleshoot-delay}
{: troubleshoot}
{: support}

You're experiencing a long delay when restoring your {{site.data.keyword.databases-for-mysql_full}} deployment.
{: tsSymptoms}

Restoring a backup can be delayed if your configurable variables aren't optimized, or if you aren't using the appropriate tools.
{: tsCauses}

- Check your configurable variables at [Migrating to Databases for MySQL](https://cloud.ibm.com/docs/databases-for-mysql?topic=databases-for-mysql-migrating){: .external}.
- You can restore a backup using [mysqldump](/docs/databases-for-mysql?topic=databases-for-mysql-migrating#migrating-mysqldump) or [mydumper](/docs/databases-for-mysql?topic=databases-for-mysql-migrating#migrating-mydumper)? `mysqldump`, the native MySQL backup client, is effective for databases smaller than 10 GB, whereas `mydumper` is effective with larger databases. Use the tool that best suits your needs.
{: tsResolve}
