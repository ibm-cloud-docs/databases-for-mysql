---

copyright:
  years: 2023
lastupdated: "2023-05-10"

keywords: mysql backup, backups, xtrabackup, corrupted_table

subcollection: databases-for-mysql

---

{{site.data.keyword.attribute-definition-list}}

# Backing up {{site.data.keyword.databases-for-mysql}} FAQ
{: #faq-mysql-backups}
{: faq}
{: support}

Backups for {{site.data.keyword.databases-for-mysql}} deployments are accessible from the Backups tab of your deployment's dashboard. 

## Optimizing `xtrabackup` table
{: #optimize-xtrabackup-table}
{: faq}

MySQL version 8.0.29 offered features that were not safe to use and the version was removed completely. {{site.data.keyword.databases-for}} chose not to offer that version, but you can still use and import tables from that corrupted version. This leads to an error like this: 

```text
[ERROR] [MY-011825] [Xtrabackup] Tables found:
2023-03-03T08:09:34.643290-00:00 0 [ERROR] [MY-011825] [Xtrabackup] corrupted_table
2023-03-03T08:09:34.643300-00:00 0 [ERROR] [MY-011825] [Xtrabackup] 
Please run OPTIMIZE TABLE or ALTER TABLE ALGORITHM=COPY on all listed tables to fix this issue.
```

To resolve this error, query and optimize the `Xtrabackup` table using a command like:

```sh
SELECT NAME FROM INFORMATION_SCHEMA.INNODB_TABLES WHERE TOTAL_ROW_VERSIONS > 0;
```
{: pre}

If the results are a list of tables, run `OPTIMIZE TABLE` on the list before taking a backup. 
