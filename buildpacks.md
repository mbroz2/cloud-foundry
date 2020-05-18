---

copyright:
  years: 2017, 2019
lastupdated: "2019-08-26"
subcollection: cloud-foundry

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# IBM Cloud Foundry buildpacks
{: #available_buildpacks}

Cloud Foundry buildpacks provide the runtime support for applications in the Cloud Foundry environment. When you deploy an application to {{site.data.keyword.cloud}}, it starts a buildpack that supports your application type. {{site.data.keyword.cloud_notm}} provides Cloud Foundry buildpack support for Java EE, Node.js, ASP.Net, Swift, and other application types.
You can use the buildpacks included with {{site.data.keyword.cloud_notm}} to deploy applications and bind them to services.

*  Cloud Foundry

    Cloud Foundry is an open source platform for application lifecycle automation.  {{site.data.keyword.Bluemix}} is built on top of the Cloud Foundry platform as a service. Check out the [Cloud Foundry documentation](https://www.cloudfoundry.org/learn/) to learn more.

*  Cloud Foundry Application

   A cloud foundry application or app, is any application intended to be instantiated by on one of the buildpacks provided by {{site.data.keyword.Bluemix_notm}}.

*  Buildpack

   A buildpack is a typically language specific package of software provided by {{site.data.keyword.Bluemix_notm}}. When an application is deployed to {{site.data.keyword.Bluemix_notm}} a buildpack appropriate for the application is chosen. The buildpack provisions the runtime environment for the application.  You can see the set of buildpacks provided by {{site.data.keyword.Bluemix_notm}} by issuing the `ibmcloud cf buildpacks` command from the command line.

*  Runtime

   A runtime is the set of software components which provide the exectuion environment for an application.  The terms *runtime* and *buildpack* are sometimes used interchangeably.  When a buildpack has completed deploying an application the runtime environment is established.
   
*  Starter

   A starter is a simple application designed for a particular runtime. The starters provide templates or samples of the language and runtime specific application types. You can use a starter to begin development of more sophisticated applications. 

*  Service

   A service is a {{site.data.keyword.Bluemix_notm}} provided facility which can be coupled with an application.
