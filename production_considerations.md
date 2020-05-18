---

copyright:
  years: 2016, 2019
lastupdated: "2019-09-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Best practices for running apps in production
{: #production}

<!-- This file is reused in the CF Public subcollection. -->

Cloud Foundry is an excellent platform for running cloud-ready applications in production. These best practices can help with running production Cloud Foundry apps with {{site.data.keyword.Bluemix}}.
{:shortdesc}

## Multiple instances
{: #multiple_instances}

Cloud-ready apps are horizontally scalable, and are therefore able to dynamically add or remove application instances. Run at least three instances of each application to help avoid unexpected downtime in the event of isolated incidents or platform maintenance.

## Monitoring options
{: #monitoring-bps}

{{site.data.keyword.Bluemix_notm}} makes it easy to monitor your application with services like [Monitoring and Analytics](/docs/cloud-monitoring?topic=cloud-monitoring-getting-started) and [New Relic ![External link icon](../icons/launch-glyph.svg)](http://newrelic.com/){: new_window}. Check out [Integrated logging capabilities](/docs/cloud-foundry-public?topic=cloud-foundry-public-monitoring_logging_cloud_foundry_apps) for more information.

## Support options
{: #support}

{{site.data.keyword.Bluemix_notm}} paid pricing plan offers a number of different account types with optional paid support.  No matter the type of your account, if you plan to bring an application to production on {{site.data.keyword.Bluemix_notm}}, consider enrolling in this option.

With or without paid support, you can get help as described at [Getting support](/docs/get-support?topic=get-support-getting-customer-support), but paying for support will help  ensure that your support tickets will be given the level attention that they deserve.
