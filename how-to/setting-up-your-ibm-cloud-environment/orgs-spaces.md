---

copyright:
  years: 2018, 2019
lastupdated: "2019-08-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Creating organizations and spaces
{: #create_orgs}

Apps in an {{site.data.keyword.cfee_full}} are scoped within specific spaces. In turn, a space exists within a specific organization. Members of an organization share a quota plan, apps, services instances, and custom domains. After organizations and spaces are created, users can be added to them with specific roles that determine the level of access and control to those organizations and spaces. To create organizations and assign managers to those organizations, you need an administrator role for both the {{site.data.keyword.cfee_full_notm}} service and to the specific environment instance to which users are being added. These permissions are set in the _Identity & Access_ page under the **Manage > Users** menu in the {{site.data.keyword.Bluemix}} header.
{: shortdesc}

## Creating organizations
{: #create-org}

To create organizations in an {{site.data.keyword.cfee_full_notm}}:

1. Go to the [{{site.data.keyword.Bluemix_notm}} Cloud Foundry Environments dashboard](https://cloud.ibm.com/resources?filter=cf_environments){: new_window} and open the {{site.data.keyword.cfee_full_notm}} where you want to create organizations.
2. In the {{site.data.keyword.cfee_full_notm}} user interface, go to the **Organizations** entry in the navigation pane to open the _organizations_ page.
3. Click **Create Organization**.
4. Enter a **Name** for the new organization.
5. Under **Managers**, identify at least one user. Only users with a Manager role can add members to that organization.
6. Under **Quota plan**, select an available plan. A quota plan limits the memory available for apps in the organization, the maximum number of apps instances hosted, and the maximum number of routes. The quota that is allocated to an organization represents the resources that are available within the organization.

In addition to managing organization membership, organization managers can create, view, edit, or delete spaces within the organization. Organization managers can also view the organization's usage and quota, invite users to the organization, manage membership and roles in the organization, and manage custom domains for the organization.

To create spaces within an organization, in the _Organizations_ page, go to the **Spaces** tab and click **Add new**. Members of an organization can also create and manage their own spaces within an organization. For more information on Cloud Foundry roles, see [Cloud Foundry Access](https://cloud.ibm.com/docs/iam/cfaccess.html#cfroles){: new_window}.

**Note**: Access to organizations and spaces within an {{site.data.keyword.cfee_full_notm}} requires that users have permission to access the environment itself. Without permission to access the {{site.data.keyword.cfee_full_notm}}, users can't access organizations and spaces within the environment, regardless of their membership and role in those organizations and spaces.
