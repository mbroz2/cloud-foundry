---
copyright:
  years: 2019
lastupdated: '2019-08-12'
---

{:shortdesc: .shortdesc}  
{:new_window: target="_blank"} 
{:codeblock: .codeblock}  
{:pre: .pre} 
{:screen: .screen} 
{:tip: .tip}

# Management enablement

{: #management-enablement}

This document is an introduction to administrative and operational aspects of {{site.data.keyword.cfee_full_notm}} ({{site.data.keyword.cfee_short}}).

## Overview of Management

{: #management-overview}

During the lifecycle of a {{site.data.keyword.cfee_short}} instance there is a shared responsibility between the customer and IBM to handle administrative and operational tasks.

Operating and managing a {{site.data.keyword.cfee_short}} instance has two sets of responsibilities:

- Customer-managed responsibilities
- IBM Cloud-managed responsibilities

## Roles and Responsibilities

{: #management-roles-responsibilities}

Responsibilities are associated with roles. The customer can decide to fulfill all roles on his own or to engage with a **Managed Service Provider** (MSP) as shown in the table below:

Role                               | Responsibility | Description
---------------------------------- | -------------- | ---------------------------------------------------------------------
Account Owner                      | Customer       | Manage on-boarding and support of users to his account in IBM Cloud
Application Developer and Operator | Customer       | Manage lifecycle of customer-owned applications
Cloud Foundry Administrator        | Customer       | Manage day-to-day requirements of development teams and Cloud Foundry
**Environment Administrator**      | Customer       | Manage all elements of Cloud Foundry, IaaS, databases, etc
IBM Cloud                          | IBM            | Provide means for deployment, scaling, upgrading, and operating

The following picture is a visual representation of the roles and responsibilities:

![{{site.data.keyword.cfee_full_notm}}](images/ICFEE_HighLevelRoles.png "High level roles in an {{site.data.keyword.cfee_full_notm}}")

## Customer Management

{: #management-msp-engagement}

The customers choice to use {{site.data.keyword.cfee_short}} includes how to handle the Environment Administrator role. The customer can fulfill the responsibilities of the Environment Administrator on their own, or may engage a management offering provider to fulfill that role for them.

The customer decides {{site.data.keyword.cfee_short}} management based on the following:

- Understand there are layers of "shared management responsibilities" – Customer & IBM Cloud roles
- Deploy a {{site.data.keyword.cfee_short}} instance (with or without the assistance of management offering provider)
- Decide who should fulfill the Environment Administrator role – the customer or a management offering provider)
- If desired, engage with that management offering provider for environment management services

## Managing change

{: #managing-change}

During the life-cycle of a {{site.data.keyword.cfee_full_notm}} ({{site.data.keyword.cfee_short}}) instance there is a shared responsibility between the customer and IBM to apply changes.

The customer will notice available updates in the IBM Cloud console. The customer can implement the changes as described [here](https://cloud.ibm.com/docs/cloud-foundry?topic=cloud-foundry-update-scale#update-scale).

Various considerations need to be taken into account based on the customers procedures for change management. For instance:

- determination of appropriate change windows
- installation of prerequisites
- installation of changes
- verification of successful changes
- customer-specific considerations

## Management Offering Provider

{: #management-service-provider}

During the life-cycle of a {{site.data.keyword.cfee_short}} instance there is a shared responsibility between the customer and IBM to perform administrative and operational tasks: monitoring health, handling alerts, executing upgrades, scaling up and down the environment, etc. These tasks are typically done by the Environment Administrator role.

If the customer decides to not manage this role themselves, but instead engage a management offering provider to fulfill the responsibilities of the Environment Administrator role, the following steps are recommended with regard to that provider:

1. You should check the terms and conditions, capabilities, operations practices, roles and responsibilities, rules, SLA offered, cost, and all other relavent information before signing a management offering contract<br>
  (outside of and not related to the digital purchase of {{site.data.keyword.cfee_short}} instances)
2. You would then add the management offering provider to have access tp the IBM Cloud Account that contains the {{site.data.keyword.cfee_short}} instances, and any other set up steps with which you have agreed upon
3. The management offering provider would then access your environment and would set up steps, processes and tooling as it was agreed upon
4. The management offering provider would then fulfill the responsibilities of the Environment Administrator for proper availability, health and performance of {{site.data.keyword.cfee_short}} instances, along with any other agreed upon tasks.

<!-- ### MSP Enabled Methods This enablement by IBM Cloud to an MSP lists the recommended methodologies and operations strategies for managing the Cloud Foundry environment, allowing for a flexible and complete management solution. It is enabled by a "management operations" concept that attaches tools to levels of the management capability. IBM defines the management operations and includes various information about possible tooling, connecting methods, and examples. Some management tooling IBM Cloud offers with the Cloud Foundry environment stand-up, others are added by the MSP. MSPs can easily expand operational tools into this management concept. This allows a flexible delivery of management with low operational overhead and known starting point capabilities, and allows MSPs to expand with unique offerings. -->

 ## Operational Model

The below gives an opinionated overview on how to setup CFEE for production workload and how to configure secondary tools and services to operate one or multiple CFEE instances. A few simple steps allow the automation of each step of the management process.

![{{site.data.keyword.cfee_full_notm}}](images/OpsModel.png "Operational Model for {{site.data.keyword.cfee_full_notm}}")

From an end-to-end operation flow, the three steps below are best practices for operations on CFEE.

1. **Monitor**: Various monitoring solutions from application to Kubernetes are chosen to monitor Kubernetes, CFEE instances and applications.
2. **Event/Incident Management**: Manage events that are generated from different monitoring tools, alerting and notifications for services and application disruptive events, and escalation with incidents.
3. **Notification**: Various notification channels integrate with the Event/Incident Management system to notify SRE to take corrective actions for problem mitigation.

**IBM Cloud Availability Monitoring**: [IBM Cloud Availability Monitoring](https://cloud.ibm.com/catalog/services/availability-monitoring) helps DevOps teams ensure CFEE applications are always available and meeting user expectations for response time. The service which runs synthetic tests from locations around the world, help proactively detect and fix performance issues before they impact users.

**Prometheus**: [Prometheus](https://prometheus.io/) enables you to analyze, visualize and manage metric alerts in CFEE, in case there are multiple CFEE instances deployed in your account, a centralized federated Prometheus can be optionally deployed to empower alert consolidation and customization, which you can create your own alert using existing CFEE metrics.

**Sysdig**: IBM Cloud Monitoring with [Sysdig](https://cloud.ibm.com/docs/Monitoring-with-Sysdig) is powered by Sysdig in partnership with IBM, which is a unified approach to cloud-native visibility, helping monitor your Kubernetes container environment. Currently, Sysdig is not the default monitoring source for IBM Cloud Event Management, but it can be built to integrate using CEM generic webhooks and define the alert normalization following the Sysdig user [guide](https://cloud.ibm.com/docs/AlertNotification?topic=AlertNotifications-task_zpk_hnm_4x).

**CEM**: [IBM Cloud Event Management](https://cloud.ibm.com/catalog/services/event-management) sets up real-time event/incident management for your services, applications, and infrastructure, which receive events from IBM Cloud Availability Monitor, Prometheus and Sysdig. This allows your DevOps teams to correlate different sources of events into actionable incidents, synchronize teams, and automate incident resolution, which help restore service and resolve operational incidents quickly. CEM has default [incoming integration](https://cloud.ibm.com/docs/EventManagement?topic=cloudeventmanagement-definingeventsources) with IBM Cloud Availability Monitor, Prometheus and [outgoing integration](https://cloud.ibm.com/docs/EventManagement?topic=cloudeventmanagement-task_amz_zcp_h1b) with Voice, SMS, Email and Slack.

**Pagerduty**: [Pagerduty](https://www.pagerduty.com/) acts as the central event/incident management system for Real-Time Operations, and integrates machine data & human intelligence to improve visibility & agility across organizations. This is an alternative options to use CEM if your organization is already using it for event and incident management.

**Notification Channel**: [Slack](https://slack.com/), Voice, SMS and Email are the notification channels you can choose to notify your organizations SRE.

### Considerations before provisioning CFEE

{: #before-provisioning-cfee}

We recommend to check if VLAN spanning is enabled in your account before provisioning a {{site.data.keyword.cfee_short}} instance:

```
ibmcloud ks vlan-spanning-get
```

{: pre}

If it is not enabled, please [enable VLAN spanning](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) before attempting {{site.data.keyword.cfee_short}} provisioning.

We also recommend to create a resource group and provision one or multiple {{site.data.keyword.cfee_short}} instances into each resource group. A resource group allows to [organize resources](/docs/overview?topic=overview-whatis-rgs) and separately control the access to these resources.

You can create and target a resource group using these commands:

```
ibmcloud resource group-create myProdResGroup
ibmcloud target -g myProdResGroup
```

{: pre}

Now you can create one or multiple [access groups](/docs/iam?topic=iam-groups) with different permissions and invite developers and operators to these access groups in order to give them access to your {{site.data.keyword.cfee_short}} instances.

To create an access group:

```
ibmcloud iam access-group-create prdAccGrpDepl -d "access group that allows CFEE deployments"
```

{: pre}

Assign the following permissions to the access group in order to allow members of the access group to deploy CFEE instances. More about [CFEE permissions](/docs/cloud-foundry?topic=cloud-foundry-permissions#permissions) :

```
ibmcloud iam access-group-policy-create prdAccGrpDepl --roles Viewer --resource-group-name myProdResGroup  
ibmcloud iam access-group-policy-create prdAccGrpDepl --roles Editor --resource-group-name myProdResGroup --service-name cfaas  
ibmcloud iam access-group-policy-create prdAccGrpDepl --roles Administrator,Manager --resource-group-name myProdResGroup --service-name containers-kubernetes   
ibmcloud iam access-group-policy-create prdAccGrpDepl --roles Administrator --resource-group-name myProdResGroup --service-name databases-for-postgresql  
ibmcloud iam access-group-policy-create prdAccGrpDepl --roles Editor,Manager --resource-group-name myProdResGroup --service-name cloud-object-storage
```

{: pre}

Now you can invite users into your account and into the access group:

```
ibmcloud account user-invite email@domain.com  
ibmcloud iam access-group-user-add prdAccGrpDepl email@domain.com
```

{: pre}

### Provisioning CFEE

{: #provisioning-cfee-cli}

To learn how to create a {{site.data.keyword.cfee_short}} instance via the UI and to understand the available provisioning options, please go to [Creating a CFEE instance page](/docs/cloud-foundry?topic=cloud-foundry-create-environment). The following JSON file is an example configuration to illustrate the automated provisioning of {{site.data.keyword.cfee_short}} instances using the [ibmcloud cli](/docs/cli?topic=cloud-cli-ibmcloud_commands_cfee) .

```
ibmcloud cfee create -c example.json
```

{: pre}

Copy the following JSON to a file and change the options to meet your needs:

```
{
  "name": "my-new-cfee",
  "region_id": "eu-de",
  "parameters": {
    "cells_per_zone": 1,
    "cluster": {
      "zones": [
        {
          "id": "fra02",
          "private_vlan": "",
          "public_vlan": ""
        },
        {
          "id": "fra04",
          "private_vlan": "",
          "public_vlan": ""
        },
        {
          "id": "fra05",
          "private_vlan": "",
          "public_vlan": ""
        }
      ]
    }
  }
}
```

The create-status command can be used to poll for status messages during the creation process:

```
ibmcloud cfee create-status my-new-cfee
```

{: pre}

### Enable Monitoring

{: #enable-monitoring-cli}

We recommend to enable the {{site.data.keyword.cfee_short}} monitoring to benefit from pre-configured monitors and alerting. Please got to [monitoring](/docs/cloud-foundry?topic=cloud-foundry-monitoring#grafana-dashboards) to learn more about {{site.data.keyword.cfee_short}} monitoring and how to enable it via the UI. In order to enable it via the ibmcloud cli you need to target a specific {{site.data.keyword.cfee_short}} instance and run the `ibmcloud cfee monitoring-enable` command:

```
ibmcloud cfee environments
ibmcloud target --cf-api "api.my-new-cfee.eu-de.containers.appdomain.cloud"
ibmcloud cfee monitoring-enable
```

You can check the status of monitoring in your {{site.data.keyword.cfee_short}} instance using:

```
ibmcloud cfee monitoring-status
```

{: pre}

Access to the monitoring tools is provided via links in the IBM Cloud console on the overview page of a {{site.data.keyword.cfee_short}} instance in the operations section (Operations -> Monitoring).

### Receiving Alerts

{: #receiving-alerts}

This section shows you how to setup Prometheus alert manager with CEM or Pagerduty as notification channel and receive alert.

1. Find your CFEE instance from IBM Cloud [Cloud Foundry](https://cloud.ibm.com/cloudfoundry/environments) page.
2. Click the CFEE instance you want to configure.
3. Navigate to monitoring section and switch to configuration tab.
4. Upload your own alert manager configuration file in order to define inhibition rules, notification routing, and notification receivers.

Here is the sample alert manager configuration file for defining the receivers to CEM and Pagerduty.

```
global:
    route:
      receiver: 'default'
      group_by: ['clustername']
      group_wait: 10s
      group_interval: 5m
      repeat_interval: 4h
    receivers:
    - name: 'default'
      pagerduty_configs:
      - service_key: XXXXXXX
      webhook_configs:
      - url: XXXXXX
        send_resolved: true
```

In the webhook_configs section, replace the "url" field with the one generated from CEM. For more information, see [this page](https://cloud.ibm.com/docs/EventManagement/em_prometheus.html). After configuring the receiver to CEM, CEM will act as the central event and incident management platform, [here](https://cloud.ibm.com/docs/EventManagement?topic=cloudeventmanagement-task_jvr_kzx_ybb) is the guide on how to configure the notification channel from CEM to Slack, as well as the [guide](https://cloud.ibm.com/docs/EventManagement?topic=cloudeventmanagement-managingusersandgroups) for email, SMS and Voice.

In the pagerduty_configs section, replace the "service_key" with the one generated from your Pagerduty integration. For more information, see [this page](https://www.pagerduty.com/docs/guides/prometheus-integration-guide/).

### Monitoring applications

{: #application-monitoring}

To monitor the availability of applications running on top of your {{site.data.keyword.cfee_short}} instances you can use the [Availability Monitoring](https://cloud.ibm.com/catalog/services/availability-monitoring) service from the IBM Cloud catalog.

We recommend to create one central instance of the Availability Monitoring service in one of the available regions and configure synthetic tests for each of the applications running on your {{site.data.keyword.cfee_short}} instance that you want to monitor.

To do so, follow these steps:

1. Create a CF org and space within the us-south, au-syd or eu-gb public CF instance using the IBM Cloud [console](https://cloud.ibm.com/account/cloud-foundry). Then target that org and space:

  ```
  ibmcloud target -r [us-south,eu-gb,au-syd]
  ibmcloud target --cf https://api.[us-south,eu-gb,au-syd].cf.cloud.ibm.com
  ibmcloud target -o yourOrg -s yourSpace
  ```

  {: pre}

2. Provision an instance of the "Availability Monitoring" service and bind it to a dummy application (the app can remain stopped, but the "Availability Monitoring" currently requires to be bound to an app).

  ```
  ibmcloud cf create-service AvailabilityMonitoring Lite yourMonServ
  git clone https://github.com/IBM-Bluemix/get-started-node
  cd get-started-node
  ibmcloud cf push --no-start
  ibmcloud cf bind-service GetStartedNode yourMonServ
  ```

  {: pre}

3. Now select your instance of "Availability Monitoring" from your [resource list](https://cloud.ibm.com/resources) (search in Cloud Foundry services) and open the dashboard by clicking on the application name. Scroll down and add synthetic tests to monitor any website URL, any REST API URL or to do automated Selenium test scripts. Please see this [tutorial](https://www.ibm.com/cloud/garage/tutorials/tutorial_bam?task=1) for further information. Please also consider to disable or delete the default test against your dummy application.

### Capacity management

{: #capacity-management}

There are two locations that you can visit to understand the resource usage of your {{site.data.keyword.cfee_short}} instances.

1. The first one offers a point in time information about CPU and memory consumption. It gives you an immediate indication about the current resource usage and allows you to identify applications that are causing excessive CPU or Memory consumption. Please find more information in the [reviewing resource usage](/docs/cloud-foundry?topic=cloud-foundry-reviewing-resource-usage) section.
2. The second one is via the [Monitoring](/docs/cloud-foundry?topic=cloud-foundry-monitoring) tools that you can enable for each {{site.data.keyword.cfee_short}} instance. The Grafana UI that is available to you after enabling the monitoring tools, offers the historical data. Amongst other dashboards you can use the "CF: Diego Cell Dashboard" and the "CF: Cells Capacity" to inform yourself about memory and disk consumption or the amount of app containers running on your CF cells.
