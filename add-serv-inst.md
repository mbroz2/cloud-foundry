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

# Working with services
{: #workingwith-services}

Applications deployed in an {{site.data.keyword.cfee_full_notm}} can be bound to two types of service instances:

1. Public service instances created from the {{site.data.keyword.Bluemix}} catalog and available in the {{site.data.keyword.Bluemix}} account.  
Public service instances available in the {{site.data.keyword.Bluemix}} account are not automatically available, by themselves, to CFEE environments.  In order for a public service instance (available in the {{site.data.keyword.Bluemix}} account) to become available to spaces in an CFEE environment, they must be specifically added to the target CFEE space. When you _add_ a public service instance to a CFEE  through the CFEE's user interface, an alias of the public service instance is placed in the CFEE.  Once the {{site.data.keyword.Bluemix}} service instance is added (aliased) to the CFEE space, it can be bound to applications in that CFEE space.  This allows developers to leverage the vast catalog of {{site.data.keyword.Bluemix}} services in their applications deployed in CFEE environments, while allowing access control to those {{site.data.keyword.Bluemix}} services.

   In addition to adding existing {{site.data.keyword.Bluemix}} service instances to a CFEE space you can create a new {{site.data.keyword.Bluemix}} service instance from within a CFEE space, which also adds it automatically to that CFEE space.
  
2. Service instances managed by a local Cloud Foundry service broker (local to the CFEE). These in turn can be of two types:
   *  2a. An instance of a service offering created from the Cloud Foundry marketplace of the current {{site.data.keyword.cfee_full_notm}} instance. This type of service instance requires a registered service broker available in the environment. A service broker shows a catalog of service offerings and plans, as well as enables the creation, removal, binding and unbinding of instances from those service offerings. See the [Managing Service Brokers](https://docs.cloudfoundry.org/services/managing-service-brokers.html) in the Cloud Foundry documentation for more information.
   * 2b. A user-provided service instance. Creation of this type is supported through the Command Line Interface (CLI), but not through the user interface. Nonetheless, user-provided service instances will be listed in the user interface.
   
You can also create a service instance from a CFEE space, either from the the CFEE's user interface or using the CLI (see sections below).  Creating a service instance from a CFEE using this command has a double result:
* Creates a service instance in the public {{site.data.keyword.Bluemix}}.
* Creates an alias of that public service instance inside the CFEE space from which the service instance was created.
    
The following sections describe how you can add, create, control access, and bind service instances in the user interface and with the Cloud Foundry CLI.
   

## Viewing {{site.data.keyword.Bluemix_notm}} service instances across all environments
{: #viewing-services_across}

Go to the [Cloud Foundry services dashboard](https://cloud.ibm.com/dashboard/cloudfoundry/services) to see a consolidated view of all the {{site.data.keyword.Bluemix_notm}} service instances (to which you have access) in the {{site.data.keyword.Bluemix_notm}} account and see which ones have been added to which CFEE spaces.

This view shows all the {{site.data.keyword.Bluemix_notm}} service instances available in the account and indicates which ones have been added (aliased) to which CFEE spaces. Expand a service instance to show the CFEE spaces where it has been added.  You can further expand those to see the applications in those CFEE spaces to which those service instances have been bound.  

From this view you can also bind applications deployed into the CFEE spaces to which those service instances have been added. Go to the menu located at the far right of the table row for the corresponding service (available to a specific CFEE space) and select **Bind to applications**.

## Adding existing {{site.data.keyword.Bluemix_notm}} service instances to a space
{: #adding-services-inspace}

You can bind an {{site.data.keyword.Bluemix_notm}} service instances to applications deployed into a CFEE space.  Before an IBM Cloud service instance can be bound to appplication deployed in CFEE it has to be added to the CFEE space in which the application is deployed. The following is required before you can add an {{site.data.keyword.Bluemix_notm}} service instance to a CFEE Space:
* The service whose instance is to be added cannot be a Cloud Foundry service.  Only {{site.data.keyword.Bluemix_notm}} services that support resource groups and IAM can be added (aliased) into a CFEE. Services instantiated into public Cloud Foundry orgs, spaces, and roles cannot be added (aliased) into a CFEE.  {{site.data.keyword.Bluemix_notm}} services are progressively moving to benefit from resource groups.  Once a services moves to using resource groups instead of Cloud Foundry orgs and spaces, you can migrate any previously existing instances of that service to a resource group, at which point it can be added to a CFEE.  See [Migrating Cloud Foundry service instances to a resource group](https://cloud.ibm.com/docs/resources/instance_migration.html#migrate).
* The {{site.data.keyword.Bluemix_notm}} service must be available in the same {{site.data.keyword.Bluemix_notm}} account where the CFEE instance resides.
* You must have _operator_ platform role (or higher) to the {{site.data.keyword.Bluemix_notm}} service instance itself. For more information, see the Identity & Access page under the **Manage > Users** menu in the {{site.data.keyword.Bluemix_notm}} header to check your current account access policies.

To Add an existing {{site.data.keyword.Bluemix_notm}} service instance to a CFEE space:
1. Go to the [Cloud Foundry services dashboard](https://cloud.ibm.com/dashboard/cloudfoundry/services).  
2. Locate the {{site.data.keyword.Bluemix_notm}} service instance from the list and invoke the menu at the far right of the corresponding table row.
3. Select **Add service**.
4. In the _Add service_ dialog, select the CFEE, org and space where you want to add the service, and click **Add**.
5. Expand the target {{site.data.keyword.Bluemix_notm}} service to find the new CFEE where the service has been added.


Alternatively, you can add {{site.data.keyword.Bluemix_notm}} service instance from within the CFEE space where you want to add it:
1. In your {{site.data.keyword.Bluemix_notm}} [dashboard](https://cloud.ibm.com/dashboard) or [Cloud Foundry services dashboard](https://cloud.ibm.com/dashboard/cloudfoundry/services), find the Cloud Foundry Enterprise environment that hosts your application.
2. Go to **Organizations** in the navigation pane and open the organization and space where the application is located.
3. Go to the **Spaces** tab and locate the space that contains the application.
4. In the target space page, go to the **Services** tab and click **Add service**.  This will open the __Add service__ dialog.  The page lists service instances available in the {{site.data.keyword.Bluemix_notm}} account.
5. Find and select an available service instance you want to add to the CFEE space. 
6. The service instance will be added to the list of services available in the CFEE space.

   **Note:** When you add an {{site.data.keyword.Bluemix_notm}} service to a CFEE space, an alias to that service instance is created in that space. When you **Remove** the service instance from a CFEE space (from the menu located in the far right of the corresponding service instance row in the table), the service is not deleted from the public account.  The action simply removes it from the specific CFEE account and is no longer available for binding to applications in that CFEE space.  Also, when you delete the service instance itself from the {{site.data.keyword.Bluemix_notm}} account, the service is no longer available to any CFEE spaces. 

## Creating {{site.data.keyword.Bluemix_notm}} service instances from a space's user interface
{: #creating-services-inspace}

You can create {{site.data.keyword.Bluemix_notm}} service instances from within a CFEE space.  Creating an {{site.data.keyword.Bluemix_notm}} service instance results in the following:
* The {{site.data.keyword.Bluemix_notm}} service instance is created in the IBM Cloud.  This is equivalent to creating the service instance from the {{site.data.keyword.Bluemix_notm}} [catalog](https://cloud.ibm.com/catalog).
* An alias to the {{site.data.keyword.Bluemix_notm}} service instance is added (aliased) to the CFEE space from which the creation action was initiated. An alias service instance (in a CFEE space) is a reference to the actual service instance (in the IBM Cloud account). The service instance alias allows developers to interact with the service instance (e.g., binding to applications) by handling all the credentials automatically for interacting with the service instance. .

To create a service instance from within a CFEE space:
1. In the target space page, go to the **Services** tab and click **Create service**.  This will open the **Marketplace** view for the {{site.data.keyword.cfee_full_notm}}.  The view lists services offerings from two sources (See __Source__ column):
   * Offerings from the {{site.data.keyword.Bluemix_notm}} catalog .
   * Offerings from the local {{site.data.keyword.cfee_full_notm}} marketplace.
5. Find and select an available service offering that you want to instantiate. 
6. Depending on the source of the service offering (IBM Cloud catalog or local CFEE marketplace) proceed to one of the following:
   * If the selected service offering is an {{site.data.keyword.Bluemix_notm}} catalog offering, you will see a button to **Continue** to the IBM Cloud service offering creation page.  The is the same page available from the {{site.data.keyword.Bluemix_notm}} catalog.  In this page you provide the name of the new service instance, region, plan and other properties required to create the service in the IBM Cloud.  Once you create the service instance will be created in the IBM Cloud and automatically added to the current CFEE space.
   * If the the selected service offering comes from the local CFEE catalog,  you will see a button to **Create** the service instance. You will be prompted for an organization and space in the current CFEE where the service instance will be created.

## Creating {{site.data.keyword.Bluemix_notm}} service instances from a space using the Cloud Foundry CLI
{: #creating-services_cli}

You can create a service instance from the CLI from a space in a CFEE.  Creating a service from the CLI in a CFEE space is equivalent to creating it from the UI in that space. That is, creating a service has the following double result:
* The {{site.data.keyword.Bluemix_notm}} service instance is created in the IBM Cloud.  This is equivalent to creating the service instance from the {{site.data.keyword.Bluemix_notm}} [catalog](https://cloud.ibm.com/catalog).
* The {{site.data.keyword.Bluemix_notm}} service instance is added (alised) into the CFEE space from which the creation action was initiated.

The difference between creating a service instance from a CFEE space vs. creating it from the CLI is in the lifecycle of the created service instance.  When you create the service instance in the CFEE space UI the lifecycle of the created service instance is controlled from the service instance itself in the {{site.data.keyword.Bluemix_notm}} account.  This means that updates to the service plan is controlled from the instance in the {{site.data.keyword.Bluemix_notm}} acount, not from the CFEE space.  Alternatively, if the service instance is created from the CLI, the service's lifecycle is controlled from the CFEE space (the service is updated from the CFEE space).

Service instances created from the CLI are also displayed in the UI, and are indicated by an ` * ` next to the service instance name.

To create service instances using the CLI follow the following steps:

1. Download and install the {{site.data.keyword.Bluemix}} command line interface (if you haven't done it already). [Download the Cloud Foundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon").

2. Go to the {{site.data.keyword.cfee_full}} Overview page and locate the environment's API endpoint.

3. In the command line interface, set the API endpoint to your CFEE environment's endpoint:

  ```
  cf api <api_enpoint>
  ```
  {: pre}

4. Log into the CFEE environment:

  ```
  cf login -u <username> -o <org_name> -s <space_name>
  ```
  {: pre}

  If you're using a federated ID, use the `-sso` option:

  ```
  cf login -o <org_name> -s <space_name> -sso
  ```
  {: pre}
  
5. Create the service:
You can issue a `cf create-service` command to create a service instance in the CFEE space from which the command is issued.  Creating a service instance from a CFEE using this command has a double result:
    * Creates a service instance in the public {{site.data.keyword.Bluemix}}, with a name provided by the `instance_name` parameter. If no `instance_name` is provided in the command, a name will be given by the {{site.data.keyword.Bluemix}} resource controler.  
    * Creates an alias of that public service instance inside the CFEE space from which the commmand was issued, with a name specified by `SERVICE_INSTANCE`.  Note that name of the public service instance will not automatically be the same name as the service instance in the CFEE. Thus, if you want to give the same name to both, the public service instance and the service instance in the CFEE (alias of the public one), you need to specify both, an `instance_name` and a `SERVICE_INSTANCE` (with the same values) in the following command:

  ```
  cf create-service SERVICE PLAN SERVICE_INSTANCE -c '{"instance_name":"value", "resource_group":"value", "target":"value"}'
  ```
  {: pre}
  
The following example creates an instance of the Cloudant service (standard plan) named "myCloudant" (with the same name in both, the public instance and the CFEE alias) in the "bluemix-us-south region", and group it in a specific resoure group":

  ```
  cf create-service cloudant standard myCloudant -c '{"instance_name":"myCloudant", "resource_group":"b0daaf6c3ccd4392a266da916cce2e8c", "target":"bluemix-us-south"}'
  ```
  {: pre}

 The `create-service` command can be issued with optional parameters to handle specific use cases:
 
   * Instance name (`instance_name`): Allows you to specify a custom name for the service instance created in the public {{site.data.keyword.Bluemix}}. If no `instance_name` value is provided, the default name of the public service instance in the public will be different from the `SERVICE_INSTANCE` (the name of the instance in the CFEE). We recommend that you use this parameter to control the name of the public service instance, or if you want to give it the same name as the service instance in the CFEE (alias to the public one).
   * Resource group (`resource_group`).  Allows you to specify a resource group under which to place the new instance.  Note that the value of this parameter must be the resource group's ID (_resource group ID_), not the resource group's name (_resource group name_).  You can find the _resource group ID_ of a specific resource group with the command `ibmcloud resource groups`.
   * Target deployment region (`target`). The region where the service instance is to be provisioned. Note that some services do not require specification of a target.  However, the command will fail if no target is provided when the service being created does require it.

Optionally, you can provide a JSON file containing those parameters. For example:
  ```
  {
    "NewCloudant": 
        {  
        "instance_name":"myCloudant",
        "resource_group": "b0daaf6c3ccd4392a266da916cce2e8c",
        "target": "bluemix-us-south"
        }
   }
  ```
  {: pre}
  
The JSON file can be then invoked as part of the `cf create-service` command. The path to the JSON file can be an absolute or relative:
  ```
  cf create-service SERVICE PLAN SERVICE_INSTANCE -c PATH_TO_FILE
  ```
  {: pre}
   
You can get more details by issuing the following:
  ```
  cf create-service -help
  ```
<br>
See [Managing Service Instances with the cf CLI](https://docs.cloudfoundry.org/devguide/services/managing-services.html){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") in the Cloud Foundry documentation for more information.
  
## Binding services to applications from the user interface
{: #bind-services-ui}

Users with _operator_ platform role (or higher) and _writer_ service role (or higher) to an {{site.data.keyword.Bluemix_notm}} service instance can bind that instance to an application deployed in a CFEE space,  either from the [Cloud Foundry services dashboard](https://cloud.ibm.com/dashboard/cloudfoundry/services) or from the Services tab of a CFEE space's user interface.   

To bind a service instance to an application from the [Cloud Foundry services dashboard](https://cloud.ibm.com/dashboard/cloudfoundry/services):
1. Go to the [Cloud Foundry services dashboard](https://cloud.ibm.com/dashboard/cloudfoundry/services) to see a consolidated view of all the {{site.data.keyword.Bluemix_notm}} service instances available to you in the {{site.data.keyword.Bluemix_notm}} account.  The view is also intended to show which {{site.data.keyword.Bluemix_notm}} services instances are available (aliased into) in which CFEE spaces. You can expand a service instance to show the CFEE spaces where it has been added.  You can further expand those to see the applications in those CFEE spaces to which those service instances have been bound. 
2. Locate the {{site.data.keyword.Bluemix_notm}} service instance you want to bind.
3. Expand the corresponding view to see all the CFEE spaces where that service instance has been added. If that service instance has not been added to the CFEE space where the application is deployed, select **Add** from the menu at the row's far right to make the service instance available in the target CFEE space.
4. Go to the menu located in the far right of the row corresponding to the CFEE/space where the service instance is available, and select **Bind application**.
5. In the __Bind application__ dialog select the application you want to bind. 

   **Note:** If the to-be-bound service supports both, public and private endpoints (see [IBM Cloud Service Endpoints](https://cloud.ibm.com/docs/service-endpoint?topic=service-endpoint-about#about), and/or supports multiple [service access roles](https://cloud.ibm.com/docs/iam?topic=iam-iamconcepts#am) (which specify the allowable actions over the service instance that can be carried out through the binding), a multi-step dialog will prompt you to select an endpoint type (public or private) and/or a specific service role.
6. The application is now bound to the service instance.  You can expand a service instance in the table to see the applications bound to it (in a specific CFEE space).


To bind an application to a service instance from the __Services__ page of a CFEE space:

1. Open the CFEE user interface where the application is deployed.
2. Go to **Organizations** in the left navigation pane and open the organization and space where the application is deployed.
3. Go to the **Spaces** tab and locate the space that contains the application.
4. In the target space page, go to the **Services** tab.
5. In the table of **Service instances**, go to the __Actions__ menu in the far right of the row corresponding to the service instance you want to bind, and select **Bind**.
6. Select the application that you want to bind to the service instance.

To unbind an application from a service instance:

1. In the **Services** tab of the space, expand the target service instance to show the apps that are bound to it.
2. Go the Actions menu in an application's row and select **Unbind service**.

## Binding services to applications using the Cloud Foundry CLI
{: #bind-services-cli}

You can use the `cf bind-service` command to bind a service instance to an application in a CFEE:

 ```
  cf bind-service APP_NAME SERVICE_INSTANCE -c '{"parameter": "value"}' 
  ```
  {: pre}

In the following cases the command requires special parameters:

* When you are using the `cf bind-service` command to bind an app to a service instance that supports [IBM Cloud Service Endpoints](https://cloud.ibm.com/docs/service-endpoint?topic=service-endpoint-about#about) that connect to IBM Cloud services over the IBM Cloud private network. To bind an application (deployed in a CFEE) to a service that supports both, public and private endpoints, you need to specify which one to use for the bindings.  In the following example the commands binds the service using the private endpoint

  ```
  cf bind-service myApplication myServiceInstance -c '{"service-endpoints":"private"}' 
  ```
  {: pre}

* When the service has multiple [service access roles](https://cloud.ibm.com/docs/iam?topic=iam-iamconcepts#am). Service access roles specify the allowable actions over the service instance that can be carried out through the binding.  You can specify a specific service access role through the `role` parameter. In the following example the binding specifies a "writer" service access role:

  ```
  cf bind-service myApplication myServiceInstance -c '{"role": "writer"}' 
  ```
  {: pre}
<br>
See [Bind a Service Instance](https://docs.cloudfoundry.org/devguide/services/application-binding.html){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") in the Cloud Foundry documentation for more information about binding applications using the `cf bind-service` CLI command.

## Service visibility
{: #service_visibility}

Customers can control who can create new IBM Cloud services from a CFEE environment.  They can also control who can add existing IBM Cloud services to a CFEE space.  This control is exerted by controlling the visibility of service offerings (and/or instances of those service offerings) to users.

### Controlling the creation of service instances
{: #control_servicecreation}

Controlling the creation of services can be accomplished by controlling the visibility of service offerings in the IBM Cloud Catalog for either all users in the IBM Cloud account or for users in specific Cloud Foundry organizations.

#### Controlling visibility to users in an IBM Cloud account to the IBM Cloud catalog
{: #creation_accountvisibility}

IBM Cloud account administrators (users with _Administrator_ role to all IAM enabled services) can prevent a service or service plan from showing int the catalog for all users in a given **IBM Cloud account**.  Account administrators can prevent all account users from using a service by blacklisting a service or service plan for all the users in an account. Blacklisting a service will prevent users in that account from even seeing that service (or service plan) in the IBM Cloud catalog.  This is done through an `ibmcloud catalog blacklist` command which has the following syntax:

  ```
  ibmcloud catalog blacklist [--add service NAME or entry ID] [--remove service NAME or entry ID] [--service-list] [--output TYPE]
  ```

In its simplest form, you can use the `catalog backlist` command to make a service offering (all its plans) invisibile to all users in the current account:

  ```
  Ibmcloud catalog blacklist [--add service NAME or entry ID] [--remove service NAME or entry ID] [--service-list] [--output TYPE]

  ```
  {: pre}

You could blacklist visibility not just generally for the entire service offering, but for a specific plan in a specific region.  For this, though you need to use the ID for the corresponding entry in the global catalog, not the name.   If you want to prevent users from seeing a specific plan in a specific region you can issue the following:

  ```
  Ibmcloud catalog blacklist -add <catalogEntryID>
  ```
  {: pre}
  
 To remove services from the blacklist:
 ```
  Ibmcloud catalog blacklist -remove <catalogEntryID>
  ```
  {: pre}
 
You can find the ID of a given catalog entry (e.g., a service's plan and its deployment region) through the following command. The command returns all the plans and their deployment regions for a given service:

  ```
  ibmcloud catalog service <thisService>
  ```
  {: pre}

You can find more details for the `ibmcloud catalog blacklist` by issuing the following:

  ```
  Ibmcloud catalog blacklist -help
  ```
  {: pre}


#### Controlling visibility to users in a Cloud Foundry organization
{: #creation_orgvisibility}

You can use the `cf enable-service-access` and `cf disable-service-access` commands to control access to services for specific **Cloud Foundry organizations**.  Here the control point for access is Cloud Foundry (not the IBM Cloud Account).  Note that this is a native `cf` command (not an `ibmcloud` command). 

Consider the following when setting service visibility through the `cf` CLI:

*  `cf` commands enable and/or disable service offerings (optionally, specific service offering plans) for all users of specific Cloud Foundry organizations
* Enablement works by building an organization “whitelist”. That is, an inclusion list of Cloud Foundry organizations that do have access to a service (as opposed to the "blacklist" approach in the `ibmcloud` command described earlier, which works by defining a list of services excluded from visibility to account users). This means that access control to a catalog service with this approach works, not by defining which organizations cannot see a service (blacklisting), but by defining which organizations can see it (whitelisting).
*  When you give an organization access to a service (or service plan) by adding that organization to a whitelist, you are de facto disabling (excluding) all other organization from accessing it.  That is, once you whitelist an organization, you are disabling all other organization from access to that service (or service plan)
*  Before you add organizations to the "can see" list, you have to disable visibility for all the organizations.  You cannot disable access to a service plan if the plan is currently available to all organizations.

In accordance to the general behavior described above, we recommend to control organization access to services by issuing the following commands:

  * **Disable** access to a service plan for all orgs:
  ```
  cf disable-service-access SERVICE [-p PLAN] [-o ORG]
  ```
  {: pre}
  
  The following example disables access to the the standard plan of the Cloudant service to all memberes of MyOrg:
  ```
  cf disable-service-access cloudant -p standard -o MyOrg
  ```
  {: pre}

  
  * **Enable** access to the service plan for specific CFEE organizations.  This will disable the service plan for all other organizations not specifically enabled in the command. 
  ```
  cf enable-service-access SERVICE [-p PLAN] [-o ORG]
  ```
  {: pre}

<br>  
**Note:** We recommend to execute the Cloud Foundry enable and disable service commands for the entire service, not for specific plans.


### Controlling access to existing service instances
{: #control_serviceaddition}

Developers in a CFEE space can use existing service instances available in the IBM Cloud account.  Within a specific space of a CFEE, users can **Add** a service instance to that space (See [Adding existing service instances](https://cloud.ibm.com/docs/cloud-foundry/add-serv-inst.html#workingwith-services#adding-services-inspace)above). Adding a service instance to a CFEE space creates an alias (a reference) to the service instance, which allows a developer to bind the service instance to an application deployed into that CFEE space as if it were the actual service instance.

Account administrators can control the use of service instances through [IAM access policies](https://cloud.ibm.com/docs/iam/iamusermanage.html#iamusermanage).  From either the UI or the ibmcloud CLI they can assign roles to either the service instances themselves or to the resource groups in which those instances resided.  A developer would need _Viewer_ role or higher to a service instance (or its resource group) before they can add that instance to a space.

You can find videos with in-depth discussions and demonstrations on CFEE services in the [CFEE video playlist](https://ibm.biz/CFEE-Playlist){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon").
{:tip}
