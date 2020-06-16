---

copyright:
  years: 2018, 2020
lastupdated: "2020-06-16"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Preparing your account
{: #prepare}

CFEE instances are deployed on infrastructure resources (Kubernetes worker nodes from the IBM Container service) that are billed to the IBM Cloud account. This means that the IBM Cloud account under which the CFEE instance is created must be a paid account (Pay-As-You-Go or a Subscription account).  If the IBM Cloud account under which you intend to create an CFEE instance is a trial account you will be asked to upgrade the account when you try to create an CFEE instance.  When an IBM Cloud account is upgraded (from a trial account to a Pay-As-You-Go or a Subscription account), the IBM Cloud account becomes linked to an IBM Cloud Infrastructure (previously known as "SoftLayer") account (through which infrastructure resources can be created). For more information, see [Account types](https://cloud.ibm.com/docs/account/index.html#accounts). The cost of those infrastructure resources shows up in your IBM Cloud invoice.

## How to determine if the IBM Cloud account can create instances
{: #account-check}

You can determine if an IBM Cloud account is a trial or paid account, and whether it is linked to an IBM Cloud Infrastructure account, by looking at the account information at the top right corner of the IBM Cloud banner.  

In the example below user _Mary Smith_ is logged into the IBM Cloud account _MyCompany_, which is a trial account.
![Account Checking](img/AccountExample_1.png  "Screen cap that shows an IBM Cloud account not linked to a IBM Cloud Infrastructure (previously known as a SoftLayer) account")

In the example below the same IBM Cloud account _MyCompany_ has been upgraded to a paid account.  As a result of the upgrade the account is now linked to the IBM Cloud Infrastructure account _1684806_.  Both accounts are shown in the "Account" field.
![Account Checking](img/AccountExample_2.png  "Screen cap that shows an IBM Cloud account linked to an IBM Cloud Infrastructure  (previously known as a SoftLayer) account")

## IBM Cloud Account Requirements

To provision {{site.data.keyword.cfee_full_notm}} instances, you _must_ have certain settings enabled for your account.  Specifically, your account must be enabled for **VRF** and **Cloud Service Endpoints**.  The easiest way to check is to use the `ibmcloud` CLI.  Login via the CLI, target the intended account (if required), and run

```
$ ibmcloud account show
```
{: codeblock}

There will be one line in the output for each of the two required settings, and these should each show a value of `true`.  Here is the output for a correctly configured account:

```
VRF Enabled:                        true
Service Endpoint Enabled:           true
```
{: codeblock}

Please see [here](account?topic=account-vrf-service-endpoint) for information on configuring these settings correctly.

## Using an IBM Cloud Infrastructure account instead of upgrading the IBM Cloud account
{: #account-linkswitching}

If you have Administrator role in an IBM Cloud account, you can use an IBM Cloud Infrastructure account to create the CFEE instance without upgrading the IBM Cloud account.

**Warning:** We recommend that you upgrade the IBM Cloud account instead. If you use an IBM Cloud Infrastructure account now and you update the IBM Cloud account in the future (to a Pay-As-You-Go or a Subscription account), the updated IBM Cloud may still use the  IBM Cloud Infrastructure account (whose credentials you set now) when creating future infrastructure resources. Furthermore, if you use a different IBM Cloud Infrastructure account in the future for creating Cloud Foundry Enterprise Environments, users in the IBM Cloud account may not be able to access infrastructure resources created under the IBM Cloud Infrastructure account whose credentials you set now.

To use a IBM Cloud Infrastructure account without upgrading the IBM Cloud account (see the screens below for illustration):
1. In the screen shown when the IBM Cloud account is not upgraded, click **Use a SoftLayer account**. The SoftLayer account is the same as your IBM Cloud Infrastructure account.
2. Enter the **Username** and **API Key** from an IBM Cloud Infrastructure account. To get the IBM Cloud Infrastructure Username and API key access the [SoftLayer console](https://control.softlayer.com). Once you log in to SoftLayer, select the account you want to link to the {{site.data.keyword.Bluemix_notm}} account. Selecting the account opens the account's profile page. Scroll down to the end of the page to find the account's user name and API key. If you don't have an API key, you can generate it if you are the account owner. If you are not the account owner, ask the account owner to generate it.
3. Click **Set credentials**.

![Account Checking](img/UpgradeAccountPage_1.png  "Screen cap that highlights the hyperlinklink to use a SoftLayer account to create an CFEE instance")

![Account Checking](img/UpgradeAccountPage_2.png  "Screen cap that shows the parameters for using a SoftLayer account to create an CFEE instance")

**Note:** You must have sufficient permissions in the IBM Cloud Infrastructure account to create a regular Kubernetes cluster from the IBM Container service. If you don't, ask the IBM Cloud Infrastructure account administrator, or the user who gave you access to the IBM Cloud Infrastructure account to grant you those additional permissions.
