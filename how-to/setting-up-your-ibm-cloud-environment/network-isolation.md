---

copyright:
  years: 2017, 2018
lastupdated: "2019-04-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Operating in an isolated network
{: #isolated-network}


Starting with version 2.2.0, {{site.data.keyword.cfee_full}} (CFEE) instances can operate inside an isolated network that protects and secures the environment from external threats. You create an isolated network through private VLANs and a set of control mechanisms to route, filter, and protect traffic into and out of the resources inside the network perimeter. Isolated networks are setup using technologies like Virtual Router Appliances ([VRAs](https://cloud.ibm.com/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-getting-started-with-ibm-virtual-router-appliance#vlans-and-the-gateway-appliance-s-role)).

**Note:** CFEE instances operating in an isolated network require that the CFEE Kubernetes cluster be updated to v1.13. Updating the Kubernetes cluster to v1.13 is independent of the CFEE update.

A CFEE instance is provisioned into a cluster from the IBM Kubernetes Service. The CFEE's Cloud Foundry cells (where applications are hosted) are provisioned into the Cluster's worker nodes. This means that access to the CFEE cells and applications implies accessing the cluster worker nodes. 

The worker nodes contain Application Loads Balancers (ALBs) that distribute the applications' workload among the worker nodes. CFEE v2.2.0 enables the Kubernetes ALBs in both, private and public networks.  This means that CFEE v2.2.0 can be accessed from both, public and private networks.

## Management in an isolated network
{: #control-plan-access}

In v2.2.0 the CFEE management plane (the sub-system used for provisioning, updates and other lifecycle activities) does not rely on the ALBs or worker nodes directly to manage a CFEE instance. Instead, the CFEE management plane uses the _VPN connection between the Kubernetes master and worker nodes_ to access the CFEE instance and its resources. This enables the management plane to control the CFEE instance.

After you create your CFEE instance, the management plane creates an [IAM service ID](https://cloud.ibm.com/docs/overview/whats-new?topic=overview-whatsnew#identity-and-access-management-application-authentication-feature) in your account that it gives to the IKS master node. The CFEE management plane uses this service ID to access the CFEE host workers through the master node and its VPN access for subsequent provisioning and management operations, including deploying the main Cloud Foundry component of CFEE.

The diagram below depicts a CFEE installed behind a VRA network perimeter.   
![Network isolation](images/CFEE_IsolatedNetwork.png  "Diagram describing how a CFEE works in an isolated network.")

## Controlling access to instances in an isolated network
{: #oppening-access-points}

CF apps running in a CFEE instance frequently need to reach outside the isolated network for various reasons. For example, an application may need to access a public RESTful service, or download packages from public registries, like a Docker image, NPM packages, Python modules, etc. Hence, you need to configure your firewall rules to enable your specific network access needs.

You can control inbound/outbound connectivity to/from an isolated network by configuring the rules for specific protocols, sources or target endpoints. You can create and configure these by using control mechanisms in the specific technology used to setup the network perimeter (e.g. VRA). However, the simplest way to control this is by creating [Calico network policies](https://docs.projectcalico.org/v3.4/reference/calicoctl/resources/globalnetworkpolicy) that define network rules specifically for the host Kubernetes cluster. Please see [creating and applying Calico network policies](https://cloud.ibm.com/docs/containers?topic=containers-network_policies#adding_network_policies) for step-by-step guidance.

Network access rules related to a network isolated CFEE can be of the following types:

* **Inbound traffic access:** When a Kubernetes cluster is installed, it automatically sets and configures a [default Calico policy](https://cloud.ibm.com/docs/containers/cs_network_policy.html#default_policy) that allows management access to the worker nodes (hosting your CFEE instance in this case).

<br>
**Default** Calico rules for accessing the **Kubernetes cluster** worker nodes:

    ```
    allow-node-port-dnat            1500    ibm.role == 'worker_public'                        
    allow-vrrp                      1600    ibm.role == 'worker_public'                        
    allow-icmp                      1700    ibm.role in { 'worker_public', 'master_public' }
    allow-bigfix-port               1900    ibm.role in { 'worker_public', 'master_public' }
    allow-sys-mgmt                  1950    ibm.role in { 'worker_public', 'master_public' }
    ```
    {: codeblock}
<br />
If you are using an alternative network perimeter implementation, you'll need to setup the same policies using the corresponding control mechanisms.
  
* **Outbound traffic access:** CFEE relies on a number of services in IBM Cloud with public endpoints. Your isolated network needs to allow your CFEE instance to reach these endpoints. These are listed below along with their respective Calico policies.

| Rule   | Description | Endpoint |
|-----------|---------------|---------------|
| cfee-egress-allow-adminserver | Allows outbound access to the CFEE management plane | 169.46.2.150/32, 169.47.104.126/32,     161.156.68.46/32, 130.198.90.238/32, 169.46.66.254/32, 169.46.186.113/32, 161.156.66.118/32, 130.198.90.238/32|
| cfee-egress-allow-docker | Allows outbound access to the Docker registries | `registry-1.docker.io`, `docker.io`, `production.cloudflare.docker.com`|
| cfee-egress-allow-iks | Allows outbound access to the CFEE Kubernetes cluster management plane | [Learn more](https://cloud.ibm.com/docs/containers?topic=containers-firewall#firewall)|
| cfee-egress-allow-internal  | Allows outbound access and routing to the CFEE's Kubernetes cluster portable IP addresses | You can find your cluster portable addresses using: `kubectl get cm -n kube-system ibm-cloud-provider-vlan-ip-config -o json` \`jq .data."reserved_public_ip"`, `kubectl get cm -n kube-system ibm-network-interfaces -o yaml`, `kubectl get svc --all-namespaces`|
| cfee-egress-allow-postgres | Allows outbound access to the CFEE's Compose for PostgreSQL database | You can find the endpoint from the Compose instace `<cfee-name>-postgress`'s dashboard page and in the CFEE's _Private Access_ page. |
| cfee-egress-allow-rc | Allows outbound access to the IBM Cloud platform to enable service syndication | `resource-controller.cloud.ibm.com` |
| cfee-egress-allow-registry | Allows outbound access to the IBM Cloud Container Registry | [Learn more](https://cloud.ibm.com/docs/containers?topic=containers-firewall#firewall) |
| cfee-egress-allow-logging | (optional) Allows outbound network traffic from the CFEE worker nodes to IBM Cloud Monitoring and IBM Cloud Log Analysis services | [Learn more](https://cloud.ibm.com/docs/containers?topic=containers-firewall#firewall_outbound) |
| cfee-egress-allow-helm-tiller | Allows outbound network traffic from the CFEE worker nodes to the helm image retistry| This calico rule is for IPs of the Helm tiller, which needs access to the `gcr.io` and `storage.googleapis.com` domains to pull the helm tiller image. |

<br>  
The final network security rule (the most **important** one) is the one that denies all access to network traffic that don't match the previous policies.
<br>
Calico rule for **denying all outbound access** (`cfee-egress-deny-all`):

```
apiVersion: projectcalico.org/v3
kind: GlobalNetworkPolicy
metadata:
  name: cfee-egress-deny-all
spec:
  applyOnForward: true
  egress:
  - action: Deny
    destination: {}
    source: {}
  order: 3000
  selector: ibm.role in { 'worker_public' }
  types:
- Egress
```
{: codeblock}

<br>
Calico rule for **denying all inbound access** (`cfee-ingress-deny-all')
`):

```
apiVersion: projectcalico.org/v3
kind: GlobalNetworkPolicy
metadata:
  name: cfee-ingress-deny-all
spec:
  applyOnForward: true
  ingress:
  - action: Deny
    destination: {}
    source: {}
  order: 4000
  selector: ibm.role=='worker_public'
  types:
- Ingress
```
{: codeblock}
<br>  
One complexity with outbound access from isolated networks is that some of the target endpoints are behind a _public edge_ network, like Cloudflare or Akamai. In this case you may need to potentially allow access to hundreds of IP addresses that change often. You can optionally avoid this situation by **allowing all outbound access** by using the following Calico rule (`cfee-egress-allow-all`):

```
apiVersion: projectcalico.org/v3
kind: GlobalNetworkPolicy
metadata:
   name: cfee-egress-allow-http
    spec:
   egress:
   - action: Allow
     destination:
       ports:
       - 443
       - 80
     protocol: TCP
     source: {}
   order: 1611
   selector: ibm.role in { 'worker_public' }
   applyOnForward: true
   types:
   - Egress
```
{: codeblock}
  
In the above example, all HTTP and HTTPS calls (all TCP traffic on ports 443 and 80) coming from the CFEE cluster worker nodes (“worker_public”) are allowed outbound (“egress”) access to the public network.

## Communication with supporting services
{: #private-access}

A CFEE instance continuously communicates with its supporting services instances: _Kubernetes cluster_, _Cloud Object Storage_ and _Databases for PostgreSQL_.  Users with _administrator_ role in the CFEE instance can see information about the endpoints used by the CFEE to access those supporting services. Go to the **Management access** in the overflow menu located in the upper right corner of the _Overview_ page in the CFEE user interface. The _Management access_ dialog shows the type of access (private or private) and the actual service endpoints use to access the supporting service instances.  The endpoints are not intended for development tasks, but only for troubleshooting purposes (e.g., to verify how CFEE accesses those service instances). 

By default, the CFEE management control plane accesses its _Kubernetes cluster_  via the cluster **master** (as opposed to through the cluster's ingress router). Access to the cluster master requires an **API key** provided and applied by default for a **Service ID** (also provided by default). A CFEE administrator can replace the default API key with a custom key.  To generate an API key, go to the IAM [Service ID](https://cloud.ibm.com/iam/serviceids) user interface, find the Service ID created for the CFEE cluster, open it, go to the **API keys** tab, and click **Create**. 

A CFEE administrator can also **disable** accessing the cluster via the master.  When access to the cluster via the master is disabled, access to the cluster takes place through the ingress router.  **Note** that when access to the cluster via the master is disabled, re-enabling it requires generating a new API key (see above).

In addition to communication to the _Kubernetes cluster_, the CFEE instance needs to communicate with the _Cloud Object Storage_ and _Databases for PostgreSQL_ instances to perform routine operations.  Communication with the _Cloud Object Storage_ service instance always takes place through a private access endpoint. Communication with the _Databases for PostgreSQL_ instance takes place through a private endpoint if the IBM Cloud account is enabled for Virtual Routing & Fowarding ([VRF](https://cloud.ibm.com/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)) and IBM [Cloud Service Endpoint](https://cloud.ibm.com/docs/service-endpoint?topic=service-endpoint-getting-started#getting-started). Otherwise, communication with  _Databases for PostgreSQL_ instance takes place through a public endpoint.  

**Note:** Changing the service endpoints of the _Cloud Object Storage_ and _Databases for PostgreSQL_ service instances used by CFEE would disrupt the normal operations of the CFEE.

