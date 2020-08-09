# Azure Monitor

Azure Monitor maximizes the availability and performance of your applications by delivering a comprehensive solution for collecting, analyzing, and acting on telemetry from your cloud and on-premises environments. It helps you understand how your applications are performing and proactively identifies issues affecting them and the resources they depend on.
- Configuring Azure Monitoring Metrics or Azure Monitoring Insights will give you performance data that you require for the VM.
- Azure Monitoring Alerts is configured to give you alerts rather than metrics.
- Azure VM Boot Diagnostics gives you diagnostics into VM boot issues or crashes.
- You can configure a Azure Storage Account as a sink.
- You cannot configure a sink to Application Insights for application layer information.

**General**
- Contains log, metrics, and traces
- Collects from Application, Guest OS, Resource, Subscription, and Tenant
- Monitoring solutions may require both Log Analytics Workspace and Automation account if using Runbooks

﻿The Azure diagnostics extension is an agent that runs inside a VM instance. The agent monitors and saves performance metrics to Azure storage. These performance metrics contain more detailed information about the status of ﻿the VM, such as AverageReadTime for disks or PercentIdleTime for CPU. You can create autoscale rules based on a more detailed awareness of the VM performance, not just the percentage of CPU usage or memory consumption.
﻿
### Data Source

- Application monitoring data -
- Guest OS monitoring data -
- Azure resource monitoring data (Data about the operation of an Azure resource.)
- Azure subscription monitoring data (Data about the operation and management of an Azure subscription, as well as data about the health and operation of Azure itself.)
- Azure tenant monitoring data (Data about the operation of tenant-level Azure services, such as Azure Active Directory.)

You can extend the data you're collecting into the actual operation of the resources by enabling diagnostics and adding an agent to compute resources. Under resource settings, you can enable Diagnostics
- Enable guest-level monitoring
	-- Performance counters: collect performance data
	-- Event Logs: enable various event logs
	-- Crash Dumps: enable or disable
	-- Sinks: send your diagnostic data to other services for more analysis
	-- Agent: configure agent settings

 **Application Insights is a service** that monitors the availability, performance, and usage of your web applications, whether they're hosted in the cloud or on-premises. It leverages the powerful data analysis platform in Log Analytics to provide you with deeper insights into your application's operations.

**Azure Monitor for containers** is a service that is designed to monitor the performance of container workloads, which are deployed to managed Kubernetes clusters, hosted on Azure Kubernetes Service (AKS).

**Azure Monitor for VMs is a service** that monitors your Azure VMs at scale, by analyzing the performance and health of your Windows and Linux VMs (including their different processes and interconnected dependencies on other resources, and external processes).

**Autoscale**. Azure Monitor uses Autoscale to ensure that you have the right amount of resources running to manage the load on your application effectively.

