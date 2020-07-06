---

copyright:

  years: 2015, 2019, 2020

lastupdated: "2020-02-27"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}


# What's new in {{site.data.keyword.cfee_full_notm}}

<!-- Update topic to follow the [release notes guidance and template](https://test.cloud.ibm.com/docs/developing/writing/markdown?topic=writing-releasenotes). -->

This document describes what's new in each released version of the {{site.data.keyword.cfee_full}} (CFEE) service.



## Cloud Foundry Enterprise Deprecation
{: #deprecation}

The IBM Cloud Foundry Enterprise Environment single-tenant cloud product is being deprecated and withdrawn from the market.


### Details

IBM Cloud's strategic direction for isolated single-tenant PaaS is Red Hat OpenShift, a fully managed PaaS that leverages the enterprise scale and security of IBM Cloud, allowing users to focus on developing and managing their applications.

IBM Cloud continues to fully support the mission of the Cloud Foundry Foundation and the delivery of Cloud Foundry applications. [IBM Cloud Foundry Public Applications](https://cloud.ibm.com/docs/cloud-foundry-public?topic=cloud-foundry-public-getting-started) remains a key compute application deployment capability in IBM Cloud and will continue to be available.

### Key Dates
* End-of-marketing - April 14th, 2020
* Removal from the IBM catalog - May 15th, 2020
* End-of-support - July 15th, 2020

The last planned version will be 6.1.0. The product will have no additional regulatory compliance renewals between now and the end-of-life.


### References & Product Information

Some references and information about IBM PaaS products and application development with Kubernetes containers

**Redhat OpenShift on IBM Cloud**
* [OpenShift Getting Started on IBM Cloud](https://cloud.ibm.com/docs/openshift?topic=openshift-openshift_tutorial)
* [Documentation & Getting Started](https://cloud.ibm.com/docs/openshift?topic=openshift-openshift_tutorial) on OpenShift on IBM Cloud 
* [Guided Tour](https://developer.ibm.com/technologies/containers/videos/red-hat-openshift-on-ibm-cloud-video/) to Red Hat OpenShift on IBM Cloud
* Red Hat OpenShift Cluster [deployment](https://cloud.ibm.com/kubernetes/catalog/create?platformType=openshift)


**Application creation in Kubernetes containers**
* [Multicluster Kubernetes Strategies to Kubernet-ify Apps](https://www.youtube.com/watch?v=Zq1VNXQOdGI)
* [Strategies to "Kubernetify" Legacy Applications](https://www.youtube.com/watch?v=mMk6hvgv-NI)
* Migrating an app from Cloud Foundry to a Kubernetes Cluster [Tutorial Walk-through](https://cloud.ibm.com/docs/containers?topic=containers-cf_tutorial) => This walks through the differences between a CF app and a Kubernetes app – it has step-by-step instructions and sample applications. Several large IBM Cloud customers have used this reference recently to successfully learn how to deploy CF apps to Kubernetes containers


**Questions about OpenShift on IBM Cloud**
* If you have questions, engage our team via Slack. You can register here (https://cloud.ibm.com/kubernetes/slack) and join the discussion in the #cfee-to-ocp channel on (https://ibm-cloud-success.slack.com).

<br/>

---

---

<br/>


The {{site.data.keyword.cfee_full_notm}} service packages specific versions of Cloud Foundry and buildpacks, and it is tested for function using specific versions of the Cloud Foundry CLI.  Where these versions do not change between releases, we typically do not list them explicitly.  Users can assume that the most recently listed versions for a given release are still valid for subsequent {{site.data.keyword.cfee_full_notm}} releases unless otherwise specified.

## Version 6.1.1
{: #611}

_Release Date:_ 2020-07-07

### New Capabilities
* {{site.data.keyword.cfee_full_notm}} v6.1.1 now supports IKS 1.16.10

## Version 6.1.0
{: #610}

_Release Date:_ 2020-04-16

### New Capabilities

* {{site.data.keyword.cfee_full_notm}} v6.1.0 supports updating kubernetes when deployed into a VPC

## Version 6.0.0
{: #600}

_Release Date:_ 2020-02-27

### New Capabilities

The following new capabilities were introduced in {{site.data.keyword.cfee_full_notm}} v6.0.0:
* [Tooling](/docs/cloud-foundry?topic=cloud-foundry-monitoring) within the monitoring toolset has been renamed from "Application Deployment Validation" to "Cloud Foundry Api Tester". Metrics produced by this tooling have been renamed from `cmlt_` to `cfapitester_`. 
* [Sample Sysdig Dashboards](https://github.com/cfibmers/CFEE-dashboards) are provided for the "Cloud Foundry Api Tester".

### Issues resolved

The following issues were resolved in {{site.data.keyword.cfee_full_notm}} v6.0.0:
* [CVE-2019-17596](https://www.cloudfoundry.org/blog/cve-2019-17596/)
* [CVE-2019-3789](https://www.cloudfoundry.org/blog/cve-2019-3789/)

### Packaged Software Versions and Corequisites

* {{site.data.keyword.cfee_full_notm}} v6.0.0 updates Cloud Foundry to cf-deployment v12.17.0.  Please see the [Cloud Foundry Release Notes](https://github.com/cloudfoundry/cf-deployment/releases?after=v12.18.0) for information on changes in Cloud Foundry.  As CFEE vendors Cloud Foundry from SUSE SCF, see also the relevant [SCF Release Notes](https://github.com/SUSE/scf/releases?after=2.21.0)
* {{site.data.keyword.cfee_full_notm}} v6.0.0 updates Monitoring tools as follow:
  * Prometheus v2.13.1
  * Grafana v6.5.0
  * Alertmanager v0.18.0
  * blackbox-exporter v0.16.0
  * cf-exporter v0.10.0
  * firehose-exporter v6.0.0
  * k8s-sidecar v0.0.17
  * postgres-exporter v0.5.1

Pre-exisiting platform logging configurations will stop functioning after updating to {{site.data.keyword.cfee_full_notm}} v6.0.0. Please disable and re-enable platform logging after the update to continue receiving CF component logs.
{: note}

## Version 5.2.2
{: #522}

_Release Date:_ 2020-02-06

### Issues Resolved

The following issues were resolved in {{site.data.keyword.cfee_full_notm}} v5.2.2:
* Some mTLS configurations were broken due to a bug in the parsing of certificate subjects.  New client certificate configurations for custom domains are now validated using a certificate's SHA fingerprint.  Existing client certificate configurations are not affected.

## Version 5.2.1
{: #v521}

_Release Date:_ 2020-01-10

### New Capabilities

The following new capabilities were introduced in {{site.data.keyword.cfee_full_notm}} v5.2.1:
* Bug fixes to address the disablement of monitoring and the inability to scale an newly created environment


The update to v5.2.1 is available only to {{site.data.keyword.cfee_full_notm}} v.5.2.0, v.5.1.0, v.5.0.0, v4.0.0, and v3.2.2 instances, See [Version 5.2.0](#v520),[Version 5.1.0](#v510),[Version 5.0.0](#v500),[Version 4.0.0](#v400), and
[Version 3.2.2](#v322).
{: note}

Prior to any scale-up operation on a new 5.2 classic cfee that has been updated to 5.2.1, remove the bcf.type label from the 5.2.1 classic cell worker pool via the Kubenetes UI. Instances updating from a pre-5.2 classic cfee are not impacted.
{: note}

## Version 5.2.0
{: #v520}

_Release Date:_ 2019-12-03

### New Capabilities

The following new capabilities were introduced in {{site.data.keyword.cfee_full_notm}} v5.2.0:
* Support for scaling instances provisioned into VPC on Classic
* Support for enabling monitoring in instances provisioned into VPC on Classic
* Support for enabling Sysdig in instances with monitoring enabled (both VPC and non-VPC instances).  Once a user has enabled monitoring, they can refer to the [{{site.data.keyword.cfee_short}} Monitoring documentation](/docs/cloud-foundry?topic=cloud-foundry-monitoring#sysdig) to deploy Sysdig into the {{site.data.keyword.cfee_short}} instance.  At this point the user can choose to use either Prometheus or Sysdig.
* Support for sending Cloud Foundry platform logs to LogDNA

Prior to any scale-up operation on a new 5.2 classic cfee, remove the bcf.type label from the 5.2 classic cell worker pool via the Kubenetes UI.
{: note}

The update to v5.2.0 is available only to {{site.data.keyword.cfee_full_notm}} v.5.1.0, v.5.0.0, v4.0.0, and v3.2.2 instances, See [Version 5.1.0](#v510),[Version 5.0.0](#v500),[Version 4.0.0](#v400), and
[Version 3.2.2](#v322).
{: note}

## Version 5.1.0
{: #v510}

_Release Date:_ 2019-11-11

### New Capabilities

The following new capabilities were introduced in {{site.data.keyword.cfee_full_notm}} v5.1.0:
* Support for updating instances provisioned into VPC on Classic

### Issues Resolved

The following issues were resolved in {{site.data.keyword.cfee_full_notm}} v5.1.0:
* Enabling monitoring was temporarily impacted by a change to the underlying infrastructure layer.  This change, which was introduced on 2019-11-05, prevented monitoring enablement from working for all versions of {{site.data.keyword.cfee_full_notm}} prior to v5.1.0.  Users wishing to enable monitoring _must_ update to v5.1.0.  Users who had enabled monitoring prior to 2019-11-05 were not impacted.
* Using a custom UAA identity provider did not work unless only a single UAA pod was running.
* Some options for log forwarding were being handled incorrectly.
* Updating either a classic or VPC instance could result in the deletion of the IBM Cloud Activity Tracker with LogDNA event forwarder pods, thereby breaking the flow of events to the Activity Tracker instance.

The update to v5.1.0 is available only to {{site.data.keyword.cfee_full_notm}} v.5.0.0, v4.0.0, and v3.2.2 instances, See [Version 5.0.0](#v500),[Version 4.0.0](#v400), and
[Version 3.2.2](#v322).
{: note}

## Version 5.0.0
{: #v500}

_Release Date:_ 2019-10-28

### New Capabilities

The following new capabilities were introduced in {{site.data.keyword.cfee_full_notm}} v5.0.0:
* Support for provisioning instances into [VPC on classic](https://cloud.ibm.com/docs/vpc-on-classic?topic=vpc-on-classic-getting-started)
* Integration with [IBM Cloud Activity Tracker with LogDNA](https://cloud.ibm.com/catalog/services/ibm-cloud-activity-tracker-with-logdna)
* [Additional tooling](/docs/cloud-foundry?topic=cloud-foundry-monitoring) within the monitoring toolset enables continuous testing of important CF APIs.

You can now provision your {{site.data.keyword.cfee_full_notm}} in a Virtual Private Cloud (VPC).  Instructions can be found [here](cloud-foundry?topic=cloud-foundry-cfee-on-vpc)

### Limitations

The following limitations exist for a {{site.data.keyword.cfee_full_notm}} instance in VPC:
* Scaling in CFEE v5.0 is currently not supported. The ability to scale your CFEE will be available at a later date.
* Monitoring in CFEE v5.0 is currently not supported. The ability to enable and disable monitoring will be available at a later date.

The following limitations exist for integration with IBM Cloud Activity Tracker with LogDNA:
* Activity Tracker integration is currently only available in the Dallas, London, Frankfurt, and Sydney regions.  This integration will be available in the remaining regions (Tokyo and Washington, D.C.) at a later date.


The update to v5.0.0 is available only to {{site.data.keyword.cfee_full_notm}} v4.0.0 and v3.2.2 instances, See [Version 4.0.0](#v400) and
[Version 3.2.2](#v322).
{: note}

### Packaged Software Versions and Corequisites

* {{site.data.keyword.cfee_full_notm}} v5.0.0 updates Cloud Foundry to cf-deployment v7.11.0.  Please see the [Cloud Foundry Release Notes](https://github.com/cloudfoundry/cf-deployment/releases?after=v8.0.0) for information on changes in Cloud Foundry.  As CFEE vendors Cloud Foundry from SUSE SCF, see also the relevant [SCF Release Notes](https://github.com/SUSE/scf/releases?after=2.18.0)
* {{site.data.keyword.cfee_full_notm}} v5.0.0 includes the following Cloud Foundry buildpacks:
  * liberty-for-java: `buildpack_liberty-for-java_v3.36-20190905-1704.zip`
  * sdk-for-nodejs: `buildpack_sdk-for-nodejs_v4.0.1-20190930-1425.zip`
  * dotnet-core: `buildpack_dotnet-core_v2.4-20190912-1554.zip`
  * swift_buildpack: `buildpack_swift_buildpack_cflinuxfs3_v2.1.0-20190404-1206.zip`
  * xpages_buildpack: `buildpack_xpages_buildpack_v1.2.2.zip`
  * staticfile_buildpack: `staticfile-buildpack-cflinuxfs3-v1.4.35.zip`
  * java_buildpack: `java-buildpack-cflinuxfs3-v4.16.1.zip`
  * ruby_buildpack: `ruby_buildpack-cflinuxfs3-v1.7.42.zip`
  * nodejs_buildpack: `nodejs-buildpack-cflinuxfs3-v1.6.34.zip`
  * go_buildpack: `go-buildpack-cflinuxfs3-v1.8.29.zip`
  * python_buildpack: `python-buildpack-cflinuxfs3-v1.6.23.zip`
  * php_buildpack: `php-buildpack-cflinuxfs3-v4.3.64.zip`
  * binary_buildpack: `binary-buildpack-cflinuxfs3-v1.0.27.zip`
* {{site.data.keyword.cfee_full_notm}} v5.0.0 has been tested using [version 6.46.1](https://github.com/cloudfoundry/cli/releases/tag/v6.46.1) of the Cloud Foundry CLI.


## Version 4.0.0
{: #v400}

_Release Date:_ 2019-09-24

The following changes were released in version 4.0.0 of the {{site.data.keyword.cfee_full_notm}} service:
* {{site.data.keyword.cfee_full_notm}} instances can now be created using 8x32 cells (8 vCPUs, 32 GB RAM).  Note that cell sizes cannot be mixed
within a {{site.data.keyword.cfee_full_notm}} instance - all cells must be the same size.  Once an instance has been created using a given cell
size, this cannot be changed later.  To change cell size, you must create a new CFEE instance with the desired cell size and move your applications
over.
* {{site.data.keyword.cfee_full_notm}} can now contain up to 16 cells per zone.  In a multi-zone region containing three zones, this means that
a {{site.data.keyword.cfee_full_notm}} instance can contain as many as 48 cells distributed evenly across the three zones.
**Important** There is a known issue when scaling a {{site.data.keyword.cfee_full_notm}} instance down.  If you scale down the instance by a sufficiently large quantity of cells, you _may_ encounter a condition wherein some of the cells become unusable (and metrics
  cannot be reported for these cells).  To avoid this,
_please scale your instance down in smaller increments of no greater than 3 cells/zone_.  If you need to scale down by more than three cells,
perform this as multiple separate scale-down actions to avoid this condition.  Should you nonetheless encounter this condition, please contact
support for assistance.  A fix for this is being worked on and will be released as soon as possible.
* The per-application disk quota has been raised from 2 GB to 4 GB.
* The CF cloud controller timeout has been increased from 180 seconds (three minutes) to 600 seconds (ten minutes).  This limit was raised to avoid
timeouts when pushing very large applications.
* Cloud Foundry has been updated to cf-deployment 6.10.  Please see the [Cloud Foundry Release Notes](https://github.com/cloudfoundry/cf-deployment/releases?after=v7.0.0) for information on changes in Cloud Foundry.  As CFEE vendors Cloud Foundry from SUSE SCF, see also the relevant [SCF Release Notes](https://github.com/SUSE/scf/releases?after=2.17.0).

To update your {{site.data.keyword.cfee_full_notm}} instance to this version, go to the _Updates and Scaling_ page in the {{site.data.keyword.cfee_full_notm}} user interface and click **Update**.

The update to 4.0.0 can be disruptive to Cloud Foundry management operations - this means that, during the update, `cf login` and other
cf commands may fail.  _As with all {{site.data.keyword.cfee_full_notm}} updates, however, your running applications are not
impacted as long as there is sufficient whitespace to allow them to be rebalanced during the update._  Following the update, operations
can be performed via the CF CLI.
{: note}

* **Important** After applying this update to your {{site.data.keyword.cfee_full_notm}} instance, you may find that,
for a period of time (some number of hours), the UI healthchecks fail, instance metrics may be unavailable, and some UI management actions may fail.
_Your instance is still functional and your applications will continue to run._  In addition, you can perform actions
(e.g. creating orgs, pushing apps) via the CF CLI during this time.  After a period of time the metrics and healthchecks will return
to a normal healthy state without intervention.
* **Important** If you want, you can run a command via the `kubectl` CLI to restart one of the {{site.data.keyword.cfee_full_notm}}
components and fix the metrics/healthchecks manually.  You can do this at any time after the update to restore normal UI capabilities.
The specific command you run depends on whether or not you have enabled monitoring for your instance.  To begin, first login via the
IBM Cloud CLI and retrieve the kubernetes cluster configuration (the name of your cluster is `$INSTANCE_NAME-cluster`):
```
$ ibmcloud login
$ $(ibmcloud ks cluster config --cluster <YOUR_CLUSTER_NAME> --export)
```
What command you run next depends on whether or not your have enabled monitoring for your {{site.data.keyword.cfee_full_notm}} instance.
* If you have _not_ enabled monitoring: `kubectl -n cf-admin-agent set env deploy/cf-admin-exporters RESTART_TIME="$(date)"`
* If you _have_ enabled monitoring: `kubectl -n monitoring set env deploy/cf-exporter RESTART_TIME="$(date)"`


The update to v4.0.0 is available only to {{site.data.keyword.cfee_full_notm}} v3.2.2 and v3.1.1 instances, See [Version 3.2.2](#v322) and
[Version 3.1.1](#v311).
{: note}

## Version 3.2.2
{: #v322}

_Release Date:_ 2019-09-04

Version 3.2.2 of the {{site.data.keyword.cfee_full_notm}} service resolves the following:

* Issue where UAA wasn't reporting proper version information.
* Issue with UAA and the redirect endpoint.

To update your {{site.data.keyword.cfee_full_notm}} instance to this version, go to the _Updates and Scaling_ page in the {{site.data.keyword.cfee_full_notm}} user interface and click **Update**.

## Version 3.2.1
{: #v321}

_Release Date:_ 2019-08-26

Version 3.2.1 of the {{site.data.keyword.cfee_full_notm}} service resolves the following:

* Errors encountered when upgrading to version 3.2 from earlier versions of {{site.data.keyword.cfee_full_notm}}.

To update your {{site.data.keyword.cfee_full_notm}} instance to this version, go to the _Updates and Scaling_ page in the {{site.data.keyword.cfee_full_notm}} user interface and click **Update**.

## Version 3.2
{: #v320}

_Release Date:_ 2019-08-19

To update your {{site.data.keyword.cfee_full_notm}} instance to this version, go to the _Updates and Scaling_ page in the {{site.data.keyword.cfee_full_notm}} user interface and click **Update**.

As of version 3.2.0, you can provision {{site.data.keyword.cfee_full_notm}} instances in Chennai, India.

{{site.data.keyword.cfee_full_notm}} v3.2.0 updates the following versions of its constituent components:
* {{site.data.keyword.containerlong_notm}}: Kubernetes 1.14.  Applying the 3.2.0 update to your instance will update your cluster to Kubernetes 1.14 automatically.

The following changes were released in version 3.2.0 of the {{site.data.keyword.cfee_full_notm}} service:
* An additional control plane node size is now available.  As of 3.2.0 you can now provision an {{site.data.keyword.cfee_full_notm}} instance with a larger machine type of size 8x32 for the control plane.  All cell nodes will remain at the previous size of 4x16.  We highly recommend that you use larger control plane nodes for high volume and network intensive workloads.
* The restriction on cell count of 4 cells per control plane node has been lifted.  You can now provision an {{site.data.keyword.cfee_full_notm}} instance with up to 16 cells per control plane node of type 4x16.
* Enhancements have been made to the monitoring toolset. [Access](/docs/cloud-foundry?topic=cloud-foundry-monitoring#access-the-kubernetes-cluster) to Prometheus, Prometheus AlertManager and Grafana no longer require Kubernetes port forwarding. [Federation of the Prometheus server](/docs/cloud-foundry?topic=cloud-foundry-monitoring#federation) is enabled. Additional Prometheus alerts have been added for situations including [unhealthy Diego cells](/docs/cloud-foundry?topic=cloud-foundry-cf_debug#unh_cell_debug) and [excessive database connections](/docs/cloud-foundry?topic=cloud-foundry-cf_debug#db_con_debug).
* Applying an {{site.data.keyword.cfee_full_notm}} update will now, if applicable, automatically update the Kubernetes version of the underlying cluster.  No separate user action will be required in this case.

## Version 3.1.1
{: #v311}

_Release Date:_ 2019-07-01

Version 3.1.1 of the {{site.data.keyword.cfee_full_notm}} service resolves the following:

* Problem accessing the Stratos Console when installed on the {{site.data.keyword.cfee_full_notm}} Kubernetes cluster.

To update your {{site.data.keyword.cfee_full_notm}} instance to this version, go to the _Updates and Scaling_ page in the {{site.data.keyword.cfee_full_notm}} user interface and click **Update**.


## Version 3.1.0
{: #v310}

_Release Date:_ 2019-06-21

To update your {{site.data.keyword.cfee_full_notm}} instance to this version, go to the _Updates and Scaling_ page in the {{site.data.keyword.cfee_full_notm}} user interface and click **Update**.
The following changes were released in version 3.1.0 of the {{site.data.keyword.cfee_full_notm}} service:
* {{site.data.keyword.cfee_full_notm}} instances can now work on private-only access endpoints to communicate with its supporting services (i.e., without using any public service endpoints).  Use of private-only access endpoints requires enabling {{site.data.keyword.cfee_full_notm}} access to the Kubernetes access through the Kuberenetes cluster master. To enable {{site.data.keyword.cfee_full_notm}} access of the Kubernetes cluster through its cluster master, go to the _Overview_ page of the {{site.data.keyword.cfee_full_notm}} user interface, invoke **Management access** from the menu in the top right, and click **Enable**. Note that use of access endpoints in {{site.data.keyword.cfee_full_notm}} requires that the {{site.data.keyword.cloud_notm}} account is enabled for Virtual Routing & Fowarding ([VRF](https://test.cloud.ibm.com/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)) and [{{site.data.keyword.cloud_notm}} Service Endpoint](https://cloud.ibm.com/docs/service-endpoint?topic=service-endpoint-getting-started#getting-started). See [{{site.data.keyword.cfee_full_notm}} communication with supporting services](https://cloud.ibm.com/docs/cloud-foundry?topic=cloud-foundry-isolated-network#private-access) for more information.
* Log persistence in {{site.data.keyword.cfee_full_notm}} instances is enabled through integration with the LogDNA service, replacing the deprecated Log Analysis service.
* Client certificates can be whitelisted along with the Certicate Authority (CA) certificates required to authenticate them. This provides customers a convenient way to create mutual Transport Layer Security (TLS) authentication, scoping the authentication to specific custom domains. This enables customers more granular control over client access to their Cloud Foundry applications. See [Managing domains](https://cloud.ibm.com/docs/cloud-foundry?topic=cloud-foundry-domains#upload-certificates) for more information.
* Updating to a new {{site.data.keyword.cfee_full_notm}} version is disabled while an administration operation is in-progress (e.g., scaling, auditing or logging enablement).

Update to v3.1.0 is available only to {{site.data.keyword.cfee_full_notm}} v3.0.x instances, See [Version 3.0.0](#v300).
{: note}


## Version 3.0.2
{: #v302}

_Release Date:_ 2019-05-30

Version 3.0.2 of the {{site.data.keyword.cfee_full_notm}} service resolves the following:
* Problem provisioning {{site.data.keyword.cfee_full_notm}} instances of the Eirini Techncal Preview plan.
* No security group was defined in the private network connecting {{site.data.keyword.cfee_full_notm}} components, which prevented communication between applications deployed in an {{site.data.keyword.cfee_full_notm}} instance. This is resolved for new provisions of 3.0.2.  Updating from 3.0.1 will be addressed in a future release.

To update your version, go to the _Updates and Scaling_ page in the {{site.data.keyword.cfee_full_notm}} user interface.

Update to v3.0.2 is available only to {{site.data.keyword.cfee_full_notm}} v3.0.x instances, See [Version 3.0.0](#v300).
{: note}


## Version 3.0.1
{: #v301}

_Release Date:_ 2019-05-22

Version 3.0.1 of the {{site.data.keyword.cfee_full_notm}} service resolves the following:
* Problem resolving DNS hostnames correctly after enabling communication between {{site.data.keyword.cfee_full_notm}} and its Kubernetes cluster through the cluster master.
* Problem provisioning {{site.data.keyword.cfee_full_notm}} instances in the London region.

To update your version, go to the _Updates and Scaling_ page in the {{site.data.keyword.cfee_full_notm}} user interface.

Update to v3.0.1 is only available to {{site.data.keyword.cfee_full_notm}} v3.0.0 instances, See [Version 3.0.0](#v300).
{: note}


## Version 3.0.0
{: #v300}

_Release Date:_ 2019-05-13

Existing {{site.data.keyword.cfee_full_notm}} instances from version 2.x or earlier cannot migrate to version 3.0.0.
{{site.data.keyword.cfee_full_notm}} version 3.0.0 is only available in new {{site.data.keyword.cfee_full_notm}} instances. This is due to a change in the database service used by {{site.data.keyword.cfee_full_notm}} to store Cloud Foundry data. {{site.data.keyword.cfee_full_notm}} v3.0.0 uses {{site.data.keyword.databases-for-postgresql_full}}, instead of the deprecated Compose for PostgreSQL service used in previous versions.
{: important}

Note that new {{site.data.keyword.cfee_full_notm}} instances (v3.0.0) cannot be created at present in the Chennai region (Asia Pacific). Deployments in the Chennai region will be enabled at a later time.

{{site.data.keyword.cfee_full_notm}} v3.0.0 includes the following versions of its constituent components:
* Cloud Foundry: [3.6.0](https://github.com/cloudfoundry/cf-deployment/releases/tag/v3.6.0)
* Cloud Foundry API: [2.120.0](http://apidocs.cloudfoundry.org/3.6.0/)
* {{site.data.keyword.containerlong_notm}}: No version update provided.  **Important:** We recomment that you update the {{site.data.keyword.cfee_full_notm}} Kubernetes cluster to v1.13 before updating to {{site.data.keyword.cfee_full_notm}} v2.2.0 (required when the {{site.data.keyword.cfee_full_notm}} instance operates in an isolated network).
* {{site.data.keyword.cfee_full_notm}} buildpacks: v3.0.0 introduces support for the **_CFLinuxFS3_** Stack. The default stack for IBM buildpacks is cflinuxfs3 since they do not specify a stack. Community buildpacks have new version based on the _cflinuxfs3_ Cloud Foundry stack.
   The following buildpacks and their versions are including in v3.0.0:
    * Liberty for Java: jv3.30-20190325-1301
    * SDK for Nod.js: v3.26-20190313-1440
    * .Net Core: v2.2-20190327-1013
    * Swift: v2.1.0-20190404-1206
    * Xpages: v1.2.2
    * Staticfile: v1.4.35
    * Java: v4.16.1
    * Ruby: v1.7.27
    * Node.js: v1.6.34
    * Go: v1.8.29
    * Python: v1.6.23
    * PHP: v4.3.64
    * Binary: v1.0.27

The following changes were released in version 3.0.0 of the {{site.data.keyword.cfee_full_notm}} service:
* The database used by {{site.data.keyword.cfee_full_notm}} to store Cloud Foundry data is no longer Compose for PosgreSQL, but {{site.data.keyword.databases-for-postgresql_full}}. Resulting from this database change:
    * Existing {{site.data.keyword.cfee_full_notm}} instances with versions previous to 3.0.0 (v2.x or v1.x) cannot be updated to version 3.0.0.
    * Creating a new {{site.data.keyword.cfee_full_notm}} instance no longer requires developer access to a Cloud Coundry organization and space in the public {{site.data.keyword.Bluemix_notm}}.
* More secured communication between the {{site.data.keyword.cfee_full_notm}} instance and the supporting services (_Kubernetes cluster_, _Cloud Object Storage_ and _Databases for PostgreSQL_). Communication with the _Kubernetes cluster_ takes place by default through the cluster master (instead of through the ingres router). Communication with the _Cloud Object Storage_ service instance always takes place through a private access endpoint. Communication with the _Databases for PostgreSQL_ instance takes place through a private endpoint if the {{site.data.keyword.cloud_notm}} account is enabled for Virtual Routing & Fowarding ([VRF](https://cloud.ibm.com/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)) and IBM [Cloud Service Endpoint](https://cloud.ibm.com/docs/service-endpoint?topic=service-endpoint-getting-started#getting-started). Otherwise, communication with  _Databases for PostgreSQL_ instance takes place through a public endpoint.
* Synchronizing the local {{site.data.keyword.cfee_full_notm}} catalog with the {{site.data.keyword.cloud_notm}} catalog takes place automatically every 24 hours. Synchornization can be done manually within the UI of an {{site.data.keyword.cfee_full_notm}} space page.  In version 3.0.0 that manual synchronization can also be done more easily in the overflow menu of the _Overview_ page (in the {{site.data.keyword.cfee_full_notm}} user interface, click **Synchronize catalog** in the overflow menu located in the upper right corner of the _Overview_ page).
* **Auditing** and **Logging** capabilities in an {{site.data.keyword.cfee_full_notm}} are supported through the _Activity Tracker_ and the _Log Analysis_ services respectively.  Those two services are being deprecated from the {{site.data.keyword.Bluemix}} catalog.  Although you will still be able to use existing instances of those two services for auditing and logging in an {{site.data.keyword.cfee_full_notm}}, you will not able to create new instances of those services.
* New documentation is available with [managing](https://cloud.ibm.com/docs/cloud-foundry?topic=cloud-foundry-management-enablement) and [troubleshooting](https://cloud.ibm.com/docs/cloud-foundry?topic=cloud-foundry-debug_overview) guidance.


## Version 2.2.4
{: #v224}

_Release Date:_ 2019-04-25

Version 2.2.4 of the {{site.data.keyword.cfee_full_notm}} service resolves a problem that prevented users from creating an {{site.data.keyword.cfee_full_notm}} instance of the Eirini Technical Preview plan.  Existing {{site.data.keyword.cfee_full_notm}} instances do **not need to be updated** to this version.


## Version 2.2.3
{: #v223}

_Release Date:_ 2019-04-23

The following changes were released in version 2.2.3 of the {{site.data.keyword.cfee_full_notm}} service:

* Resolved problem in the user interface that made metrics unretrievable on environments without monitoring enabled.

To update your {{site.data.keyword.cfee_full_notm}} version, go to the _Updates and Scaling_ page in the {{site.data.keyword.cfee_full_notm}} user interface. This version pre-requires v2.1.0.

This version does not pre-require version 2.2.0. You can update directly to version 2.2.3. However, if you bypass version 2.2.0, we still recommend that you update the {{site.data.keyword.cfee_full_notm}} Kubernetes cluster to v1.13 before the update (recommended also before a v2.2.0 update).
{: note}


## Version 2.2.2
{: #v222}

_Release Date:_ 2019-04-16

The following changes were released in version 2.2.2 of the {{site.data.keyword.cfee_full_notm}} service:

* Resolved v2.2.1 update problems. Version 2.2.2 di facto replaces version 2.2.1.
* Resolved problem affecting specific use cases of `cf login`.
* Resolved minor problem with deployment of monitoring tools.

To update your {{site.data.keyword.cfee_full_notm}} version, go to the _Updates and Scaling_ page in the {{site.data.keyword.cfee_full_notm}} user interface.

This version does not pre-require version 2.2.0. You can update directly to version 2.2.2. However, if you bypass version 2.2.0, we still recommend that you update the {{site.data.keyword.cfee_full_notm}} Kubernetes cluster to v1.13 before the update (recommended also before a v2.2.0 update).
{: note}


## Version 2.2.1
{: #v221}

_Release Date:_ 2019-04-04

The following changes were released in version 2.2.1 of the {{site.data.keyword.cfee_full_notm}} service:

* Resolved a number of minor problems interfering with routine operations of an {{site.data.keyword.cfee_full_notm}} instance:
     * Misattributed error messages.
     * Incorrect buildpack upload timeout.
     * Unintended events on doble click delete icon.
     * Incorrect cluster version display.
     * Incorrect getting started page display.

To update your version, go to the _Updates and Scaling_ page in the {{site.data.keyword.cfee_full_notm}} user interface.

This version does not pre-require version 2.2.0. You can update directly to version 2.2.1. However, if you bypass version 2.2.0, we still recommend that you update the {{site.data.keyword.cfee_full_notm}} Kubernetes cluster to v1.13 before the update (recommended also before a v2.2.0 update).
{: note}


## Version 2.2.0
{: #v220}

_Release Date:_ 2019-04-01

{{site.data.keyword.cfee_full_notm}} v2.2.0 includes the following versions of its constituent components:
* Cloud Foundry: v2.7.1.15.5
* Cloud Foundry API: v2.115.0
* {{site.data.keyword.cfee_full_notm}} buildpacks: [v0.1.5](https://github.com/cloudfoundry/apt-buildpack/releases/tag/v0.1.5)
* {{site.data.keyword.containerlong_notm}}: No version update provided.  **Important:** We recommend that you update the {{site.data.keyword.cfee_full_notm}} Kubernetes cluster to v1.13 before updating to {{site.data.keyword.cfee_full_notm}} v2.2.0 (required when the {{site.data.keyword.cfee_full_notm}} instance operates in an isolated network).

The following changes were released in version 2.2.0 of the {{site.data.keyword.cfee_full_notm}} service. To update your version, go to the **Updates and Scaling** page in the {{site.data.keyword.cfee_full_notm}} user interface and click **Update**:

* New option to **create** an {{site.data.keyword.cfee_full_notm}} instance in a **multi-zone** Kubernetes cluster. Instances created on a multi-zone cluster distribute application instances, not only across worker nodes within a data center, but also across data centers, making your applications more resilient against infrastructure outages. Furthermore, multi-zone {{site.data.keyword.cfee_full_notm}}s can be **scaled** by  adding additional cells, or by removing existing ones from the current zones.  Note that you cannot add or remove zones from a multi-zone {{site.data.keyword.cfee_full_notm}} after the instance is created.
* {{site.data.keyword.cfee_full_notm}} v2.2.0 instances can now [operate within an **isolated network**](https://cloud.ibm.com/docs/cloud-foundry?topic=cloud-foundry-isolated-network#isolated-network). Note that if the instance is deployed in VLANs from an isolated network, some endpoints (IP addresses) [must be identified and routed properly](https://cloud.ibm.com/docs/cloud-foundry?topic=cloud-foundry-isolated-network#oppening-access-points) for the instance to become operational. An instance requires a Kubernetes cluster v1.13 to operate in an isolated network.
* New capability to bind applications (deployed to an {{site.data.keyword.cfee_full_notm}} space) to service instances through the internal IBM network. The capability allows services supporting the IBM Cloud Service Endpoint to be consumed without accessing the public internet.
* **Monitoring** tools:
    * The monitoring tools (Prometheus, Grafana and Alertmanager) are no longer provisioned by default when you create an {{site.data.keyword.cfee_full_notm}}.  In v2.2.0 monitoring tools are provisioned only when explictely enabled by {{site.data.keyword.cfee_full_notm}} administrators.  Furthermore, when enabled, the monitoring tools are no longer provisioned into the {{site.data.keyword.cfee_full_notm}} control plane worker nodes. They are provisioned on their own worker nodes, outside the control plane. To enable and provision monitoring tools, go to the {{site.data.keyword.cfee_full_notm}} _Monitoring_ page (in the left-nav pane) and click **Enable monitoring**.
    * When you update an existing {{site.data.keyword.cfee_full_notm}} to version 2.2.0, the existing monitoring tools are deleted from the control plane. Any existing custom configuration of the monitoring tools will be lost.
    *  New capability to replace the default [Alertmanager configuration](https://prometheus.io/docs/alerting/configuration/) with a custom configuration that allows you to customize the handling, grouping, and notification routing of alerts. In the _Monitoring_ page you will be able to _Download_ the default configuration file, and _Upload_ a configuration file from your local file system.  See for an [example](https://github.com/prometheus/alertmanager/blob/master/doc/examples/simple.yml) of a configuration file.
* New capability to tag {{site.data.keyword.cfee_full_notm}} instances in the {{site.data.keyword.Bluemix_notm}} [resource list](https://cloud.ibm.com/resources) and in the {{site.data.keyword.cfee_full_notm}} user interface.
* General improvements to the user experience of the {{site.data.keyword.Bluemix_notm}} [Cloud Foundry dashboard](https://cloud.ibm.com/dashboard/cloudfoundry/overview). The Cloud Foundry dashboard shows global views of {{site.data.keyword.cfee_full_notm}} instances, applications and public services aliased into {{site.data.keyword.cfee_full_notm}} spaces.

A new **Eirini technical preview** plan is available when creating a new {{site.data.keyword.cfee_full_notm}} instance. The plan is independent  of the v2.2.0 previously described, which applies only to the _Standard_ plan.  The Eirini Technical Preview plan allows you to explore an {{site.data.keyword.cfee_full_notm}} using native Kubernetes as the container scheduler (instead of Diego). See [Getting Started with the Eirini technical preview](https://cloud.ibm.com/docs/cloud-foundry?topic=cloud-foundry-getting-started-eirini#getting-started-eirini) for more information.
{: note}

See a demonstration of what's new in v2.1.0 in this [brief video](https://ibm.biz/CFEE-V220){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon").


## Version 2.1.1
{: #v211}

_Release Date:_ 2019-03-20

The following changes were released in version 2.1.1 of the {{site.data.keyword.cfee_full_notm}} service. To update your version, go to the _Updates and Scaling_ page in the {{site.data.keyword.cfee_full_notm}} user interface:

* Resolved problems preventing successful provisioning.


## Version 2.1.0
{: #v210}

_Release Date:_ 2019-02-20

You can only update to this version from version **2.0.2**. Update to version 2.0.2 before updating to version v2.1.0.
{: note}

The following changes were released in version 2.1.0 of the {{site.data.keyword.cfee_full_notm}} service. To update your version, go to the **Updates and Scaling** page in the {{site.data.keyword.cfee_full_notm}} user interface and click **Update**:

* New autoscaling capability that automatically scales Cloud Foundry application instances based on custom rules. Autoscaling is accessible from the CLI and from the Stratos console. The Stratos console can be optionally installed and launched from the _Overview_ page in the {{site.data.keyword.cfee_full_notm}} user interface. In the _Applications_ page of the Stratos console, select an  application and look for the **Auto Scaling** tab. Click *Create Policy* to launch the autoscaling editor to define the scaling policy for the target application.
  See the [autoscaling documentation](https://cloud.ibm.com/docs/cloud-foundry?topic=cloud-foundry-autoscale_cloud_foundry_apps#autoscale_cloud_foundry_apps) for more information.
* New capability to manage buildapcks from the {{site.data.keyword.cfee_full_notm}} user interface, including drag-and-drop positioning of a buildpack in the priority list. The capability is available in the new **Buildpacks** page of the {{site.data.keyword.cfee_full_notm}} user interface (not the Stratos console). In the release, only buildpack zip files 1 MB in size (or smaller) can be added or updated through the user interface. You can upload buildpack zip files larger than 1 MB using the `cf create-buildpack` and `cf update-buildpack` commands.
* New capability to manage organization quotas from the user interface, including visualizing which organizations are using a specific quota. The capability is available in the new **Quotas** page of the {{site.data.keyword.cfee_full_notm}} user interface (not the Stratos console). You can create new quotas and edit existing ones. You can also update the values of the default quota with the values from any existing quota by invoking the **Copy to Default** from the quota's menu.
* A new **Getting Started** page in the user interface guides users to the most important tasks to setup and use the environment.
* **Tags** can be added to an {{site.data.keyword.cfee_full_notm}} instance in the {{site.data.keyword.cloud_notm}} resource list.  Tags added in the resource list will also appear in the header of the {{site.data.keyword.cfee_full_notm}} Overview page.
* **Cloud Foundry** version 2.7.21.
* **Stratos** console version 2.3.0, which includes a security patch. Note that simply updating the {{site.data.keyword.cfee_full_notm}} version will not update the Stratos console version. You need to delete and re-install the Stratos console (in the {{site.data.keyword.cfee_full_notm}} Overview page), which automatically picks the latest Stratos console version.
* Users can navigate to the {{site.data.keyword.cfee_full_notm}} Kubernetes **cluster** page from the overflow menu in the {{site.data.keyword.cfee_full_notm}} Overview page.

If you update to v2.1.0 from a v2.0.x version, the update takes place with a single _Update_ action that automatically updates both, the control plane and the cells in sequence. If you update from a v1.x.x version, the update requires two separate _Update_ actions, one for updating the control plane (first), and one for updatting the cells.
{: note}

See a demonstration of what's new in v2.1.0 in this [brief video](https://ibm.biz/CFEE-V210){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon").


## Version 2.0.2
{: #v202}

_Release Date:_ 2019-02-08

The following changes were released in version 2.0.2 of the {{site.data.keyword.cfee_full_notm}} service. To update your version, go to the _Updates and Scaling_ page in the {{site.data.keyword.cfee_full_notm}} user interface:

* Resolved problems preventing successful version updates. This version is a pre-req for updating to version **2.1.0**.


## Version 2.0.1
{: #v201}

_Release Date:_ 2019-01-10

The following changes were released in version 2.0.1 of the {{site.data.keyword.cfee_full_notm}} service. To update your version, go to the _Updates and Scaling_ page in the {{site.data.keyword.cfee_full_notm}} user interface:

* Resolved problem that prevents custom domains from working properly.
* Resolved problem whereby organization metrics are not available while new metrics are retrieved.


## Version 2.0.0
{: #v200}

_Release Date:_ 2018-12-13

The following changes were released in version 2.0.0 of the {{site.data.keyword.cfee_full_notm}} service. To update your version, go to the _Updates and Scaling_ page in the {{site.data.keyword.cfee_full_notm}} user interface:

* Improved {{site.data.keyword.cfee_full_notm}} version update resiliency.
* Improvements to the operational availability and resiliency of {{site.data.keyword.cfee_full_notm}} instances.
* Client side certificates for more secured access to applications, allowing the server to grant application access only to certified clients.
* When an {{site.data.keyword.cfee_full_notm}} instance is created, the supporting Kubernetes cluster and Cloud Object Storage service instance are now created in the same resource group as the {{site.data.keyword.cfee_full_notm}} service instance (instead of under the "Default" resource group).
* Security patch.
* Improvements to the `ibmcloud cfee` CLI:
    * New commands for creating an {{site.data.keyword.cloud_notm}} service instance from an {{site.data.keyword.cfee_full_notm}} space and adding it to that space (`ibmcloud cfee service-create`, `ibmcloud cfee service-alias-create`).
    * New commands for scaling the capacity of an {{site.data.keyword.cfee_full_notm}} (`ibmcloud cfee scale-up`, `ibmcloud cfee scale-down`).
    * Improvements to the command for managing {{site.data.keyword.cfee_full_notm}} instances (`ibmcloud cfee environments`).

      Update the {{site.data.keyword.cloud_notm}} CLI (`ibmcloud update`) to access these command enhancements, and issue `ibmcloud cfee -help` for more details.

Updating an {{site.data.keyword.cfee_full_notm}} instance to version 2.0.0 only requires a single _update_ action that updates both, the control plane and the cells. No separate updates are required for the {{site.data.keyword.cfee_full_notm}} control plane and cells.
{: note}


## Version 1.1.2
{: #v112}

_Release Date:_ 2018-12-07

The following changes were released in version 1.1.2 of the {{site.data.keyword.cfee_full_notm}} service:
* New data centers in Chennai (che01) and Milan (mil01) are now available for provisioning {{site.data.keyword.cfee_full_notm}} instances.
* Security patch.

## Version 1.1.1
{: #v111}

_Release Date:_ 2018-11-28

The following changes were released in version 1.1.1 of the {{site.data.keyword.cfee_full_notm}} service:
* Security patch.

## Version 1.1.0
{: #v110}

_Release Date:_ 2018-11-16

The following changes were released in version 1.1.0 of the {{site.data.keyword.cfee_full_notm}} service:

* New capabilities:
   * {{site.data.keyword.cfee_full_notm}} instances can be provisioned on [virtual-dedicated](https://cloud.ibm.com/docs/containers/cs_clusters.html#clusters#clusters_ui_standard) Kubernetes worker nodes hosted on infrastructure that is devoted to your {{site.data.keyword.cloud_notm}} account.
   * A new data center in Tokyo (tok05) is available for provisionng {{site.data.keyword.cfee_full_notm}} instances.
   * [Updating](https://cloud.ibm.com/docs/cloud-foundry/updating-scaling.html#update-scale#update) an {{site.data.keyword.cfee_full_notm}} instance to a new version.
   * [Scaling](https://cloud.ibm.com/docs/cloud-foundry/updating-scaling.html#update-scale#scale) the capacity of an {{site.data.keyword.cfee_full_notm}} instance by adding or deleting Cloud Foundry cells.
   * [Auditing](https://cloud.ibm.com/docs/cloud-foundry/auditing-logging.html#auditing-logging#auditing) Cloud Foundry activities through integration with an automatically configured IBM Cloud Activity Tracker service instance.
   * Persisting Cloud Foundry [log events](https://cloud.ibm.com/docs/cloud-foundry/auditing-logging.html#auditing-logging#logging) through an automatically configured IBM Cloud Log Analysis service instance.
   * Health Check view that indicates the operational status of {{site.data.keyword.cfee_full_notm}} components.
   * New commands to perform {{site.data.keyword.cfee_full_notm}} related actions on the command line interface (CLI):
     * `ibmcloud cfee create`, `ibmcloud cfee create-locations`, and `ibmcloud cfee create-status` commands to create instances from the CLI, get a list of available data centers where an {{site.data.keyword.cfee_full_notm}} can be provisioned, and check the provisioning status of an {{site.data.keyword.cfee_full_notm}} being created.
     * `ibmcloud cfee create-permission-get` and `ibmcloud cfee create-permission-set`  [commands](https://cloud.ibm.com/docs/cloud-foundry/permissions.html#permissions#permcli-creating) to retrieve and set the permissions required to create instances. The commands aggregate and simplify the setting of permissions for the {{site.data.keyword.cfee_full_notm}} service and for the required supporting services.
     * `ibmcloud catalog blacklist` command to simplify [visibility control](https://cloud.ibm.com/docs/cloud-foundry/add-serv-inst.html#workingwith-services#service_visibility) of {{site.data.keyword.cloud_notm}} services for users in an {{site.data.keyword.cloud_notm}} account.

* Resolved problems:
   *  Resolved Problem: Intermittent error accessing cell metrics
<br/>
See a demonstration of what's new in v1.1.0 in this [brief video](https://ibm.biz/CFEE-V110){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon").  You can also find other videos on various topics in the [{{site.data.keyword.cfee_full_notm}} video playlist](https://ibm.biz/CFEE-Playlist){: new_window} ![External link icon](../icons/launch-glyph.svg "External link icon").
