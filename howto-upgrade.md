---
copyright:
  years: 2020, 2023
lastupdated: "2023-05-30"

keyowrds: mysql, databases, upgrading, major versions, mysql new deployment, mysql database version, mysql major version

subcollection: databases-for-mysql

---

{{site.data.keyword.attribute-definition-list}}

# Upgrading to a new Major Version
{: #mysql-upgrading}

Once a major version of a database is at its End Of Life (EOL), it is a good idea to upgrade to a current major version. 

Find the available versions of MySQL on the [{{site.data.keyword.databases-for-mysql_full}} catalog](https://cloud.ibm.com/databases/databases-for-mysql/create) page, from the {{site.data.keyword.databases-for}} CLI `plug-in` command [`ibmcloud cdb deployables-show`](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployables-show), or from the {{site.data.keyword.databases-for}} API [`/deployables`](https://cloud.ibm.com/apidocs/cloud-databases-api#get-all-deployable-databases) endpoint.

## Requirements for upgrading to MySQL version 8
{: #mysql-upgrading-reqs}

## Check for an upgrade with Upgrade Checker Utility
{: #mysql-check-upgrade-checkForServerUpgrade}

The [Upgrade Checker Utility in MySQL Shell](https://dev.mysql.com/doc/mysql-shell/8.0/en/mysql-shell-utilities-upgrade.html){: external} allows you to check if an upgrade is required for MySQL. This function is useful when you want to determine whether your current version is compatible with an upgrade package.

You can start the upgrade checker utility from the command line using the [mysqlsh](https://dev.mysql.com/doc/mysql-shell/8.0/en/mysqlsh.html) command interface. The following example checks a MySQL server for upgrade to release 8.0.27, and returns JSON output:
```sh
mysqlsh -- util checkForServerUpgrade user@localhost:3306 
                   --target-version=8.0.27 --output-format=JSON --config-path=/etc/mysql/my.cnf
```
{: pre}

## Upgrade from a Read-only Replica
{: #mysql-upgrading-replica}

To upgrade by setting up a read-only replica, provision a read-only replica with the same database version as your deployment. Then wait while it replicates all of your data.

The [Configuring Read-only Replicas](/docs/databases-for-mysql?topic=databases-for-mysql-read-replicas) page covers provisioning, syncing, and other details for read-only replicas.
```sh
curl -X POST \
  https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/remotes/promotion \
  -H 'Authorization: Bearer <>'  \
 -H 'Content-Type: application/json' \
 -d '{
    "promotion": {
        "version": "8",
        "skip_initial_backup": false
    }
}' \
```
{: pre}

`skip_initial_backup` is optional. If set to `true`, the new deployment does not take an initial backup when the promotion completes. Your new deployment is available in a shorter amount of time, at the expense of not being backed up until the next automatic backup is run, or you take an on-demand backup.

## Back up and Restore Upgrade
{: #mysql-backup-restore}

One way to upgrade your database version is to [restore a backup](/docs/databases-for-postgresql?topic=cloud-databases-dashboard-backups#restoring-a-backup) of your data into a new deployment that is running the new database version.

### Upgrading in the UI
{: #mysql-upgrading-ui}
{: ui}

You can upgrade to a new version when [restoring a backup](/docs/databases-for-mysql?topic=databases-for-mysql-dashboard-backups) from the _Backups_ menu of your _Deployment dashboard_. Clicking **Restore** on a backup brings up a dialog box where you can change some options for the new deployment. One of them is the database version, which is auto-populated with the versions available for you to upgrade to. Select a version and click **Restore** to start the provision and restore process.

### Upgrading through the CLI
{: #mysql-upgrading-cli}
{: cli}

When you upgrade and restore from backup through the {{site.data.keyword.cloud_notm}} CLI, use the provisioning command from the resource controller.
```sh
ibmcloud resource service-instance-create <service-name> <service-id> <service-plan-id> <region>
```
{: pre}

The parameters `service-name`, `service-id`, `service-plan-id`, and `region` are all required. You also supply the `-p` with the version and backup ID parameters in a JSON object. The new deployment is automatically sized with the same disk and memory as the source deployment at the time of the backup.

```sh
ibmcloud resource service-instance-create example-upgrade databases-for-mysql standard us-south \
-p \ '{
  "backup_id": "crn:v1:bluemix:public:databases-for-mysql:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
  "version":11
}'
```
{: pre}

### Upgrading through the API
{: #mysql-upgrading-api}
{: api}

Similar to provisioning through the API, you need to complete [the necessary steps to use the resource controller API](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#create-resource-instance) before you can use it to upgrade from a backup. Then, send the API a POST request. The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required. You also supply the version and backup ID. The new deployment has the same memory and disk allocation as the source deployment at the time of the backup.
```sh
curl -X POST \
  https://resource-controller.cloud.ibm.com/v2/resource_instances \
  -H 'Authorization: Bearer <>' \
  -H 'Content-Type: application/json' \
    -d '{
    "name": "my-instance",
    "target": "bluemix-us-south",
    "resource_group": "5g9f447903254bb58972a2f3f5a4c711",
    "resource_plan_id": "databases-for-mysql-standard",
    "backup_id": "crn:v1:bluemix:public:databases-for-mysql:us-south:a/54e8ffe85dcedf470db5b5ee6ac4a8d8:1b8f53db-fc2d-4e24-8470-f82b15c71717:backup:06392e97-df90-46d8-98e8-cb67e9e0a8e6",
    "version":11
  }'
```
{: pre}

### Dry running the promotion and upgrade
{: #mysql-promotion-dry-run}

In order to evaluate the effects of major version upgrades, you can trigger a dry run. A dry run does not perform the promotion and upgrade, but it does simulate it, with the results printed to the database logs. You can access and view your database logs through the [Log Analysis Integration](/docs/databases-for-postgresql?topic=cloud-databases-logging). This ensures the version that you are currently running with its extensions can be successfully upgraded to your intended version.

The dry run must be run with `skip_initial_backup` set to `false`, and `version` defined.
```sh
curl -X POST \
  https://api.{region}.databases.cloud.ibm.com/v4/ibm/deployments/{id}/remotes/promotion \
  -H 'Authorization: Bearer <>'  \
 -H 'Content-Type: application/json' \
 -d '{
    "promotion": {
        "version": "8",
        "skip_initial_backup": false,
        "dry_run": true
    }
}' \
```
{: pre}