![Azure Monitor | Microsoft Azure](https://azurecomcdn.azureedge.net/cvt-055e2ba73dcb82f515a437f87a8976c8fe966152c026a85d3a16a947e8890858/images/page/services/monitor/diagram.jpg)

Tools you may use for visualizing data, for particular audiences and scenarios, include:

Dashboards
Views
Power BI

Azure Monitor is the service for collecting, combining, and analyzing data from different sources.

All the application log data that Application Insights collects is stored in a workspace that Azure Monitor can access. You'll then have a central location to monitor and analyze the health and performance of all your applications.
﻿
**Key Capabilities**
Monitor and visualize metrics
Query and analyze log
Setup alerts and actions

![](https://data3.com/wp-content/uploads/2017/09/End2End_Bala_5-800x348.jpg)

**Monitor**
Usage And Estimated Costs blade in Azure Monitor presents a wonderful summary of your Azure consumption and spend over the past 31 days. If you’re in search of more historical data, then browse in the Azure portal to the Cost Management + Billing blade instead.

Configure diagnostics for Azure resources to create performance baselines and to analyze your consumption.

General
Most metrics retained for 93 days for most metrics
Log-based metrics inherit retention of the Log Analytics workspace
Application Insight logs are retained for 90 days
Aggregate on min, max, count, sum and adjust time span
Two metrics can be monitored in a single rule
Log data can be converted to metrics

Components
Platform Metrics - Collected from Azure resources at one minute frequency
Guest OS Metrics - Agent-based metrics retrieved from VM using Windows Diagnostic Extension or InfluxData Telegraf Agent
Application Metrics - Created by application insights
Custom Metrics - defined with application via Application Insights or via API

**Azure Application Insights**

 You use Azure Application Insights to monitor and manage the performance of your applications. Application Insights automatically gathers information related to performance, errors, and exceptions in applications. You also use Application Insights to diagnose what has caused the problems that affect an application.

Criteria for assessing Application Insights
You use Application Insights if:

You want to analyze and address issues and problems that affect your application's health.
You want to improve your application's development lifecycle.
You want to analyze users' activities to help understand them better.


**Log Analytics**

General
Log data is stored in a Log Analytics Workspace
Application Insights stores application log data in a separate workspace for each application
Cross-resource queries can be used to query across workspaces

Application Monitoring
Application Insights monitors availability, performance, and usage of web apps running in Azure or on-premises
Azure Monitor for Containers monitors container workloads running in Azure Kubernetes Service
Azure Monitor for VM
Configure which OS event logs and metrics are logged to the workspace in Advanced Settings -> Data
Union joins other tables and workspaces

Key Tables
Event -> Windows Event Logs
Heartbeat -> Agent communication
_CL -> Text file on Win/Lin

Alert
Perf
W3CIISLogs -> IIS Logs
Key Queries
Event | union Syslog | where EventLevelName == "Error" or SeverityLevel == "Error"
Heartbeat | summarize count() by Computer, bin(TimeGenerated, 5min)
  ![Optimizing IT Operations Using Azure Monitor and Log Analytics ...](https://media.springernature.com/original/springer-static/image/chp%3A10.1007%2F978-1-4842-4910-9_6/MediaObjects/472358_1_En_6_Figau_HTML.jpg)

You use Azure Monitor if:

- You need a single solution to help you collect, analyze, and act on log data from both cloud and on-premises.
- You're using services such as Azure Application Insights and Azure Security Center. Those services store their collected data in workspaces for Azure Monitor.
- You can then use Azure Monitor Log Analytics to interactively query the data.

**Three signal types** that you can use to monitor your environment:

- **Metric alerts** provide an alert trigger when a specified threshold is exceeded. *For example, a metric alert can notify you when CPU usage is greater than 95 percent.*
- **Activity log alerts** notify you when Azure resources change state. *For example, an activity log alert can notify you when a resource is deleted.*
- **Log alerts** are based on things written to log files. *For example, a log alert can notify you when a web server has returned a number of 404 or 500 responses.*

you can create **alert rules** for these items and more:

- Metric values
- Log search queries
- Activity log events
- Health of the underlying Azure platform
- Tests for website availability

The following alert capabilities aren't yet available for the generation of monitoring data:
- Service health alerts based on activity logs
- Web availability tests through Application Insights
﻿
Other services like **Security Center** also rely on Azure Monitor. Security Center, for example, collects security-related data from your virtual machines and other resources. Security Center stores this data in a workspace that you can access from Azure Monitor.

## Kusto query language

The Kusto query language that Azure Monitor uses is case-sensitive. Language keywords are typically written in lowercase. When you're using names of tables or columns in a query, make sure to use the correct case. Events, captured from the event logs of monitored computers, are just one type of data source.

## Diagnostic Logging
General
Tenant and resource logs
Send to Azure Monitor Logs, Event Hubs, Azure Storage
Retention for logs in storage accounts can be 0 (forever) - 365 days
Enable on each resource or centrally through Azure Monitor (for everything but Activity Monitor and Azure AD Sign In/Audit)
Multiple diagnostic settings are supported per resource allowing for variation in where and what is sent
Requires no agent and captures data from Hypervisor
Storage Accounts and Event Hubs can be in different subscriptions

Diagnostic Setting
Turn on or off
Max of 5 per resource
Send to Azure Storage Account, Event Hub, or Log Analytics Workspace
Set retention for archiving to Storage Account for 0-365 days

Enable Diagnostic Logging
PS - Set-AzDiagnosticSetting
CLI - az monitor diagnostic-setting
ARM - providers/diagnosticSettings (subresource of a resource)

Configure Diagnostic Logs to stream to the Event Hub. The following is the correct full PowerShell script:
```sh
Set-AzDiagnosticSetting -ResourceId logsbapp01 -ServiceBusRuleId serbuazh740 -Enabled $true
```

 ![Diagnostic Logging](https://www.prof-its.be/wp/wp-content/uploads/2017/03/diagram.png)

## Azure Activity Log

General
Platform activities for write operations only
Filters can be saved and re-used on dashboard
Download as CSV, export to Event Hub/Storage Account
Send to Log Analytics Workspace (subscription-level setting)
90 days of retention

Azure Activity Logs Solution
Monitoring solution for Azure Monitor
Used to retain logs for longer than 90 days by adjusting retention of Logs Analytics Workspace up to 730 days
Add through More option in Insights section of Azure Monitor or through choosing Logs option in Activity Logs

Cross Tenant
Deliver across tenants by using the pattern of Source account Activity Log -> Source account Event Hub -> Destination Account Logic App -> Destination Account Log Analytics
Useful for CSP
Azure Stor

## Alerts

Azure Monitor proactively notifies you of critical conditions using alerts, and can potentially attempt to take corrective actions. Alert rules based on metrics can provide alerts in almost real-time, based on numeric values. Alert rules based on logs allow for complex logic across data, from multiple sources.

The following are actions that can be configured in Azure alerts:
- SMS
- Webhook
- Logic App

**Metric Alerts for Dynamic Threshold**

ML learns metrics history and identifies behavior (baseline)
Allows for three sensitivities, high, medium, low
Alerts can be trigger on greater/lower than maximum threshold, greater than threshold, or lower than threshold
Thresholds can be ignored until a certain date (testing period) or adjust alerting through deviations
Enable/Disable when setting up a new alert for a metric and moving the threshold radio button from static to dynamic

### Action Group

Used by Azure Monitor and Service Health Alerts to notify alert has occurred
Can be used by multiple alerts
Limit of 2000 per sub
Limits of 1 SMS / Voice every 5 minutes and <100 emails per hour
Create in Azure Monitor -> Alerts ->

Actions
Email/SMS/Push/Voice
Logic App
Azure Function
Webhooks
ITSM
Automation Runbook


**Autoscale**.

Azure Monitor uses Autoscale to ensure that you have the right amount of resources running to manage the load on your application effectively.
Vertical scaling often requires VMs to be redeployed; hence, it may cause temporary unavailability to the service while an upgrade is happening. Therefore, it’s a less conventional approach to scaling.

The scaling out approach doesn’t require downtime or impact availability because the load balancer automatically routes traffic to new instances when they are in a ready state. Conversely, additional instances are gracefully removed automatically as load stabilizes.

**General**
- To create this alert you need to configure an Alert by configuring a resource, condition, action group and alert details.
- Alerts that used to be managed by Log Analytics and Application Insights are now called Classic Metrics
- Alerts have a state of new, acknowledged, or closed
- Alerts have a monitoring state of fired or resolved
- Smart Groups are groups of alerts analyzed by ML to reduce noise and aggregate
- Service Health Alerts  - Configured in Service Health blade

**Components**
- Class of service interruption (service issues, planned maintenance, health advisory)
- Affected subscriptions
- Azure Service
- Region
- Action Group

**General**
Enabled or disabled

Components
- Target resource
- Signal - metric, activity log, application insights, log
- Criteria
- Name
- Description
- Severity (0-4)
- Action

**Sources**
- Metrics values
- Log search queries
- Activity log events
- Health of underlying azure platform
- Website availability

**The rate limit thresholds are:**
✑ SMS: No more than 1 SMS every 5 minutes.
✑ Voice: No more than 1 Voice call every 5 minutes.
✑ Email: No more than 100 emails in an hour.
✑ Other actions are not rate limited.

## Autoscaling

■ Azure Virtual Machines Azure VMs use virtual machines scale sets, a set of identical virtual machines grouped together for autoscaling. Scaling rules can be configured either based on metrics, such as CPU usage, memory usage, and disk I/O, or based on a schedule to trigger scale out to meet a service level agreement (SLA) on the performance.
■ Azure App Services Azure App services come with a built-in mechanism to configure autoscaling rules based on resource metrics such as CPU usage, memory demand, and HTTP queue length or on specific schedules. The rules are set to an app service plan and apply to all apps hosted on it.
■ Service Fabric Likewise, virtual machine scaling, service fabric also supports autoscaling using virtual machine scale sets.
■ Cloud Services These are Microsoft legacy PaaS offerings, but they do support autoscaling at the individual roles level (Web or Worker).
■ Azure Functions Azure functions are a Microsoft serverless compute option. The autoscaling options depend on the hosting plan you choose. If you select the App Service plan, the scaling then works the same as we discussed for Azure App Service. However, if you decide to have the on-demand consumption plan, you don’t have to configure any autoscaling rules because of the nature of the service; it allocates the required compute-on-demand as your code runs.

**Advanced performance analysis**
Advanced performance analysis, included in the performance diagnostics tool, includes all checks in the performance analysis, and collects one or more of the traces, as listed in the following sections. Use this scenario to troubleshoot complex issues that require additional traces. Running this scenario for longer periods will increase the overall size of diagnostics output, depending on the size of the VM and the trace options that are selected.

**Performance diagnostics tool**

The performance diagnostics tool helps you troubleshoot performance issues that can affect a Windows or Linux virtual machine (VM). Supported troubleshooting scenarios include quick checks on known issues and best practices, and complex problems that involve slow VM performance or high usage of CPU, disk space, or memory.
