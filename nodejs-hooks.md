---

copyright:
  years: 2018
lastupdated: "2018-11-09"
subcollection: cloud-foundry

---

{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:tip: .tip}

# Integrate third-party services using hooks
{: #hooks}

You can use hooks to easily integrate third-party services in the SDK for Node.js buildpack. Note that IBM does not provide support for any third-party services that you integrate. For more information about support, see [Third-party services](/docs/cloud-foundry?topic=cloud-foundry-buildpack_support_statement).

The SDK for Node.js buildpack includes the Dynatrace hook. Dynatrace enables application monitoring of Node.js applications. Learn more about using the Dynatrace hook in the buildpack in the [Dynatrace documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")]( https://www.dynatrace.com/support/help/cloud-platforms/cloud-foundry/application-only/deploy-oneagent-on-cloud-foundry-for-application-only-monitoring/){: new_window}.


When following instructions in third-party documentation, remember to use the `ibmcloud cf` command instead of `cf` to run commands for your {{site.data.keyword.Bluemix_notm}} buildpack.
{: tip}
