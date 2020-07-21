---

copyright:
  years: 2015, 2020
lastupdated: "2020-07-20"
subcollection: cloud-foundry

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:deprecated: .deprecated}

# Managing Liberty and Node.js apps
{: #app_management}

## {{site.data.keyword.cloud_notm}} console and CLI

### Managing application through {{site.data.keyword.cloud_notm}} console

The {{site.data.keyword.cloud_notm}} console provides the ability to start, stop and restart application from the "Action" tab on your application from console.

### Managing application through cf CLI

It can also be accomplished from command line.

  To start the application:

  ```
  cf start myApp
  ```
  
  To stop an application:
  
  ```
  cf stop myApp
  ```
  
  To restart an application:
  
  ```
  cf restart myApp
  ```
  
## Application management through SSH shell session

Application can be managed and debugged through ssh-ing into them. This can be accomplished through {{site.data.keyword.cloud_notm}} console and cf CLI.

### {{site.data.keyword.cloud_notm}} console

User can ssh into the application through their application through {{site.data.keyword.cloud_notm}} console. Navigate to the Runtime tab on your application and then click on SSH to manage.

### Command Line CLI

To ssh into your application from the command line use:

```
cf ssh myApp
```


App Management is a set of development and debugging utilities that can be enabled for your Liberty applications on {{site.data.keyword.Bluemix}}.
{:shortdesc}


## App Management utilities
{: #Utilities}

### Liberty utilities
* [proxy](/docs/cloud-foundry?topic=cloud-foundry-app_management#proxy)
* [noproxy](/docs/cloud-foundry?topic=cloud-foundry-app_management#noproxy)
* [hc](/docs/cloud-foundry?topic=cloud-foundry-app_management#hc)
* [debug](/docs/cloud-foundry?topic=cloud-foundry-app_management#debug)
* [jmx](/docs/cloud-foundry?topic=cloud-foundry-app_management#jmx)
* [localjmx](/docs/cloud-foundry?topic=cloud-foundry-app_management#localjmx)

## How to configure App Management
{: #configure}

To enable App Management utilities, set the value of the *BLUEMIX_APP_MGMT_ENABLE* environment variable to the utility or list of utilities you want to enable, then restage your application. You can enable multiple utilities by separating them with a **+**.

For example, to enable *hc*, *debug* and *trace* utilities, run the following command:

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_ENABLE hc+debug+trace
```
{: codeblock}

Restage your application after you set the environment variable:

```
ibmcloud cf restage myApp
```
{: codeblock}

If you do not want the App Management utilities to be installed with your application, set the *BLUEMIX_APP_MGMT_INSTALL* environment variable to 'false' and restage your application.

For example, run the following commands to stage your application without App Management utilities:

```
ibmcloud cf set-env myApp BLUEMIX_APP_MGMT_INSTALL false
ibmcloud cf restage myApp
```
{: codeblock}

## Restrictions
{: #restrictions}
* Changes that you make to your application by using App Management are transient and are lost after you exit this mode. This mode is only for temporary development use and is not intended to be used as a production environment due to performance.

### Liberty utilities
{: #liberty_utilities}

#### jmx
{: #jmx}

The *jmx* utility enables the JMX REST Connector to allow a remote JMX client to manage the application by using {{site.data.keyword.Bluemix_notm}} user credentials.

You can monitor multiple instances of an application by using JMX, but it requires a separate JMX connection for each instance. The default is to monitor instance 0. To monitor instance 1, you can use the following code snip:

```
ibmcloud cf ssh -i 1 -N -T -L 5000:127.0.0.1:5001
```
{: codeblock}

For more information on configuring a JMX connector, see [Configuring secure JMX connection to the Liberty profile ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www-01.ibm.com/support/knowledgecenter/was_beta_liberty/com.ibm.websphere.wlp.nd.multiplatform.doc/ae/twlp_admin_restconnector.html){:new_window}.

**Important**: The *jmx* utility does not start *proxy*.

#### localjmx
{: #localjmx}

The *localjmx* utility enables the [localConnector-1.0 ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_feature_localConnector-1.0.html){:new_window} Liberty feature. Combined with local port forwarding, *localjmx* acts an alternate way of allowing a remote JMX client to manage the application.


**Before you begin**: *localjmx* requires you to install JConsole.

The *localjmx* utility is only applicable to applications running on a Diego cell. To use *localjmx*, first establish port forwarding using the `ibmcloud cf ssh` command. For example:

```
ibmcloud cf ssh -N -T -L 5000:127.0.0.1:5000 <appName>
```
{: codeblock}

Next, to connect with JConsole, choose **Remote Process**, specify `127.0.0.1:5000`, and use an insecure connection.

#### proxy
{: #proxy}

The *proxy* utility provides minimal application management between your application and {{site.data.keyword.Bluemix_notm}}.

When enabled, the buildpack starts a proxy agent that is located between your application's runtime and container.  The *proxy* utility handles all requests that the application receives. Based on the type of request, it either takes an App Management action or forwards the request to your application. By using *proxy*, your application container continues to live even if the application crashes. The proxy agent also allows for incremental file updates, which enables the *Live Edit* mode.

Some App Management utilities require you to use the *proxy* utility with your application, and can start *proxy* automatically.

#### noproxy
{: #noproxy}

The *noproxy* utility disables the *proxy* utility when it is automatically started by another utility.  With Diego, the proxy is not necessary because Diego provides the capability to *ssh* directly to your application and set up port forwarding.

The *noproxy* utility only applies to applications that run in a Diego cell.

#### hc 
{: #hc}

The (*hc*) Health Center agent enables your application to be monitored by the Health Center client.  

When you have the Health Center agent enabled, you can analyze the performance of your Liberty applications by using the IBM Monitoring and Diagnostic Tools. For more information see [How to analyze the performance of Liberty Java in {{site.data.keyword.Bluemix_notm}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/bluemix/2015/07/03/how-to-analyze-performance-in-bluemix/){:new_window}.

**Important:** The *hc* utility starts *proxy*.

**Using *hc* with *noproxy* **

The *hc* utility can be used in conjunction with *noproxy*. To use Health Center with *noproxy*, first establish port forwarding using the `ibmcloud cf ssh` command. For example:

```
ibmcloud cf ssh -N -T -L 1883:127.0.0.1:1883 <appName>
```
{: codeblock}

Next, to connect with the Health Center client, use an [MQTT connection ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.ibm.com/support/knowledgecenter/SS3KLZ/com.ibm.java.diagnostics.healthcenter.doc/topics/connectingtojvm.html){: new_window} and specify the host as `127.0.0.1` and port as `1883`.

##### For Node.js versions after 6.3.0
When you start debugging mode, *proxy* is automatically enabled, even if you use a version of Node.js that does not include *proxy*. Versions of Node.js after 6.3.0 do not include *proxy.* If you use the *inspector* utility with versions of Node.js after 6.3.0, you can turn off *proxy* again by using *noproxy.*

Instead of using *proxy* to access the *inspector* interface, you use the Developer Tools capability of the Google Chrome web browser.  

Enable access to the URL with local port forwarding with the following command:
```
ibmcloud cf ssh -N -T -L 9229:127.0.0.1:9229 <appName>
```
{: codeblock}

Get the startup log for the application by using the following command.
```
ibmcloud cf logs <appName> --recent
```
{: codeblock}

If the *inspector* utility is active, the log contains messages similar to the following:
```
 ... You will need a SSH tunnel for port 9229 to be able to use the Chrome DevTools to remotely debug your app
 ... Starting app with 'node --inspect=9229  app.js '
```
{: codeblock}

Use an up-to-date version of the Google Chrome web browser to browse to `chrome://inspect`.
From this URL, you see your app listed along with a link to your applictation files such as `file://home/vcap/app/app.js`., then select **inspect** to access the inspect interface.
