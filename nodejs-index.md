---

copyright:
  years: 2015, 2019
lastupdated: "2019-09-20"
subcollection: "Nodejs"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# SDK for Node.js
{: #nodejs_runtime}

The Node.js runtime on {{site.data.keyword.Bluemix}} is powered by the sdk-for-nodejs buildpack.
The sdk-for-nodejs buildpack provides a complete runtime environment for Node.js apps.
{: shortdesc}

The sdk-for-nodejs buildpack is used when the application contains a **package.json** file in the root directory.

The application must listen on the port that is assigned to it through the PORT environment variable.
```
var port = (process.env.PORT || 3000);
```
{: codeblock}

## Starter application
{: #starter_application}

{{site.data.keyword.Bluemix_notm}} provides a Node.js starter applications.  The Node.js starter application is a simple Node.js app that provides a template that you can use for your app. You can experiment with the starter app, and make and push changes to the {{site.data.keyword.Bluemix_notm}} environment.

## App Management (deprecated)
{: #app_management}

**Deprecation Note**:  As of version 4.0, the SDK for Node.js buildpack no longer supports App Management.    

{{site.data.keyword.Bluemix_notm}} provides a number of utilities for managing and debugging your Node.js app.  See [App Management](/docs/runtimes-common/app_mng.html) for complete details.  
