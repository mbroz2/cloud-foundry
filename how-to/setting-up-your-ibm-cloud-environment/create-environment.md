---

copyright:
  years: 2017, 2019
lastupdated: "2019-09-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Creating a CFEE instance
{: #create-environment}

## Prerequisites
* Ensure that you are in the {{site.data.keyword.Bluemix_notm}} account where you want to create the environment.
* Ensure that you have the correct [permissions](https://cloud.ibm.com/docs/cloud-foundry?topic=cloud-foundry-permissions
) before creating instances of the {{site.data.keyword.cfee_full_notm}}. 
* [Prepare your IBM Cloud account](https://cloud.ibm.com/docs/cloud-foundry/prepare-account.html) to ensure that it can create the infrastructure resources necessary for a CFEE instance (CFEE instances are deployed into Kubernetes worker nodes from the Kubernetes Service).  

## Creating an instance
1.  Go to the {{site.data.keyword.Bluemix_notm}} [catalog](https://cloud.ibm.com/catalog).

2.  Locate the {{site.data.keyword.cfee_full_notm}} service in the catalog and click on it to open the overview page for the service.  That first page provides an overview of the main features. Click **Continue**.

3.  Configure the CFEE instance to be created:
    * Select a plan. See the [Eirini technical preview](https://cloud.ibm.com/docs/cloud-foundry?topic=cloud-foundry-create-environment#create-environment#eirini) section for a description of the limitations of this plan.
    * Enter a **Name** for the CFEE instance.
    * Select a **Resource group** under which the environment is grouped. Only those resource groups to which you have access in the current IBM Cloud account will be listed in the _Resourouce groups_ dropdown, which means that you need to have permission to access at least one resource group in the account to be able to create an CFEE.
    * Select the **Geography** and **Location** where the service instance is to be provisioned. See the list of [available provisioning locations and data centers](https://cloud.ibm.com/catalog/docs/cloud-foundry/index.html#provisioning-targets){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon") by geography for CFEE and supporting services. 

4. CFEE instances are provisioned on a Kubernetes cluster. Cloud Foundry cells are provisoned within worker zones in the Kuberentes cluster. You can configure the location, hardware and encryption of the cluster:
    * Select the **Geography** and **Region** where the Kubernetes cluster willl be provisioned.
    * Select the **Zones** where the Kubernetes worker nodes will be deployed. You have the option of provision the worker nodes in the same zone (**Single Zone**) or in multiple zones (**Multizone**).  **Note** that creating a multizone CFEE requires [VLAN spanning](https://cloud.ibm.com/docs/containers?topic=containers-subnets#vlan-spanning), which is also supported when the {{site.data.keyword.Bluemix_notm}} account is [VRF enabled](https://cloud.ibm.com/docs/infrastructure/direct-link/vrf-on-ibm-cloud.html#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud).
    
    Available VLANs are automatically retrieved for the selected zones. If no available VLANs are available, they will be created automatically.
    
    **Note: Provisioning on an isolated network:** You can target VLANs in an isolated network. When the CFEE instance is created in an isolated network, the policies in the isolated network (e.g., calico policies or VRA rules) must allow ALL outbound access from the CFEE instance for the CFEE to be provisioned successfully. Once the CFEE is created, the outbound access restrictions can be reinstated, except for outbound access to certain services and micro-services in the IBM Cloud.  See the [Operating in an isolated network](https://cloud.ibm.com/docs/cloud-foundry?topic=cloud-foundry-isolated-network#isolated-network) documentation for more information.
    
    Once the CFEE is created, the Kubernetes cluster into which the environment is provisioned will appear (like any other resource in your IBM Cloud account) in the {{site.data.keyword.Bluemix_notm}} [dashboard](https://cloud.ibm.com/catalog/dashboard/apps/). For more information, see [Kubernetes Service documentation](/docs/containers?topic=containers-getting-started#getting-started).

5. Configure the security of the CFEE:
    * Select the **hardware isolation** for the Kubernetes cluster:   
      * _Virtual-shared_: Infrastructure resources in the cluster, such as the hypervisor and physical hardware, are distributed between you and other IBM customers, but each worker node is single-tenant to you.
      * _Virtual-dedicated_: The worker nodes in the cluster are hosted on infrastructure that is devoted to your account.
    * Data in the cluster is **encrypted** by default. You have the option of turning off _Encrypt local disk_.
    * **Service access endpoints used for management:** If the IBM Cloud account is enabled for Virtual Routing & Fowarding ([VRF](https://cloud.ibm.com/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)) and IBM [Cloud Service Endpoint](https://cloud.ibm.com/docs/service-endpoint?topic=service-endpoint-getting-started#getting-started), management access by the CFEE control plane to the new CFEE and its supporting services (Kubernetes cluster, Databases for PostgreSQL and Cloud Object Storage) will take place through the IBM private network.  One the CFEE instance is created, administrators can see information about the endpoints used for management access in the **Management access** page (located in the upper right corner of the _Overview_ page in the CFEE user interface). [Learn more](https://cloud.ibm.com/docs/cloud-foundry?topic=cloud-foundry-isolated-network#private-access)

6.  Configure the capacity of the CFEE:
    * Select the **Number of cells** for the CFEE. These are the application cells that will host the applications deployed into the CFEE.  
    * Select the **Cell node size**, which determines the capacity of the Cloud Foundry cells (CPU and memory).
    * Select the **Control plane node size**, which determines the capacity (CPU and memory) of additional nodes which are created and reserved for the operation and control of the Cloud Foundry platform in your CFEE. In order to fully utilize Cloud Foundry cell capacity, your Control plane nodes must have a capacity greater than or equal your Cell node capacity.

    **Number of Control Plane nodes:** In a single-zone CFEE, 2 Control plane nodes are created. In a multi-zone CFEE, a single Control plane node is created in each zone.

    **Customer-available memory:** A CFEE's available memory is equal to the memory available to its cell nodes minus system memory. System memory is consumed by Cloud Foundry services which run on each individual cell.

    **Note:** We recommend that VLAN spanning be enabled if the worker nodes in the Kubernetes cluster are provisioned on different subnets.  Worker nodes on different subnets may prevent connectivity among them if VLAN spanning is disabled.  See [VLAN spanning](/docs/containers?topic=containers-subnets#vlan-spanning) documentation for more information.

7.  The **Order Summary** in the right-hand side of the page reflects the cost of the CFEE instance and the ancillary services along with the estimated monthly total.

8.  Click **Create**, which opens the environment dashboard and indicates the creation progress and status.

**Note:** Once the creation process has started, a table row for the CFEE is shown in the {{site.data.keyword.Bluemix_notm}} dashboard, in the _Resource List_, and in the [Cloud Foundry Environments dashboard](https://cloud.ibm.com/resources?filter=cf_environments).  The status in the table row indicates when creation is completed.

The automated process that creates the environment deploys the infrastructure into a Kubernetes cluster and configures it to make it ready for use. The process takes 90 - 120 minutes.

Once CFEE is created you will receive multiple emails confirming the provisioning of the CFEE and supporting services, as well as emails notifying you of the status of the corresponding orders.

When you create a CFEE instance, there are three additional supporting service instances created in the same IBM Cloud account. Those supporting service instances are named after the CFEE instance name. Hence, creating a CFEE results in a total of four service instances created in the IBM Cloud account:
* CFEE instance ("_cfeename_").
* Kubernetes cluster ("_cfeename_-cluster"). The cluster provides the infrastructure into which the CFEE instance is provisioned.
* Cloud Object Storage instance ("_cfeename_-cos"). The instance is used to store data generated during the creation of the CFEE application containers (e.g. uploaded application packages, buildpacks, and compiled executables).
* Compose for PosgreSQL instance ("_cfeename_-postgres"). The instance is used to store Cloud Foundry data on the CFEE instance (e.g., auditing application deployment, start and stop events; keeping records of CFEE user membership, organizations, spaces, applications and service connections). 

## The Eirini technical preview plan
{: #eirini}

 The Eirini Technical Preview plan allows you to explore a CFEE using native Kubernetes as the container scheduler (instead of Diego). The Kubernetes scheduler is used in the CFEE to deploy and run apps in the Cloud Foundry application runtime, add users, create an organization and spaces, deploy apps, and bind those apps to services. See the [Eirini project](https://www.cloudfoundry.org/project-eirini/) documentation for more information about the Eirini incubator project from the Cloud Foundry Foundation.
 The following limitations apply to CFEE instances created from the Eirini Technical preview plan:
 
* Cloud Foundry SSH is not supported.
* `cf push` of Docker images is not supported.
* Kubernetes native Eirini staging is not supported. CFEE instances created from the Eirini technical preview plan use Eirini for running apps, but not for staging them. Diego cells are still used for staging apps.
* Restart of specific app instances (`cf restart-app-instance`) is not supported.
