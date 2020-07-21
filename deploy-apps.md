---

copyright:
  years: 2018, 2020
lastupdated: "2020-07-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Deploying and viewing apps
{: #deploy_apps}

You can deploy applications to {{site.data.keyword.cfee_short}} with the command line interface or the integrated development environments (IDEs). You can also use application manifests to deploy applications. When you use an application manifest, you reduce the number of deployment details that you must specify every time that you deploy an application to {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

## Deploying applications with the Cloud Foundry command
{: #dep_apps}

[Download and install the Cloud Foundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon").

After you install the command line interface, follow these steps:

1. Change to the directory where your code is located. `cd <your_directory>`
2. Go to the {{site.data.keyword.cfee_full}} Overview page and locate the environment's API endpoint.
3. In the command line interface, set the API endpoint to your environment's endpoint:

  ```
  cf api <api_endpoint>
  ```
  {: pre}

4. Log in to the environment

  ```
  cf login -u <username> -o <org_name> -s <space_name>
  ```
  {: pre}

  If you're using a federated ID, use the `-sso` option.

  ```
  cf login -o <org_name> -s <space_name> -sso
  ```
  {: pre}

  **Note**: You must add single or double quotation marks around `username`, `org_name`, and  `space_name` if the value includes a space, for example, `-o "my org"`.

5.  From `<your_new_directory>`, redeploy your app to {{site.data.keyword.Bluemix_notm}} by using the `ibmcloud app push` command. For more information, see [`cf push`](/docs/cli/reference/ibmcloud?topic=cf-cli-plugin-cf-cli-plugin#cf_push).

  ```
  cf push <app_name>
  ```
  {: pre}

6.  Access your app by browsing to `https://<app_url>.<AppDomainName>`.

## Viewing deployed applications in the user interface
{: #view_apps}

You can view CFEE deployed applications either in the context of a specific space, or gobally across all CFEE instances.

### Viewing applications deployed in a specific space
{: #view_specific}

To view applications deployed in a specific space of a specific CFEE instance:
1. Go to the [{{site.data.keyword.Bluemix_notm}} dashboard](https://cloud.ibm.com/dashboard/apps/) and open the {{site.data.keyword.cfee_full_notm}} where you want to create organizations.
2. In the {{site.data.keyword.cfee_full_notm}} user interface, go to the **Organizations** entry in the navigation pane to open the _organizations_ page.
3. Go to the **Spaces** tab at the top of the page.
4. In the __Spaces__ tab, click on a space in the table to open the space's page.
5. In the __Space__ page, go to the **Applications** tab.
6. The __Applications_)_ tab shows all the applications deployed in that space.
Optionally, you can Start, Restart, Stop or Delete an application by accessing the menu in the far right of the row representing the application.

You can also expand the row for an application to view the service instances to which the application is bound.

### Viewing applications deployed across all instances
{: #view_global}

To view all applications deployed across all CFEE instances:
1. Click the Menu icon ![Account Checking](images/HamburgerMenu.png  "Screen cap that shows the menu icon") > **Cloud Foundry** to open the Cloud Foundry dashboard.  
2. Click **Enterprise > Applications** in the left navigation pane.
The table in the view shows the following information:

| Element   | Description |
|-----------|---------------|
| Name | The name of the application. |
| Instances | The number of instances of the application. |
| Environment | The {{site.data.keyword.cfee_full}} environment where the application is deployed. |
| Organization | The organization where the application is deployed. |
| Space | The organization in the CFEE instance where the alias resides. |
| Memory | The amount of memory used by the application. |
| Status | The status of the application. |
{:caption="Table 1. Columns in the applications table of the Cloud Foundry dashboard" caption-side="top"}

Optionally, you can Start, Restart, Stop or Delete an application by accessing the menu in the far right of the row representing the application.
You can click any of the applications, environments, organizations or spaces to navigate to the corresponding page in the CFEE user interface.

In this view you  have the option to group applications by organization, space, and/or application name.  This capability allows you to coalesce into a single entity applications that may have different names and/or be deployed into different CFEE organizations or spaces, but that correspond to the same logical application entity.  For example, you may have different applications that represent components of a broader project or that are deployed across different delivery stages (e.g., development, testing, production) but that you would like to view grouped as part of the same logical entity.

To group applications go to the **Group** dropdown located at the page's top-right.
When you group applications, each resulting group is represented by a single entry in the table. You can expand that row to show the applications under that group.

You can perform the following actions in this view:
* Sort the items in the table by any of the properties displays as table columns.
* Bind applications to a specific alias by accessing the alias overflow menu of the alias located at the row's far right.
* Expand an alias (row) to see all the applications bound that alias.
* Start, stop applications by accessing the application overflow menu of the alias located at the row's far right.
* Navigate to an CFEE, CFEE organization or CFEE space by clicking on the link with the corresponding CFEE, organization, or space.

## Application manifest
{: #appmanifest}

Application manifests include options that are applied to the `cf push` command. You can use an application manifest to reduce the number of deployment details that you must specify every time that you push an application to {{site.data.keyword.Bluemix_notm}}.

In application manifests, you can specify options such as the number of application instances to create, the amount of memory and disk quota to allocate, and other environment variables. You can also use application manifests to automate application deployments. The default name of a manifest file is `manifest.yml`.

### Supported options in the manifest file

The following table shows the supported options that you can use in an application manifest file. If you choose to use a different file name other than `manifest.yml`, you must use the **-f** option with the `cf push` command. In the following example, `appManifest.yml` is the file name:

```
cf push -f appManifest.yml
```

| Options | Description | Usage or example |
|:----------|:--------------|:---------------|
| **buildpack** | The URL or name of the custom buildpack. | `buildpack:` *buildpack_URL* |
| **disk_quota** | The disk quota that is allocated for an application. The default value is 1 G. | `disk_quota: 500M` |
| **domain** | The domain name of the application in {{site.data.keyword.Bluemix_notm}}. | `domain:` ng.cloud.ibm.com |
| **host** | The host name of the application in {{site.data.keyword.Bluemix_notm}}. This value must be unique in the {{site.data.keyword.Bluemix_notm}} environment. | `host:` *host_name* |
| **name** | The application name in {{site.data.keyword.Bluemix_notm}}. This value must be unique in the {{site.data.keyword.Bluemix_notm}} environment. | `name:` *appname* |
| **path** | The location of your application. This value can be a relative path or absolute path. | `path:` *path_to_application* |
| **command** | The custom start command for your application, or the command to run script files. | `command:` *custom_command* `command:` *bash ./run.sh* |
| **memory** | The amount of memory to allocate for the application. The default value is 1G. | `memory: 512M` |
| **instances** | The number of instances to be created for your application. | `instances: 2` |
| **timeout** | The maximum amount of time in seconds that is used to start the application. The default value is 60 seconds. | `timeout: 80` |
| **no-route** | A Boolean value to prevent a route from being assigned to the application if the application is just running background. The default value is **false**. | `no-route: true` |
| **random-route** | A Boolean value to assign a random route to the application. The default value is **false**. | `random-route: true` |
| **services** | The services to bind to the application. | `services: - mysql_maptest` |
| **env** | The custom environment variables for the application. | `env: DEV_ENV: production` |
{: caption="Table 2. Supported options in the manifest YAML file" caption-side="top"}

### A sample manifest.yml file
{: #sample}

The following example shows a manifest file for a Node.js application that uses the built-in community Node.js buildpack in {{site.data.keyword.Bluemix_notm}}.

```
---
applications:
- name: myNodejsapp
  memory: 256M
  disk_quota: 512M
  path: /dev/myNodejsApp
  buildpack: nodejs_buildpack
  host: nodejs001
  domain: mycloud.com
  command: node app.js
  timeout: 80
  services:
  - mongo_8917
  env:
    env_type: production
```
{: codeblock}
