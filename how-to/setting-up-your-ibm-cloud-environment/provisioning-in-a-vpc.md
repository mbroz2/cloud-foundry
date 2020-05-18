---

copyright:
  years: 2019
lastupdated: "2019-11-11"
---

{:note: .note}

# CFEE on IBM Virtual Private Cloud (VPC)
{: #cfee-on-vpc}

You can provision an IBM Cloud Foundry Enterprise Environment into an existing IBM Virtual Private Cloud (VPC).


VPC allows you to create your own space in the IBM Cloud, to run an isolated environment within the public cloud. VPC gives you the security of a private cloud, with the agility and ease of a public cloud. IBM Cloud Foundry Enterprise Environment fully utilizes this capability and allows you to host your applications with the highest level of network security and isolation possible.

If you are not familiar with IBM's Virtual Private Cloud offerings, you can learn more about them [here](https://www.ibm.com/cloud/vpc) and [here](https://cloud.ibm.com/docs/vpc-on-classic?topic=vpc-on-classic-about).

Follow the instructions [here](https://cloud.ibm.com/docs/vpc-on-classic?topic=vpc-on-classic-getting-started) to create your VPC.

## Prerequisites
{: #prereq}
Before provisioning CFEE into your VPC, ensure that you have the following:
1. You must have the same permissions as are required to [provision on classic infrastructure](https://cloud.ibm.com/docs/cloud-foundry?topic=cloud-foundry-permissions) (excluding permissions for VLANS or datacenters).
1. The targeted account must be configured so that, for IBM Kubernetes Service (IKS) provisioning, actions are performed as a user with Kubernetes Administrator and VPC viewer access. The access can be reset using the command `ibmcloud ks api-key reset`. [See documentation here.](https://cloud.ibm.com/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#api_key-commands)
1. To provision a CFEE in an isolated environment (whether VPC or on classic infrastructure) you must enable your account for [Cloud Service Endpoints](https://cloud.ibm.com/docs/resources?topic=resources-private-network-endpoints#cs_cli_install_steps) (CSE).

## VPC-specific Prerequisites and Capacity Requirements
{: #vpc-specific-prerequisites}
Before provisioning a CFEE into your VPC, please keep in mind:
1. There are some known limitations specific to VPCs.  These are listed [here](https://cloud.ibm.com/docs/vpc-on-classic?topic=vpc-on-classic-known-limitations).
1. You need to be aware of the [VPC-specific quotas](https://cloud.ibm.com/docs/infrastructure/vpc-on-classic?topic=vpc-on-classic-quotas) and ensure that you have sufficient capacity remaining.
1. Pay particular attention to [VPC-specific load balancer quotas](https://cloud.ibm.com/docs/infrastructure/vpc-on-classic?topic=vpc-on-classic-quotas#load-balancer-quotas).
1. You must have enough unallocated IPs in a VPC subnet before you provision a CFEE. The minimum number required is the number of cells per zone plus control plane nodes plus 10. E.g., **2 cells per zone + 2 control plane nodes + 10 = 14 IPs**.

   **Note:** A VPC subnet will typically have a small number of IPs allocated prior to any use. Check the available number of IPs in the subnet to be sure there are more than the number of IPs required for a VPC CFEE.

1. If a worker node deployment fails due to an IBM Kubernetes Service issue, you must redeploy the CFEE instance. This cannot be fixed without redeploying because CFEE instances are dependant on the IBM Kubernetes Service.

## VPC Network Connectivity
{: #vpc-network-connectivity}
All {{site.data.keyword.cfee_full_notm}} instances must allow specific inbound and outbound flows which are required for the service to function.  Because of the isolation capabilities provided by Virtual Private Clouds, {{site.data.keyword.cfee_full_notm}} wishing to provision into a VPC must pay particular attention to the network connectivity and configure it so that the {{site.data.keyword.cfee_full_notm}} instance can be provisioned correctly.

### Outbound Connectivity
All {{site.data.keyword.cfee_full_notm}} instances must be able to reach specific services (IBM's Identity and Access Management service, for example) to function.  The simplest way to enable this is to ensure that every public subnet in the VPC has a public gateway.  Users can find the list of options for enabling outbound Internet connectivity from within a VPC [here](https://cloud.ibm.com/docs/vpc-on-classic?topic=vpc-on-classic-about#internet-access).

**Note:** It is the user's responsibility, prior to beginning the {{site.data.keyword.cfee_full_notm}} provisioning process, to ensure that the required Internet connectivity is in place.  You can find more information on network isolation [here](cloud-foundry?topic=cloud-foundry-isolated-network).

### Inbound Connectivity
By default, all {{site.data.keyword.cfee_full_notm}} instances are reachable over the Internet via a public load balancer.  This load balancer allows traffic to reach both the Cloud Foundry administrative APIs and applications hosted within the {{site.data.keyword.cfee_full_notm}} instance.  To disallow
inbound traffic from the Internet and operate the {{site.data.keyword.cfee_full_notm}} instance in a fully isolated fashion, please see the VPC-specific [network isolation](#network-isolation) section below.

## Creating a VPC CFEE
{: #creating-vpc-cfee}
1. Create your VPC as [detailed here](https://cloud.ibm.com/docs/vpc-on-classic?topic=vpc-on-classic-getting-started).
1. Go to the CFEE [creation page](https://cloud.ibm.com/cfadmin/create).
1. Select VPC environment.
1. Choose your recently created VPC from the dropdown.
1. Choose the Worker Zones and Subnets you plan to use.
1. Continue to follow the instructions for creating a regular CFEE [here](cloud-foundry?topic=cloud-foundry-create-environment).

## Enabling Monitoring
{: #enable-monitoring}

With the CFEE 5.2 release, enabling monitoring on a VPC infrastructure is supported.
Monitoring on VPC has some limitations compared to monitoring on classic infrastructure as detailed in [Monitoring](cloud-foundry?topic=cloud-foundry-monitoring#MonitoringVPC).

## Limitations
{: #limitations}
There are several limitations detailed in the [release notes for v5.0.0](https://cloud.ibm.com/docs/cloud-foundry?topic=cloud-foundry-what-s-new-in-ibm-cloud-foundry-enterprise-environment#v500)

## Network Isolation for CFEE on VPC
{: #network-isolation}
The following steps describe the process for configuring a provisioned instance of {{site.data.keyword.cfee_full_notm}}
Implementing network isolation for a {{site.data.keyword.cfee_full_notm}} instance will remove all access from the public Internet.  This will apply to both the IBM Kubernetes Service cluster (e.g., running `kubectl` commands) and to Cloud Foundry (both CF APIs and hosted applications).

**Important:** Do _not_ perform these steps unless you have **both** control over DNS to allow you to take over resolution for your cluster ingress domain **and** network connectivity to the private network(s) within your VPC.  Proceeding without these prerequisites in place can render your CFEE inaccessible.
{: note}

To fully isolate a {{site.data.keyword.cfee_full_notm}} from Internet traffic, users must make several changes after provisioning the instance.  These steps are:
1. Disable the public Cloud Service Endpoint [for the IBM Kubernetes Service (IKS) cluster](#disable-iks-public-cse).
1. Disable the public Cloud Service Endpoint [for the IBM Cloud Databases for PostgreSQL instance](#disable-icd-postgres-public-cse).
1. Disable the public [Application Load Balancer](#disable-public-alb) (ALB) within the IKS cluster and update DNS so that the cluster's domain resolves to the private ALB.

## Disabling the IKS public CSE on a VPC CFEE
{: #disable-iks-public-cse}

**Important:** Disabling the public CSE will remove the access needed to run `kubectl` commands or otherwise access your kubernetes API server from the public Internet.  Do not do this unless you have the required network connectivity in place to allow access to the private subnets in your VPC.
{: note}

1. Log into the IBM Cloud CLI.
2. Find the cluster associated with your VPC CFEE. The format of the name of the cluster is `<CFEE name>-cluster`.

        ibmcloud ks clusters

3. Disable the public CSE for the cluster.  You can run the following command with the `-y -f` flags appended to squelch any prompts.  If you are prompted to refresh the kubernetes API server, you should respond with `y`.  The command may output a message indicating a need to reload or reboot the cluster worker nodes.  _This is not required when running your CFEE instance in a VPC_:

        ibmcloud ks cluster feature disable public-service-endpoint --cluster <CFEE name>-cluster

4. Check that the public CSE has been fully disabled.  This is an asynchronous operation and can take as long as twenty minutes.  Do not worry if it seems to be taking longer than normal.  You can check that it has been disabled by running `ibmcloud ks cluster get --cluster <cluster name>`.  You will know it has been fully disabled when the output shows something like (note the absence of a valid URL for the `Public Service Endpoint URL`):

        Public Service Endpoint URL:    -
        Private Service Endpoint URL:   https://host:port

## Disabling the public CSE for PostgreSQL:
{: #disable-icd-postgres-public-cse}
To disable the public CSE for the IBM Cloud Databases (ICD) for PostgreSQL instance associated with your {{site.data.keyword.cfee_full_notm}} instance, first identify which is the correct ICD instance in your account and then follow the directions [here](https://cloud.ibm.com/docs/databases-for-postgresql?topic=cloud-databases-service-endpoints#changing-service-endpoints).

## Disabling Cloud Foundry and Application via the public ALB
{: #disable-public-alb}

**Important:** Public access to both Cloud Foundry APIs and applications is disabled upon disabling the public ALB.  You will not be able to restore access without both network connectivity to the private VPC subnets and the DNS updates described below.
{: note}

There are two steps to disabling the public ALB.  First, disable the ALB itself.
1. Retrieve the ID of your public alb by running `ibmcloud ks alb ls --cluster <cluster name>`.  From the same command output, be sure to note the load balancer hostname for the private alb, which you will need later.
2. Disable the public alb: `ibmcloud ks alb configure vpc-classic --alb-id <alb id> --disable-deployment`.

Finally update the default DNS resolution for cluster's subdomain to point the private load balancer for your cluster: 

1. Find the subdomain for your cluster:

        ibmcloud ks nlb-dns ls --cluster <cluster name>
2. Find the private load balancer hostname for your clsuter:

        ibmcloud ks alb ls --cluster <cluster name>

3. Update the DNS resolution:

        ibmcloud ks nlb-dns replace --cluster <cluster name> --nlb-subdomain <cluster subdomain> --lb-host <cluster private load balancer hostname>

The above DNS update takes the form of a CNAME from your `<cluster subdomain>` to the `<cluster private load balancer hostname>`. This change can take up to 30 mins to be completely.  Use `dig <cluster subdomain>` to identify when the change is live.      

## Managing VPC CFEE Worker Nodes
{: #worker-nodes}
In the VPC Kubernetes cluster hosting your CFEE, managing the worker nodes is done differently from classic CFEE. [See VPC cluster limitations](https://cloud.ibm.com/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#vpc_ks_limits). In the event that one of the worker nodes goes down, you must replace the worker node:
1. Log into the IBM Cloud CLI.
2. Find the cluster associated with your CFEE. The name of the cluster is `<CFEE name>-cluster`.

        ibmcloud ks clusters

3. Identify the worker node that is failing:

        ibmcloud ks workers --cluster <cluster name>

4. Execute a worker replace on the failing worker node:

        ibmcloud ks worker replace --cluster <cluster name> --worker <worker ID>

    Please note the worker ID (attribute "id"). You will need provide this to CFEE support so they can accurately update your cluster metadata in the management plane

5. After all workers have been replaced and have finished provisioning, you will need to provide the worker IDs and IPs to CFEE support so they can accurately update your cluster metadata in the management plane.

    Give the output of this command and your CFEE environment ID to CFEE support:

        ic ks worker ls --cluster <cfee cluster name>

    Once the metadata has been updated, the pods running the Cloud Foundry components will use the new worker nodes.

        ibmcloud ks cluster config <cluster name>
        kubectl -n cf get pods
