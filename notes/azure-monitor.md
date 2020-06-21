## Azure Monitor

Azure Monitor maximizes the availability and performance of your applications by delivering a comprehensive solution for collecting, analyzing, and acting on telemetry from your cloud and on-premises environments. It helps you understand how your applications are performing and proactively identifies issues affecting them and the resources they depend on.  

Configuring Azure Monitoring Metrics or Azure Monitoring Insights will give you performance data that you require for the VM.Azure Monitoring Alerts is configured to give you alerts rather than metrics.Azure VM Boot Diagnostics gives you diagnostics into VM boot issues or crashes.  
You can configure a Azure Storage Account as a sink.  

You cannot configure a sink to Application Insights for application layer information.  
  
### Key Capabilities  
- Monitor and visualize metrics  
- Query and analyze log  
- Setup alerts and actions  
- General  
- Contains log, metrics, and traces  
- Collects from Application, Guest OS, Resource, Subscription, and Tenant  
- Monitoring solutions may require both Log Analytics Workspace and Automation account if using Runbooks
  
### Data Source -  

 - Application monitoring data -   Guest OS monitoring data -   Azure
   resource monitoring data (Data about the operation of an Azure
   resource.)   
  - Azure subscription monitoring data - (Data about the
   operation and management of an Azure subscription, as well as data
   about the health and operation of Azure itself.)   
   - Azure tenant  - monitoring data (Data about the operation of tenant-level Azure
   services, such as Azure Active Directory.)

### You can enable Diagnostics  (Under resource settings)
 
 - Enable guest-level monitoring   
 - Performance counters: collect performance data   
  - Event Logs: enable various event logs   
  - Crash Dumps: enable or disable   
  - Sinks: send your diagnostic data to other services for more analysis   
  - Agent: configure agent settings

### Tools you may use for visualizing data  

- Dashboards  
- Views  
- Power BI

### You can create alert rules for these items and more:  
  
 - Metric values   
 - Log search queries   
 - Activity log events   
 - Health of  the underlying Azure platform   
 - Tests for website availability

It leverages the powerful data analysis platform in Log Analytics to provide you with deeper insights into your application's operations.  
  
Azure Monitor for containers is a service that is designed to monitor the performance of container workloads, which are deployed to managed Kubernetes clusters, hosted on Azure Kubernetes Service (AKS).  
  
Azure Monitor for VMs is a service that monitors your Azure VMs at scale, by analyzing the performance and health of your Windows and Linux VMs (including their different processes and interconnected dependencies on other resources, and external processes).

Autoscale. Azure Monitor uses Autoscale to ensure that you have the right amount of resources running to manage the load on your application effectively.
  
All the application log data that Application Insights collects is stored in a workspace that Azure Monitor can access. You'll then have a central location to monitor and analyze the health and performance of all your applications.  
  
Other services like Security Center also rely on Azure Monitor. Security Center, for example, collects security-related data from your virtual machines and other resources. Security Center stores this data in a workspace that you can access from Azure Monitor.  
  
### You use Azure Monitor if:  
  
- You need a single solution to help you collect, analyze, and act on log data from both cloud and on-premises.  
- You're using services such as Azure Application Insights and Azure Security Center. Those services store their collected data in workspaces for Azure Monitor. You can then use Azure Monitor Log Analytics to interactively query the data.  
  
### Three signal types that you can use to monitor your environment:  
  
- Metric alerts provide an alert trigger when a specified threshold is exceeded. For example, a metric alert can notify you when CPU usage is greater than 95 percent.
- Activity log alerts notify you when Azure resources change state. For example, an activity log alert can notify you when a resource is deleted.
- Log alerts are based on things written to log files. For example, a log alert can notify you when a web server has returned a number of 404 or 500 responses.

The following alert capabilities aren't yet available for the generation of monitoring data:  

- Service health alerts based on activity logs  
- Web availability tests through Application Insights

﻿The Azure diagnostics extension is an agent that runs inside a VM instance. The agent monitors and saves performance metrics to Azure storage. These performance metrics contain more detailed information about the status of ﻿the VM, such as AverageReadTime for disks or PercentIdleTime for CPU. You can create autoscale rules based on a more detailed awareness of the VM performance, not just the percentage of CPU usage or memory consumption.