---

copyright:
  years: 2021, 2022
lastupdated: "2022-10-14"

keywords: mysql workbench, mysql gui, mysql

subcollection: databases-for-mysql

---

{:shortdesc: .shortdesc}
{:external: .external target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}

# Creating an {{site.data.keyword.databases-for-mysql_full}} instance Tutorial
{: #mysql-create-instance-tutorial}

## Objectives
{: #mysql-create-instance-tutorial-objectives}

This tutorial guides you through the process of deploying an {{site.data.keyword.databases-for}} instance and connecting it to a web front-end. You will create a webpage that allows visitors to input a word and its definition whose values are then stored in a database or message queue running on {{site.data.keyword.databases-for}}. You will install the database infrastructure using [Terraform](https://www.terraform.io/){: external} and your web application will use the popular [Express](https://www.terraform.io/){: external} framework. The application can then be run locally, or by using [Docker](https://www.docker.com/){: external}.

## Getting productive 
{: #mysql-create-instance-tutorial-getting-started}

To begin the deployment process, install some must-have productivity tools:

* You need to have an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration).
* [Node.js and npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm){: external} - to install packages to and from public npm registries
* [Terraform](https://www.terraform.io/){: external} - to codify and deploy infrastructure
* *Optional* [Docker](https://www.docker.com/){: external} - to run your application non-locally

## Step 1: Obtain an API key to deploy infrastructure to your account
{: #mysql-create-instance-tutorial-step-1}

Follow [these steps](https://cloud.ibm.com/docs/account?topic=account-userapikey&interface=ui#create_user_key) to create an {{site.data.keyword.cloud_notm}} API key.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.{: .tip}

## Step 2: Clone the project
{: #mysql-create-instance-tutorial-step-2}

```sh
git clone https://github.com/IBM-Cloud/clouddatabases-helloworld-examples.git
```
{: pre}

## Step 3: Install the infrastructure
{: #mysql-create-instance-tutorial-step-2}

1. Open the [mysql](https://github.com/IBM-Cloud/clouddatabases-helloworld-examples/tree/master/mysql){: external} folder in the GitHub tutorial repository.

1. Create a document named `terraform.tfvars` with the following fields:

   ```sh
   ibmcloud_api_key = "<your_api_key_from_step_1>"
   region = "<your_region"
   admin_password  = "<make_up_a_password>"
   ```
   {: pre}
   
   The `terraform.tfvars` document contains variables that you may want to keep secret so it is ignored by the GitHub repository.{: .note}

1. Install the infrastructure with the following command:

   ```sh
   terraform init 
   terraform apply --auto-approve
   ```
   {: pre}
   
   The Terraform script will output configuration data that is needed to run the application, so copy it into the root folder:
   
   ```sh
   terraform output -json >../config.json
   ```
   {: pre}

## Step 4: Run your app locally
{: #mysql-create-instance-tutorial-step-3}

1. To connect to the database from your local machine, ensure that you are in the MySQL database, then install the node dependencies and run the service with the following command:

   ```sh
   npm install
   npm run start
   ```
   {: pre}
   
   If successful, the output will show you are connected:
   
   
   ```sh
   #Connected!
   #Server is listening on port 8080
   Open a browser and visit http://localhost:8080
   ```
   {: pre}
   
   A welcome page with a database logo will be displayed in your browser window.

1. To test the interface, enter a word and its definition. The data pair is added to the database and appears in a list at the bottom of the page.

## Step 5 (optional): Run the app from a Docker container
{: #mysql-create-instance-tutorial-step-3}

The first step towards hosting your application from a service like Code Engine is to containerize the app code inside a Docker container and run it from there.

Make sure you are logged into your Docker account. In the MySQL database, enter the following command:

```sh
docker build -t database-hello-world:1.0 . 
docker run -p 8080:8080 database-hello-world:1.0
```
{: pre}

Visit http://localhost:8080 to see the same page from the previous step.

Congratulations, you've created an app with a front-end that feeds data into your {{site.data.keyword.databases-for-mysql_full}} deployment!
