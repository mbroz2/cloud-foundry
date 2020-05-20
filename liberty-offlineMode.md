---

copyright:
  years: 2016, 2018
lastupdated: "2018-11-20"
subcollection: cloud-foundry

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# Use the offline mode for Liberty
{: #offline_mode}

When a Liberty application is pushed to {{site.data.keyword.Bluemix}}, the Liberty buildpack can access sites external to {{site.data.keyword.Bluemix_notm}}
to acquire artifacts required by the application.  The following are the external sites that the Liberty buildpack can access.  In [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated) and
[{{site.data.keyword.Bluemix_local_notm}}](/docs/local/index.html#local) environments, these sites might need to be *whitelisted*.

* https://download.run.pivotal.io, and https://java-buildpack.cloudfoundry.org are used to access components for:
  * [AppDynamics agent ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.appdynamics.com/)
  * [MariaDB JDBC driver ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://mariadb.com/)
  * [New Relic agent](/docs/runtimes/liberty/monitoring?topic=liberty-new_relic)
  * [OpenJDK](/docs/cloud-foundry?topic=cloud-foundry-customizing_jre#OpenJDK)
  * [PostgreSQL JDBC driver ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.postgresql.org)
* https://dl.zeroturnaround.com/jrebel/ is used to access components for [JRebel ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://zeroturnaround.com/software/jrebel/).
* https://download.ruxit.com/agent/paas/cloudfoundry/java is used to access components for [Dynatrace Ruxit agent](dynatrace.html).
* http://downloads.dynatracesaas.com/cloudfoundry/buildpack/java/  is used to access the [Dynatrace agent](dynatrace.html).

## Working with a proxy
{: #liberty-working_with_proxy}

In some environments, such as [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated) and
[{{site.data.keyword.Bluemix_local_notm}}](/docs/local/index.html#local), a proxy can be configured. See
[Working with a proxy](/docs/cloud-foundry?topic=cloud-foundry-working_with_proxy) for more details.
