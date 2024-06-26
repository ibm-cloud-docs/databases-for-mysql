---

copyright:
  years: 2021, 2024
lastupdated: "2024-05-06"

subcollection: cloud-databases, backups, restore

keywords: backups

---

{{site.data.keyword.attribute-definition-list}}

# Managing backups
{: #dashboard-backups}

Backups for {{site.data.keyword.databases-for}} deployments are accessible from the _Backups_ tab of your deployment's dashboard. See the following general information about backups.

- Automatic backups are performed daily and kept with a simple retention schedule of 30 days.
- Backups cannot be deleted. 
- If you delete your deployment, its backups are deleted automatically.
- Scheduling of the daily backup is not configurable.
- Backups are cross-regionally durable. Backups are stored across multiple regions, and are restorable to other regions, except for `eu-de`, which cannot cross regional boundaries.
- Backup storage is encrypted. If you need to manage the encryption keys, you can do so with the [Key Protect integration](/docs/cloud-databases?topic=cloud-databases-key-protect#byok-for-backups). Otherwise, they are encrypted with a key that is automatically generated for your deployment.
- Backups are also restorable across accounts, but only by using the API and only if the user that is running the restore has access to both the source and destination accounts. 

## Backups in the UI
{: #dashboard-backups-ui}

The backup types have their respective tabs, either _On-demand_ or _Automatic_. Each backup is listed with its type and when the backup was taken. Click the timestamp to change its format between elapsed time, local time, and Coordinated Universal Time (UTC). 

![List of backups on the Backups tab](images/backups-list.png){: caption="Figure 1. List of backups on the Backups tab" caption-side="bottom"}

Click the backup to reveal information for that specific backup, including its full ID. For restore options, there is a **Restore** button or a pre-formatted CLI command. 

## Backups in the CLI
{: #dashboard-backups-cli}

You can also access the list of backups and individual backup information from the {{site.data.keyword.databases-for}} CLI plug-in and the {{site.data.keyword.databases-for}} API.

Use the [`cdb deployment-backups-list`](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-backups-list) command to view the list of all available backups for your deployment. To get the details about a specific backup, use [`cdb backup-show`](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#backup-show) command.

For example, to view the backups for a deployment named "example-deployment", use the following command.

```sh
ibmcloud cdb deployment-backups-list example-deployment
```
{: .pre}

To see the details of one of the backups from the list, take the ID from the `ID` field of the `deployment-backups-list` response and use it with the `backup-show` command.

```sh
ibmcloud cdb backup-show crn:v1:staging:public:cloud-databases:us-south:a/6284014dd5b487c87a716f48aeeaf99f:3b4537bf-a585-4594-8262-2b1e24e2701e:backup:a3364821-d061-413f-a0df-6ba0e2951566
```
{: .pre}

## Backups in the API
{: #dashboard-backups-api}

For backups information in the {{site.data.keyword.databases-for}} API, use the [`/deployments/{id}/backups`](https://cloud.ibm.com/apidocs/cloud-databases-api#get-currently-available-backups-from-a-deployment) endpoint to list the deployment's backups. Use the [`/backups/{backup_id}`](https://{DomainName}/apidocs/cloud-databases-api#get-information-about-a-backup) endpoint to get information about a specific backup.

## Taking an on-demand backup
{: #dashboard-backups-ondemand}

On-demand backups are useful if you plan to make major changes to your deployment like scaling or removing databases, tables, collections. It can also be useful if you need to back up on a schedule. On-demand backups are kept for 30 days. 

Deployments come with free backup storage equal to their total disk space. If your backup storage usage is greater than total disk space, each gigabyte is charged at an overage $0.03/month. Backups are compressed, so even if you use on-demand backups, most deployments will not exceed the allotted credit.
{: .tip}

To create a manual backup in the UI, go to the _Backups_ tab of your deployment then click **Back up now**. A message is displayed that a backup is in progress, and an on-demand backup is added to the list of available backups.

In the CLI, you trigger an on-demand backup with the [`cdb deployment-backup-now`](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-backup-now) command.
```sh
ibmcloud cdb deployment-backup-now example-deployment
```
{: .pre}

In the API, sending a POST to the [`/deployments/{id}/backups`](https://cloud.ibm.com/apidocs/cloud-databases-api#initiate-an-on-demand-backup) endpoint triggers an on-demand backup.

## Restoring a backup
{: #dashboard-backups-restoring}

Backups are restored to a new deployment. After the new deployment finishes provisioning, your data in the backup file is restored into the new deployment.

For new hosting models, Restore is currently available through the CLI, TF, and API.
{: note}

By default the new deployment is auto-sized to the same disk and memory allocation as the source deployment at the time of the backup you are restoring from. If you need to adjust the resources that are allocated to the new deployment, use the optional fields in the UI, CLI, or API to resize the new deployment. Be sure to allocate enough for your data and workload; if the deployment is not given enough resources, the restore fails.

It is important that you do not delete the source deployment while the backup is restoring. You must wait until the new deployment is provisioned and the backup is restored before deleting the old deployment. Deleting a deployment also deletes its backups so not only will the restore fail, you might not be able to recover the backup.
{: .tip}

### Restoring a backup in the UI
{: #dashboard-backups-restore-ui}

To restore a backup to a new service instance,

1. Click in the corresponding row to expand the options for the backup that you want to restore.
2. Click the **Restore** button.
3. Use the dialog box to select from some available options. 
   - The new deployment is automatically named `<name>-restore-[timestamp]`, but you can rename it. 
   - You can also select the region where the new deployment is located. Cross-region restores are supported, except for restoring into or out of the `eu-de` region.
   - You can choose the initial resource allocation, either to expand or shrink the resources on the new deployment. You can also enable or disable dedicated cores.
4. Click the **Restore** button. A "restore from backup started" message appears. Clicking **Your new instance is available now.** takes you to your _Resources List_.

### Restoring a backup in the CLI
{: #dashboard-backups-restore-cli}

The Resource Controller supports provisioning of database deployments, and provisioning and restoring are the responsibility of the Resource Controller CLI. Use the [`resource service-instance-create`](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_service_instance_create) command.

```sh
ibmcloud resource service-instance-create <SERVICE_INSTANCE_NAME> <service-id> standard <region> -p '{"backup_id":"BACKUP_ID"}'
```
{: .pre}

Change the value of `SERVICE_INSTANCE_NAME` to the name you want for your new deployment. The `service-id` is the type of deployment, such as `databases-for-postgresql` or `messages-for-rabbitmq`. The `region` is where you want the new deployment to be located, which can be a different region from the source deployment. Cross-region restores are supported, except for restoring to or from `eu-de` by using another region. `BACKUP_ID` is the backup that you want to restore.

Optional parameters are available when restoring through the CLI. Use them if you need to customize resources, or use a Key Protect key for BYOK encryption on the new deployment.
```sh
ibmcloud resource service-instance-create <SERVICE_INSTANCE_NAME> <service-id> standard <region> -p
'{"backup_id":"BACKUP_ID","key_protect_key":"KEY_PROTECT_KEY_CRN", "members_disk_allocation_mb":"DESIRED_DISK_IN_MB", "members_memory_allocation_mb":"DESIRED_MEMORY_IN_MB", "members_cpu_allocation_count":"NUMBER_OF_CORES"}'
```
{: .pre}

A pre-formatted command for a specific backup is available in detailed view of the backup on the _Backups_ tab of your deployment dashboard.
{: .tip}

### Restoring a backup through the API
{: #dashboard-backups-restore-api}

The Resource Controller supports provisioning of database deployments, and provisioning and restoring are the responsibility of the Resource Controller API. You need to complete [the necessary steps to use the resource controller API](/docs/databases-for-mysql?topic=databases-for-mysql-provisioning&interface=api#provision-controller-api) before you can use it to restore from a backup. 

When you have all the information, the create request is a `POST` to the [`/resource_instances`](https://{DomainName}/apidocs/resource-controller#create-provision-a-new-resource-instance) endpoint.

```sh
curl -X POST \
  https://resource-controller.cloud.ibm.com/v2/resource_instances \
  -H 'Authorization: Bearer <>' \
  -H 'Content-Type: application/json' \
    -d '{
    "name": "<SERVICE_INSTANCE_NAME>",
    "target": "<region>",
    "resource_group": "<your-resource-group>",
    "resource_plan_id": "<service-id>",
    "parameters":{
      "backup_id": "<backup_id>"
    }
  }'
```
{: .pre}

The parameters `name`, `target`, `resource_group`, and `resource_plan_id` are all required, and `backup_id` is the backup that you want to restore. The `target` is the region where you want the new deployment to be located, which can be a different region from the source deployment. Cross-region restores are supported, except for restoring into or out of the `eu-de` region.

If you need to adjust resources or use a Key Protect key, add any of the optional parameters `key_protect_key`, `members_disk_allocation_mb`, `members_memory_allocation_mb`, and `members_cpu_allocation_count`, and their preferred values to the body of the request.

## Backups and restoration
{: #dashboard-backups-restoration}

* {{site.data.keyword.databases-for}} are not responsible for restoration, timeliness, or validity of said backups.
* Actions that you take as a user can compromise the integrity of backups, such as under-allocating memory and disk. Users can monitor that backups were performed successfully via the API, and periodically restore a backup to ensure validity and integrity. Users can retrieve the most recent scheduled backup details from the [{{site.data.keyword.databases-for}} CLI plug-in](#backups-in-the-cli) and the [{{site.data.keyword.databases-for}} API](#backups-in-the-api).
* As a managed service, {{site.data.keyword.databases-for}} monitors the state of your backups and can attempt to remediate when possible. If you encounter issues you cannot recover from, contact support for more help.
