---

copyright:

  years: 2018, 2019

lastupdated: "2019-06-28"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Assigning permissions
{: #permissions}

Before users begin creating and working with an {{site.data.keyword.cfee_full}} (CFEE) service, their permissions must be set correctly by an administrator of the account where the CFEE instance is to be created.

If provisioning in a VPC cluster, users will also need [VPC permissions](https://cloud.ibm.com/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources).

## Permissions overview
{: #perm-summary}

Following is a summary of the minimum [IAM](https://cloud.ibm.com/iam#/users) and [Cloud Foundry role assignments](https://cloud.ibm.com/account/cloud-foundry) required for performing various tasks in an CFEE instance. The remaining section describes these permissions in more detail.

|  **Task** &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;|  **IAM Access Roles** &nbsp; &nbsp; &nbsp; |**Cloud Foundry Roles** &nbsp; &nbsp; &nbsp; |
|----------------------------------------|-------------------|-------------------|
|Create a CFEE |  <ul><li>Viewer role in Resource Group where CFEE is to be created.</li> <li>Editor role in the CFEE service.</li> <li>Administrator role in Kubernetes service.</li> <li>Administrator role in Databases for PostgreSQL service.</li> <li>[Infrastructure permissions](https://cloud.ibm.com/docs/containers?topic=containers-access_reference#infra) required to create a cluster.</li> <li>Editor platform role, and manager service access role in the IBM Cloud Object Storage service.</li> </ul> | |
|Update CFEE version |  <ul><li>Viewer role in CFEE Resource Group.</li> <li>Editor platform role in the CFEE service.</li></li> <li>Operator role in Kubernetes service.</li> <li>Editor role in Cloud Object Storage service.</li> </ul> | |
|Scale CFEE capacity (add/remove cells)|  <ul><li>Viewer role in the CFEE instance Resource Group.</li> <li>Administrator role in the CFEE instance.</li> <li>Operator role in Kubernetes service.</li></ul> |
|Monitor CFEE |  <ul><li>Viewer role in the CFEE instance Resource Group</li> <li>Editor role in the CFEE instance.</li> <li>Operator role in the CFEE's Kubernetes cluster.</li></ul> |  |
|View CFEE resource usage |  <ul><li>Viewer role in the CFEE instance Resource Group.</li> <li>Viewer role in the CFEE instance.</li></ul> |  |
|Enable CFEE auditing| <ul><li>Viewer role in the CFEE instance Resource Group.</li> <li>Editor role in the CFEE instance.</li></ul> | <ul><li>Auditor role in the public Cloud Foundry space where the Activity Tracker service instance is deployed.</li></ul>  |
|View CFEE auditing events| <ul><li>Viewer role in the CFEE instance Resource Group.</li> <li>Editor role in the CFEE instance.</li></ul> | <ul><li>Auditor role in the public Cloud Foundry space where the Activity Tracker service instance is deployed.</li></ul>  |
|Enable CFEE log persistance| <ul><li>Viewer role in the CFEE instance Resource Group</li> <li>Editor role in the CFEE instance.</li></ul> |<ul><li>Auditor role in the public Cloud Foundry space where the Log Analysis service instance is deployed.</li></ul>  |
|View CFEE persisted logs| <ul><li>Viewer role in the CFEE instance Resource Group</li> <li>Editor role in the CFEE instance.</li></ul> | <ul><li>Auditor role in the public Cloud Foundry space where the Log Analysis service instance is deployed.</li></ul> |
|Create CFEE organizations| <ul><li>Viewer role in the CFEE instance Resource Group</li> <li>Editor role in the CFEE instance.</li></ul> |  |
|Create CFEE spaces| <ul><li>Viewer role in the CFEE instance Resource Group</li> <li>Viewer role in the CFEE instance.</li></ul> | <ul><li>Manager in organization where space is to be created.</li></ul> |
|Manage shared domains|<ul><li>Viewer in the CFEE instance Resource Group. </li><li>Editor role in the CFEE instance. </li></ul>|  |
|View shared domains|<ul><li>Viewer in the CFEE instance Resource Group. </li><li>Viewer role in the CFEE instance. </li></ul>|  |
|Manage private domains|<ul> <li>Viewer in the CFEE instance Resource Group. </li><li>Viewer role in the CFEE instance. </li></ul>| <ul><li>Manager role in organization owning the domain. </li></ul>|
|View Private domains|<ul> <li>Viewer in the CFEE instance Resource Group </li><li>Viewer role in the CFEE instance. </li></ul>|<ul><li>Viewer role in organization owning the domain. </li></ul>|
|Create/Delete an IBM Cloud service instance in a CFEE space| <ul><li>Viewer role in Resource Group where CFEE is to be created.</li> <li>Viewer role in the CFEE instance.</li> <li>Editor in the Resource Group where the service instance is to be created, or to the IAM-managed service to be instantiated.</li> </ul>| <ul><li>Developer role in the CFEE space from where the service instance is created (and where will be added/aliased automatically).</li></ul> |
|Add/Remove an IBM Cloud service instance to/from a CFEE space (i.e., create/delete an alias to an IBM Cloud service in a CFEE space)| <ul><li>Viewer role in the CFEE instance Resource Group.</li><li>Viewer role in the CFEE instance. </li><li>Operator platform role and reader service role to the service instance to be added. </li></ul>|<ul><li>Developer role in the CFEE space where the service instance is to be added (aliased).</li></ul> |
|Bind or unbind an IBM Cloud service instace in a CFEE space|<ul> <li>Editor in the Resource Group of the service instance to bind or unbind.</li><li>Viewer role in the CFEE instance. </li><li>Operator platform role and writer service role to the service instance to bind.</li></ul> | <ul><li>Developer role in the CFEE space where the service instance to bind.</li></ul> |
|Issue `cf` cli commands|<ul> <li>Viewer role in the CFEE instance Resource Group. </li><li>Viewer role in the CFEE instance.</li></ul> | <ul><li>Cloud Foundry roles in the organization/space required to perform the command.</li></ul> |
{: caption="Table 1. Permissions required to perform tasks in a CFEE" caption-side="top"}

## Permissions required to create a new environment
{: #perm-creating}

In order to create new instances of the CFEE service, users must be granted access policies by an account administrator, not only to the CFEE service itself, but also to the supporting services that are also created automatically when the CFEE is created.

The following Identity & Access Management (IAM) access policies are required for users to be able to create an {{site.data.keyword.cfee_full_notm}} instance:

* _Viewer_ (or higher) accesss to the **_Default_** **resource group** in the {{site.data.keyword.Bluemix}} account. Resource groups allow organizing resources into customized groupings to facilitate access control to those resources. You are prompted for a resource group when you create a new environment instance. Access to the _Default_ resource group is required because this is always the resource group where the Kubernetes cluster is required.  Users can provision the CFEE instance in a diferent resource group, but the Kuberetes cluster will still be provisioned to the _Default_ resource group.  If a user provisions the CFEE in different user group, that users will required viewer access in that resource group.

* _Administrator_ or _editor_ role to **{{site.data.keyword.cfee_full_notm}} service** resources. In the resource group to which the environment is assigned. Users with either administrator or editor roles in the {{site.data.keyword.cfee_full_notm}} service can create and delete environments. But only users with an administration role can assign users to an {{site.data.keyword.cfee_full_notm}} instance or change the roles that are assigned to users in that instance.

* _Administrator_ role to the **Kubernetes Service** resources.  Instances of the {{site.data.keyword.cfee_full_notm}} are deployed on container cluster infrastructure, which is provided by the Kubernetes service. When you create an instance of the {{site.data.keyword.cfee_full_notm}} service, the service automatically creates a Kubernetes cluster. Access to the Kubernetes Service is required for creating that cluster infrastructure. You can scope access to the Kubernetes Service policy to the specific region where you intend to provision the CFEE instance, or scope the access to all regions. Additionally, make sure that you have the right [infrastructure permissions](https://cloud.ibm.com/docs/containers?topic=containers-access_reference#infra) required to create a Kubernetes cluster.

* _Administrator_ or _editor_ platform role, and manager service access role to the **IBM Cloud Object Storage service**, which is a required dependency of the CFEE service.  An instance of IBM Cloud Object Storage service is used to store data generated during the creation of your ICFEE application containers (e.g. uploaded application packages, buildpacks, and compiled executables).

* An instance of the Databases for PostgreSQL service is a required dependency of the CFEE service.  Databases for PostgreSQL is used to store Cloud Foundry data on your CFEE instance (e.g., auditing application deployment, start and stop events; keeping records of CFEE user membership, organizations, spaces, applications and service connections).

The following screen illustrates access policies as they would appear in the Identity & Access page of the {{site.data.keyword.Bluemix_notm}} that allow a user to create an {{site.data.keyword.cfee_full_notm}} instance.

![Access policies](img/AccessPolicies_Creator.png)

You can grant user permissions using the {{site.data.keyword.Bluemix}} command line.  You can also define an access policy for a user by specifying the parameters of the policy (i.e., services, roles, regions, etc) in a JSON formatted file that is invoked by the command that creates the policy.  See  [Assigning an IAM policy by using the command line](https://cloud.ibm.com/docs/cloud-monitoring/security/assign_policy.html#assign_policy_commandline) for more information, or issue `ibmcloud iam -help` in the command line. Note that this requires installing the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cli-install-ibmcloud-cli#install_use).
{:tip}

To confirm that you have the required access policies to create an {{site.data.keyword.cfee_full_notm}} instance:
1. Go to the [**Manage > Access(IAM) > Users**](https://cloud.ibm.com/iam/#/users) menu in the {{site.data.keyword.Bluemix_notm}} header to open the **Identity & Access** page.
2. In the Access policies tab, click the user who is creating the environment to assign and view the access policies for that user.

For more information about managing users and access in the {{site.data.keyword.Bluemix}}, including how to organize a set of users and service IDs to facilitate access assignment to multiple users at a time, see [Managing users and access](https://cloud.ibm.com/docs/iam/iamusermanage.html#iamusermanage).

### Expediting the setting of permissions to create an environment using the CLI
{: #permcli-creating}

You can expedite the setting of permissions for creating CFEE instances through the `ibmcloud cfee create-permission-set`.  The command allows a CFEE administrator to setup in a single command the required access policies for creating a CFEE instance and all its ancillary services.

The command sets the permissions to an IAM _Access Group_ and adds a user to that _Access Group_.  The administrator issuing the command can include in the command an existing _Access Group_.  If no _Access Group_ is provided, a default _cfee-provision-access-group_ is created automatically.

```
ibmcloud cfee create-permission-set USER_NAME [-ag, --access-group GROUP_NAME] [--output TYPE]
```
{: pre}

The command sets the following access policies for the target user in the current IBM Cloud account:

*  Editor roles in the CFEE and Cloud Object Storage services.
*  Administrator role in the Kubernetes service.
*  Administrator role in the Databases for PostgreSQL service.

For more details on the command issue the following:

```
cfee create-permission-set -help
```
{: pre}

You can use the `ibmcloud cfee create-permission-get` to find out or validate the access policies in place for a user:

```
ibmcloud cfee provision-permission-get USER_NAME [-ag, --access-group GROUP_NAME] [--output TYPE]
```
{: pre}

## Permissions required to work with an environment
{: #perm-working}

To work with a instance of the {{site.data.keyword.cfee_full_notm}}, users must be:
1. Members of the {{site.data.keyword.Bluemix_notm}} account where the {{site.data.keyword.cfee_full_notm}} instance was created.
2. Granted the following IAM _Access Policies_ by the account administrator (see the _Identity & Access_ page under the [**Manage > Access(IAM) > Users**](https://cloud.ibm.com/iam/#/users) menu in the {{site.data.keyword.Bluemix_notm}} header to check your current account access policies):

    Any user working in a CFEE instance needs a _viewer_ platform role (or higher) to:
  - The resource group under which the CFEE instance was created.
  - The CFEE instance itself.

   The level of access and control that users have in a CFEE instance depends on the role that is granted in their access policies:

  - Users with _viewer_ role to a CFEE instance can see it listed in the main {{site.data.keyword.Bluemix_notm}} dashboard and open its user interface. Users access to specific organizations and spaces within the environment is governed by the specific organization and spaces roles that are assigned by the managers of those organizations and spaces. For more information, see [Adding users to organizations](/docs/cloud-foundry?topic=cloud-foundry-adding_users).

  - Users assigned _administrator_ or _editor_ roles to a CFEE instance can create organizations, assign managers to organizations and spaces, have full permissions to all organizations and spaces within the environment, and perform operational actions through the Cloud Controller API. These users are automatically granted _cloud_controller.admin scope_ in the Cloud Foundry _User Account and Authentication scope_.

  - Users need _editor_ platform role or higher to a CFEE instance and _operator_ role or higher to the Kubernetes cluster into which the CFEE is provisioned to be able to **update the CFEE to a new version**.

  - Users need _administrator_ platform role to a CFEE instance and _operator_ role or higher to the Kubernetes cluster into which the CFEE is provisioned to be able to **change the capacity** of a cfee (adding or removing cells).

  - Users need _operator_ platform role (or higher) to an IBM Cloud service instance to be able to **add** that *service instance* to a CFEE space (i.e., to alias a service instance into a CFEE space).

  - Users need _operator_ platform role (or higher) and _writer_ service role (or higher) to an IBM Cloud service instance to be able to **bind** that service instance to an application deployed in a CFEE space.


## Best practices: Access Groups
{: #access-groups}

Consider using access groups to manage and simplify access control for your CFEE.  Access groups allow you to define arbitrary groups to which you can assign access policies.  Any user added to an access group is automatically assigned the group's access policy.

You can create and manage access groups from either the IBM Cloud user interface or through the `ibmcloud` cli.

From the user interface, go the menu bar, click **Manage > Access (IAM)**, and select [Access Groups](https://cloud.ibm.com/iam#/groups).

Alternatively, you can use the `ibmcloud` cli:

1. Create an access group:

  ```
  ibmcloud iam access-group-create GROUP_NAME [-d, --description DESCRIPTION]
```
  {: pre}

2. Create an access policy for that access group:

  ```
  ibmcloud iam access-group-policy-create GROUP_NAME
  ```
  {: pre}

3. Add users to the access group:

  ```
  ibmcloud iam access-group-user-add <user-name> [<user-name2...]
  ```
  {: pre}

<br>
For more information, see [Setting up access groups](https://cloud.ibm.com/docs/iam/groups.html#groups).
