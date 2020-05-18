---

copyright:
  years: 2018, 2019
lastupdated: "2019-11-20"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}

# Monitoring
{: #monitoring}

Monitoring an {{site.data.keyword.cfee_full}} instance and its supported infrastructure is supported by an open-source toolset consisting of Prometheus and Grafana. The solution enables you to analyze, visualize and manage alerts for metrics in the Cloud Foundry environment. There are three web consoles from which monitoring takes place: A Grafana console, a Prometheus console, and a Prometheus Alert Manager console.

Starting with CFEE v2.2.0, monitoring tools are not enabled by default when an environment is created. Administrators can enable monitoring after the environment is created. In the CFEE user interface, go to the _Monitoring_ page in the left navigation pane and click **Enable**. Enabling monitoring automatically provisions additional Kubernetes worker nodes (in the same IBM Cloud account) and installs the monitoring tools into those worker nodes.

**Note:** Access to the monitoring capability in an {{site.data.keyword.cfee_full}} instance requires an _Administrator_ or _Editor_ role in the CFEE (in additon to _Viewer_ role in the resource group under which the CFEE is grouped). The default name of the Kubernetes cluster supporting a CFEE instance is _`<CFEEname>`-cluster_.

## Prometheus
{: #prometheus}

Prometheus is an open-source systems monitoring and alerting toolkit. Since its inception in 2012, many companies and organizations have adopted Prometheus, and the project has a very active developer and user community.
It is a standalone open source project and maintained independently of any company. To emphasize this, and to clarify the project's governance structure, Prometheus joined the Cloud Native Computing Foundation in 2016 as the second hosted project, after Kubernetes. See the [Prometheus documentation](https://prometheus.io/docs/introduction/overview/) for more information.

The Prometheus ecosystem consists of multiple components, many of which are optional:

* The main Prometheus server which scrapes and stores time series data.</li>
* [Alertmanager](https://prometheus.io/docs/alerting/alertmanager/) to handle alerts.</li>
* Various special-purpose exporters like node exporter, blackbox exporter, etc.</li>
* A push gateway for supporting short-lived jobs.</li>

Prometheus gathers metrics from instrumented jobs, either directly or via an intermediary push gateway for short-lived jobs. It stores all gathered samples locally and runs rules over this data to either aggregate and record new time series from existing data, or to generate alerts.

### Customizing the Prometheus Alertmanager configuration (optional)

Alertmanager has a default configuration file that defines the policies for handling, grouping, and notification routing of Alertmanager alerts. You can download the default configuration file and upload a custom configuration in the **Configuration** tab of the _Monitoring_ page.

You can refer to the [Alertmanager configuration](https://prometheus.io/docs/alerting/configuration/) documentation for details on how to configure notification for email, Slack, PagerDuty, etc.

### Federation of the Prometheus server
{: #federation}

Prometheus provides the concept of
[federation](https://prometheus.io/docs/prometheus/latest/federation/)
which allows one Prometheus server to scrape metrics data from another Prometheus server.
This concept allows you to integrate the Prometheus server in your
{{site.data.keyword.cfee_full}} instance with a Prometheus server you operate by yourself.

By default, the `/federate` endpoint of the built-in Prometheus server is only reachable
within the Kubernetes cluster. Any external access using the hyperlinks on the
_Monitoring_ page of the CFEE user interface will result in a
`404 Unauthorized` response. To make the Prometheus `/federate` endpoint externally accessible, a basic
solution is to create a Kubernetes Service and an Ingress object.

In order to do so, you need to have access to your Kubernetes cluster (see steps 1 through 7 in
[Accessing the Kubernetes Cluster](/docs/cloud-foundry?topic=cloud-foundry-monitoring#access-the-kubernetes-cluster)).
On your workstation, create configuration files for the Service and Ingress
objects. Consider the following as a basic example.

For the Service object, create file `service.yaml` with content similar to the following:
```
apiVersion: v1
kind: Service
metadata:
  annotations:
  labels:
    app: prometheus
    component: server
  name: prometheus-federation
  namespace: monitoring
spec:
  ports:
  - name: http
    port: 80
    protocol: TCP
    targetPort: 9090
  selector:
    app: prometheus
    component: server
  type: ClusterIP
status:
  loadBalancer: {}
```
{: pre}

For the Ingress object, create file `ingress.yaml` with content similar to the following.
You will need to adopt your CFEE instance's `domainname` in the appropriate place:
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  labels:
    app: prometheus-federation
  name: prometheus-federation
  namespace: monitoring

spec:
  rules:
  - host: federation.<domainname>
    http:
      paths:
      - backend:
          serviceName: prometheus-federation
          servicePort: 80
        path: /federate
```
{: pre}

Once you are satisfied with the configurations, you can create the respective Kubernetes objects by executing:
```
$ kubectl apply -f service.yaml
```
{: pre}
```
$ kubectl apply -f ingress.yaml
```
{: pre}

**Note:** These configuration files provide a very basic example. You
may want to add TLS configuration to the Ingress to support the `https` scheme.
Refer to the [Kubernetes documentation](https://kubernetes.io/docs/home/)
for detailed information on Kubernetes objects, and how to manage them.

## Monitoring with Sysdig (optional)
{: #sysdig}

IBM Cloud Monitoring with Sysdig is an offering which provides a fully-managed, enterprise-grade monitoring service on IBM Cloud.

After you enable monitoring on an {{site.data.keyword.cfee_full}}, you have the choice to use Sysdig or Prometheus or both.

If you want to use Sysdig, you need to do some additional steps to connect your Sysdig instance with your CFEE.
These steps are:

* Create your own Sysdig instance if it does not already exist. You can refer to the [Sysdig](https://cloud.ibm.com/docs/Monitoring-with-Sysdig?topic=Sysdig-about) documentation for details.
* Deploy the Sysdig agent on the CFEE environment. You can get the complete command on your Sysdig instance or refer to the [Sysdig agent configuration](https://cloud.ibm.com/docs/Monitoring-with-Sysdig?topic=Sysdig-config_agent) documentation for details.
 
After your CFEE cluster is connected to your Sysdig instance, you will find common Kubernetes-specific metrics as well as Cloud Foundry related metrics in your Sysdig instance.
Cloud Foundry related metrics are identified by the following prefixes:

* `firehose_`: metrics provided by the firehose_exporter component
* `cfapitester_` : metrics provided by the Cloud Foundry API Tester tool

With this enablement you can now create your own Sysdig alerts and Sysdig dashboards based on the available metrics for all Cloud Foundry components.

## Grafana
{: #grafana}

Grafana is an open-source analytics platform for all metrics collected by Prometheus. The deployed Grafana version on your cluster is already configured to use the underlying Prometheus database. It also contains some valuable Grafana dashboards. See the [Grafana documentation](http://docs.grafana.org/guides/getting_started/) for more information.

Data from individual panels can be exported from Grafana in CSV format. See the [Grafana Export Panel Data documentation](https://grafana.com/docs/reference/sharing/#export-panel-data) for more information.

## Accessing the Kubernetes cluster: Port forwarding
{: #access-the-kubernetes-cluster}

Accessing the Kubernetes cluster is **not required** for instances of CFEE **v3.2.0 or later**. If your CFEE instance is v3.2.0 or later, skip to [Launching the monitoring consoles](/docs/cloud-foundry?topic=cloud-foundry-monitoring#launching-consoles)
{:important}

Accessing the monitoring tools in instances prior to CFEE v3.2.0 requires to **forward the ports** of the Prometheus, Prometheus AlertManager, and Grafana servers. This is done through the Kubernetes CLI.
The following guides you through the steps for installing the required CLI's, forwarding the server ports, and launching the consoles.

**Note:** The following instructions are also available in the {{site.data.keyword.cfee_full}} user interface. Open the CFEE instance user interface, click **Monitoring** in the left navigation pane, and go to the **Access** tab to see the instructions.

1. Check your [Access Policies](https://cloud.ibm.com/iam/#/users) to ensure that you have at least a Viewer role on the Kubernetes cluster supporting the environment.
2. Install the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cli-install-ibmcloud-cli#install_use).
3. Install the [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/). If you have an existing Kubernetes CLI, we recommend that you install the latest version.
4. Install the container service plug-in:
```
    ibmcloud plugin install container-service
```

5. Log into your IBM Cloud account:

  ```
  ibmcloud login -a https://cloud.ibm.com
  ```
  {: pre}

  If you have a federated ID, use __ibmcloud login -sso__ to log into the IBM Cloud CLI.

6. Target the IBM Cloud Container Service region in which you want to work (e.g., us-south):

  ```
  ibmcloud cs region-set us-south
  ```
  {: pre}

7. Set the context of the cluster in your cli:

  a. Get the command to set the environment variable and download the Kubernetes configuration files:

  ```
  ibmcloud cs cluster-config <CFEE_instance_name>-cluster
  ```
  {: pre}

  b. Set the KUBECONFIG environment variable. Copy the output from the previous command and paste it in your terminal. The command output should look similar to the following:
  __export KUBECONFIG=/Users/$USER/.bluemix/plugins/container-service/clusters/cf-admin-0703-cluster/kube-config-dal10-cf-admin-0703-cluster.yml__

8. Forward server ports. Set up port-forwarding in the Kubernetes cluster for the pods running Prometheus, AlertManager and Grafana. This will enable you to host the monitoring metrics by proxy on your local machine (localhost):

  ```
  sh -c 'kubectl -n monitoring port-forward deployment/prometheus-server 9090 & kubectl -n monitoring port-forward deployment/prometheus-alertmanager 9093 & kubectl -n monitoring port-forward deployment/grafana 3000 &''
  ```
  {: pre}

## Cloud Foundry API Tester
{: #cfapitester}

Cloud Foundry API Tester (cfapitester) is a part of the monitoring solution which allows the analysis of [Cloud Foundry CLI](https://docs.cloudfoundry.org/cf-cli/cf-help.html) calls. The goal is to have a detailed timing analysis of all process steps and rest calls which are executed during a single [Cloud Foundry CLI](https://docs.cloudfoundry.org/cf-cli/cf-help.html) command such as `cf push ..`, `cf buildpacks` etc.
The tool works as a [Cloud Foundry CLI](https://docs.cloudfoundry.org/cf-cli/cf-help.html) wrapper and examines the resulting trace.
Results are detailed metrics which could be used for problem determination, alerting and visualization.

### Changing default configuration for Cloud Foundry API Tester

Cloud Foundry API Tester (cfapitester) will be enabled in CFEE when you enable monitoring. It will be deployed with a default configuration that can be modified post enablement.

The following two configuration environment variables can be used to adapt your buildpacks which are used:

  - CAT_TEST_DELAY
  This environment variable allows you to disable/enable buildpack testing and to set the interval in which tests are executed. When it is set to `0` buildpacks testing will be disabled. When it is set to a value > 0 it will calculate the interval used for buildpack test cycles in seconds.

  - CAT_TEST_FILTER
  This environment variable allows you to specify the amount of buildpacks to be tested. When this value is set to `all` or is not set, all buildpacks will be tested. When this value is set to specific buildpacks then only those buildpacks will be used.

The following buildpack tests are available:
  - `ruby_buildpack, dotnet-core, binary_buildpack, go_buildpack,nodejs_buildpack, php_buildpack, python_buildpack, ruby_buildpack, swift_buildpack, java_buildpack, liberty-for-java,xpages_buildpack, sdk-for-nodejs, staticfile_buildpack`
  The test for `ruby_buildpack` also contains the Dora app for cf acceptance tests - see [Dora app details ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/cf-acceptance-tests/tree/master/assets/dora). For more details about buildpacks - see [Developing with buildpacks]{/docs/cloud-foundry?topic=cloud-foundry-available_buildpacks}.

To change the default test configuration you can modify the two variables listed above:
1. Before you begin, please make sure that you have installed [Helm ![External link icon](../icons/launch-glyph.svg "External link icon")](https://helm.sh) on your local machine. See [Installing Helm]{/docs/containers?topic=containers-helm} for more details about this tool.
2. You also need to install one useful Helm plugin `update-config`. If the `update-config` plugin is already installed, make sure that the installed plugin version is >= 0.5.3. Otherwise remove the old plugin version first using the following command:

  ```
  helm plugin remove update-config
  ```
  {: pre}  
And install the plugin again:
  ```
  helm plugin install https://github.com/bluebosh/helm-update-config
  ```
  {: pre}


3. Find your cluster in https://cloud.ibm.com/resources. It usually looks like `<your CFEE name>-cluster`.
4. Click the cluster and follow the instruction in `Access` tab to setup the connection to your cluster.
5. As tiller is deployed in a secured way, create a temporary directory and cd into this directory.
6. Then execute the following command which will create the necessary environment variable for the secured tiller access:

  ```
  kubectl get secret helm-secret -n kube-system -o json | jq -r '.data."helm.cert"' | base64 --decode > helm.cert.pem && \
  kubectl get secret helm-secret -n kube-system -o json | jq -r '.data."helm.key"' | base64 --decode > helm.key.pem && \
  kubectl get secret helm-secret -n kube-system -o json | jq -r '.data."ca.cert"' | base64 --decode > ca.cert.pem && \
  helm_cert_path="helm.cert.pem" && \
  helm_key_path="helm.key.pem" && \
  ca_cert_path="ca.cert.pem" && \
  export tls_params="--tls --tls-ca-cert ${ca_cert_path} --tls-cert ${helm_cert_path} --tls-key ${helm_key_path}"
  ```
  {: pre}

7. Check the actual values for Cloud Foundry API Tester:

  ```
  helm get values ${tls_params} cf-api-tester
  ```
  {: pre}

  You see the values under `env` section. As example:

  ```
  env:
  cat_test_delay: 300
  cat_test_filter: binary_buildpack,staticfile_buildpack
  ```
  {: screen}

8. To change the values use following command:

  ```
  helm update-config ${tls_params} cf-api-tester --set-value env.cat_test_delay="<fill in your value>",env.cat_test_filter="<fill in your value>"
  ```
  {: screen}
  If you want to change `cat_test_filter` to a list of buildpacks use following format in the command above. As example:
  ```
  env.cat_test_filter="binary_buildpack\,staticfile_buildpack"
  ```
  {: screen}

9. Wait few minutes till the appropriate pod in namespace `monitoring` has been recreated with new values.


## Launching the monitoring consoles
{: #launching-consoles}

You can launch the **Grafana**, **Prometheus** and **Prometheus Alertmanager** consoles. Go to the _Monitoring_ page of the CFEE user interface and click on the corresponding hyperlink. Each console is opened in a separate browser tab.

### Grafana dashboards

There is a default set of Grafana dashboards included in the CFEE instance. Those default dashboards are interactive and give you a view of the infrastructure used to host your CFEE instance. (Kubernetes cluster). Once you launch the Grafana console, click the **Home** button at the top of the Grafana console to select one of the pre-deployed dashboards (see list below), which will graph the corresponding metrics:

   For CFEE v2.1.0 or earlier there is a default `admin` user in Grafana, with the default password set to `admin`. We recommend to login with Userd/Password `admin/admin`, and change them to new credentials. For CFEE v2.2.0 or later users are authenticated automatically with their IBM ID credentials.

   The following default dashboards are provided with the CFEE instance and are available from the _Home_ dropdown.

#### Cloud Foundry dashboards

   - _CF: Cells Capacity_
        - Shows the general status of the Cloud Foundry cells where the Cloud Foundry applications are deployed.
   - _CF: Diego Cell dashboard_
        - Shows the status of the Cloud Foundry cells and Diego components.
   - _CF: Router_
        - Shows the Cloud Foundry router status running on your CFEE environment.

#### Dashboards for the Kubernetes infrastructure supporting your CFEE environment

   - _Deployment_
        - Shows the status of your Kubernetes deployments.
   - _Kubernetes Cluster Health_
        - Shows the health of the Kubernetes cluster.
   - _Kubernetes Cluster Status_
        - Shows the status of the Kubernetes cluster.
   - _Kubernetes Resource Requests_
        - Shows the used CPU, memory and other parameters of the Kubernetes cluster.
   - _Pods_
        - Shows details for each pod running on the Kubernetes cluster.
   - _Replica Set_
        - Shows the status of the Kubernetes replica sets.
   - _Worker Nodes_
        - Shows details for each worker node of the Kubernetes cluster.
   - _Worker Nodes Overview_
        - Shows the CPU and memory usage of the kubernetes infrastructure, along with its network traffic.

#### Cloud Fondry API Tester Dashboards

   - _CfApiTester - Runtimes_
        - Provides a general status about Cloud Foundry application provisioning tests. It allows to the get the duration and test result of `cf push <app>` commands related to the used buildpack and gives an overview about the command duration and the duration of the most important sub steps of the command.
   - _CfApiTester - Internal Metrics_
        - Gives an overview about CfApiTester's Container health including information about cpu/memory/disk usage, count of processes, threads, traces etc., mapper errors and thrown exception during the test execution. Mapper errors and thrown exception are only warnings and expected as both are dependent from the used [Cloud Foundry CLI](https://docs.cloudfoundry.org/cf-cli/cf-help.html).

## CFEE Monitoring on IBM Virtual Cloud Private (VPC)
{: #MonitoringVPC}

For a VPC {{site.data.keyword.cfee_short}}, there must be two (2) unallocated IPs in each of the VPC subnets in the {{site.data.keyword.cfee_short}}.

Monitoring on VPC does not support persistence for Prometheus and Grafana:
1. Reload of the Prometheus pod/container will lose collected time series data history. If you need historical data you can federate Prometheus as described [here](cloud-foundry?topic=cloud-foundry-monitoring#federation).

2. Reload of the Grafana pod/container will lose any customized Grafana dashboards. It is recommended to backup all customized Grafana dashboards to enable restore after pod/container reload.
