---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Reviewing resource usage

Administrators and developers can see how the resource capacity (memory and CPU) of an CFEE is used by applications and cells. To monitor resource usage in a CFEE instance:

1. Go to the [{{site.data.keyword.Bluemix_notm}} Cloud Foundry dashboard](https://cloud.ibm.com/resources?filter=cf_environments) and open the {{site.data.keyword.cfee_full_notm}} where you want to manage resource usage.
2. In the {{site.data.keyword.cfee_full_notm}} user interface, go to the **Resource Usage** entry in the left navigation pane to open the _Resource Usage_ page. Under the _Resource Usage_ page you can either go to the _Applications_ or the _Cells_ sub-entries to open the corresponding pages.  The information displayed in the _Applications_ and _Cells_ pages can be considered two ways of breaking down resource usage:
   * The **Applications** page analyzes resource usage by applications aggregated across instances.
   * The **Cells** page shows resource usage of application instances running on specific cells. The pattern of resource usage across cells can provide insights into capacity and load distribution.  For example, it may help identify problems with the load balancing of applications (e.g., large differences in the resources used by an application across cells) or resource usage approaching overall capacity (i.e., large percentages of resource usage in all cells).

**Note:** Resource usage data represents the usage of resources during the latest 5 minute collection cycle. Resource usage data can be refreshed by clicking **Refresh data** at the top of the page.

## Applications
{: #usage_apps}

The Applications page has two sections:
1. Horizontal bar charts showing physical and reserved memory usage.
   * Reserved and physical memory used by the **selected apps** in the table below.
   * Reserved and physical memory used by **all the apps** in the CFEE instance.
   * Physical memory used by the **system**, which includes memory used by the Cloud Foundry control plane, and application memory cached by the Cloud Foundry control plane.
   * **Total available** reserved and physical memory in the CFEE instance.
   
   **Note:** When applications reserve more memory than there is physically available, a dotted red outline in the reserved memory bar chart is shown to represent the amount of memory overreserved by the selected application(s).

   To show the percentage of memory and CPU used by all the apps or those selected in the table, hover over the corresponding portion of the chart.  Hovering over the *all apps* portion of the chart shows the percentage of memory and CPU relative to the total available. Hovering over the *selected apps* portion of the chart shows the percentage of memory and CPU relative to the total available memory or CPU.

2. A table listing all the applications.  Each row in the table displays resource information for that application.  Expanding a row shows resource usage information for the various instances of that application.

  The first column in the table is a checkbox that determines if the corresponding application is to be included in the set of *Selected Apps* to be included in the chart at the top of the page. To include (or exclude) an application in the selected set, click on the corresponding checkbox.  When an application is selected or unselected for inclusion in the set, the graph is refreshed.  The _Selected Apps_ legend to the right of the bar chart indicates (in parenthesis) the number of applications currently selected for inclusion in the graph.

  The following information is displayed as columns in the applications table:
   * **Application Name**: the name of the application or application instance. If the user does not have permission to access the application, the application GUID is displayed instead.
   * **Instances**: For an application it represents the number of running instances.  For an application instance (shown below the application name when the application row is expanded) it represents the name of the cell where the application instance runs.
   * **Total Memory-Physical**: MBs of physical memory used by all the instances of an application.  The physical memory used by an application equals the sum of the physical memory used by all the application's instances.
   * **Total Memory-Reserved**: MBs of memory reserved by all the instances of an application.  The memory reserved by an application equals the sum of the memory reserved by all the application's instances.
   * **Average CPU (% of cell)**: For application instances, the metric represents the average CPU **within the cell in which it runs**.  For an application, it represents CPU usage averaged acrossed its instance averages.
   * **Max CPU (% of cell)**: For application instances, the metric represents the maximum CPU used by that instance **within the cell in which it runs**.  For an application, it represents the highest of its instances maximum CPU usage.
   * **CPU (% of system)**: The percentage of total CPU available in the CFEE used by an application and its instances.  The CPU used by an application equals the sum of CPU percentages used by all the application's instances.
   * **Requests**: The number of requests to an application during the latest data collection cycle (five minutes).
   * **Organization**: The organization where the application is deployed. If the user is not a member of the organization, the organization GUID is displayed instead.

Expand an application row in the table to see the list of application instances and their corresponding resource usage metrics.

### Filtering applications
You filter the content of the table through the **Applications** and **Organizations** filter dropdowns located above the table.

Additionally, you can use the filter entry field above the table to show only applications maching the string you enter in the filter field.  The filter is reflected in both, the table and the chart above it.

**Note:** The filters work independently of the table rows selected (through the checkbox in the first column table) for inclusion in the set of _Selected Apps_ included in the chart above. For example,  if there is a total of ten applications in the CFEE environment, and five of them are selected for inclusion in the chart, when you apply a filter that matches only one application instance, only that application instance will appear in the table.  Furthermore, the set of _Selected Apps_ will now include only that matching application, and the chart will be refreshed accordingly.  When you remove the filter, all ten applications will be shown again in the table, and the set of _Selected Apps_ will be reset to include all the apps.


## Cells
{: #usage_cells}

The Cells page has two sections:
1. Vertical bar charts showing:
   * *Total available* memory and CPU available in the CFEE instance.
   * Memory and CPU used by the **Selected app instances** in the CFEE instance.
   * Memory and CPU used by the **All app instances** in the CFEE instance.
   * Memory and CPU used by the **System** in the table below.  The system usage represents the resource used by the CFEE service componentry plus application cache.
   * Total Memory and CPU available in the Cell. The **Total Cell** equals the total (memory or CPU) available in the Kubernetes worker node (where the cell is provisioned) minus what is used by the system.

   To show the percentage of memory or CPU used by all the app instances or by the app instances selected in the table, hover over the corresponding portion of the chart.  Hovering over the chart bar shows the absolute memory or CPU available, the memory or CPU used by all the apps, and the memory or CPU used by system+cache.  It also shows the percentage of memory or CPU used by all apps and by system+cache relative to the total available.

2. A table listing all the cells and applications instances running in them.  Each row in the table displays resource information for that cell and application instance.

  The first column in the table is a checkbox that determines if the corresponding application instance is to be included in the set of *Selected App instances* to be included in the chart at the top of the page. To include (or exclude) an application instance in the selected set, click on the corresponding checkbox.  When an application instance is selected or unselected for inclusion in the set, the graph is refreshed.

  The following information is displayed as columns in the table:
   * **Cell name**: The name of the cell.
   * **Application name**: The name of the application running on the cell. If the user does not have permission to access the application, the application GUID is displayed instead.
   * **Instances**: The number of the application instances running on the cell.
   * **Memory-Physical**: MBs of physical memory used by the application instance running in the cell.
   * **Memory-Reserved**: MBs of memory reserved by the application instance running in the cell.
   * **CPU (% )**: The percentage of total CPU available in the CFEE used by the application instance running on the  cell.

### Filtering cells and app instances
You can filter the content of the table through the **Cells** filter dropdown located above the table and selecting the cells to be displayed in the table.

Additionally, you can use the filter entry field above the table to show only applications instances maching the string you enter in the filter field.  The filter is reflected in both, the table and the chart above it.

**Note:** The filters work independently of the table rows selected (through the checkbox in the first column table) for inclusion in the set of _Selected App Instances_ included in the chart above. For example,  if there is a total of ten application instances in the CFEE environment, and five of them are selected for inclusion in the chart, when you apply a filter that matches only one application instance, only that application instance will appear in the table.  Furthermore, the set of _Selected App Instances_ will now include only that app instance, and the chart will be refreshed accordingly.  When you remove the filter, all ten application instances will be shown again in the table, and the set of _Selected App Instances_  will be reset to include all app instances.

## Memory metrics
{: #memory_metrics}

There are various types of memory metrics available in CFEE.  This variety of memory metrics stems from the various perspectives from which memory utilization can be analyzed. Memory utilization can be measured relative to the capacity available to the Cloud Foundry cell(s), or relative to the total physical capacity of the Kubernetes worker node into which cells are provisioned (which may include other memory utilization unrelated to Cloud Foundry). Memory capacity can also be seen from the perspective of the memory reserved (allocated) by applications, or from the perspective of the memory actually used by those applications.  

Following are the memory metrics available to users of a CFEE instance:

* Overall memory used by the Kubernetes worker nodes supporting the Cloud Foundry cells relative to the total memory capacity of those nodes.  This reflects worker node memory used not only by Cloud Foundry cells, but also by system or cache  overhead unrelated to Cloud Foundry.  This is shown as **Overall usage** gauge in the **Overview** page.
* Memory allocated to applications relative to the available cell memory, irrespective of that memory actually being used or not. This is shown as **Allocated** gauge in the **Overview** page.
* Memory physically used by applications relative to the available cell memory. The overall application memory used is shown in the **App usage** gauge in the **Overview** page. The memory used by specific applications is shown in the **Resources Usage - Applications** page.
* Memory used by Cloud Foundry cells relative to the memory capacity of the worker node.  This is shown in the **Resources Usage - Cells ** page.
* Total memory used by worker nodes (related or not to Cloud Foundry). This is shown in the **Updates and Scaling** page for both, nodes supporting the CFEE cells and  nodes supporting the CFEE control plane.
* Additional memory metrics are shown in Grafana dashboards, which can be launched from the **Monitoring** page.  These metrics show detailed memory capacity and usage for Cloud Foundry cells and Kubernetes cluster and worker nodes.
