# App


## Logic App

Logic Apps perform custom integrations between applications and services both inside and outside of Azure. Logic Apps are also called a designer-first serverless integration service that allows you to use Azure portal and visually see and design workflows.

Logic Apps support more than 200 managed connectors that provide triggers and actions to access cloud-based SaaS Services, Azure native suite of services, and on-premises services using a Logic App gateway agent.

To enable the log analytics feature for a logic app, ensure that the log analytics workspace that will collect the information exists beforehand.


## Azure Functions App

Function Apps will be able run code in Azure. They are what is termed as Microservices. A Consumption plan is the most cost effective plan for code that will be called infrequently.

**General**
- Name is unique and ends with .azurewebsite.net
- Consumption or App Service Plan
- Requires storage account w/ Blob, Queue, and Tables
- Pay for execution time and number of executions
- Linux supports .NET, JavaScript, Python, and Docker
- Windows supports .NET, JavaScript, Java, PowerShell

Function apps are built to listen for events that kick off code execution. Some of the events that functions listen for are
- HTTP Trigger
- Timer Trigger
- Azure Queue Storage
- Azure Service Bus Queue trigger
- Azure Service Bus Topic trigger

Multiple Types of Authentication Possible when configuring a function for the HTTP Trigger, you need to choose the Authorization level to determine whether an API key will be needed to allow execution. If another Azure service trigger is used, you may need an extension to allow the function to communicate with other Azure resources.


