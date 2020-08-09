# App Service Plans

### Service Plans
- Free - 10 apps, 1 GB disk
- Shared - 100 apps, 1GB disk, LB, custom domains
- Basic - 10GB disk, 3 instances, functions
	- 	Standard Pricing Tier (basic)
		- Basic supports only up to 3 instances.
- Standard - 50GB, 10 instances, deployment slots, VNET integration, autoscale
	- The Standard S1 plan is the recommended minimum pricing tier for production web apps.
		- Standard Pricing Tier (recommended)  - Standard supports up to 10 instances, and would be enough as the Standard plan includes auto scale that can automatically adjust the number of virtual machine instances running to match your traffic needs.
- Premium - 250GB, 20 instances, clone app
- Isolated - 1TB disk, 100 instances
	- The Isolated Service Plan L1 is the correct plan as the tier allocates 3.50 GB RAM memory.
	- The  Isolated Service Plan L3 is not suitable as this tier would cost a lot more as it provides 14 GB RAM memory.

- Linux -> docker, ruby/.NET Core/Node.js/PHP
- All plans but Linux -> .NET, .NET Core, Java, Node.js, PHP, Python

### Key Points
- Azure web apps are instances of an App Service that run within an App Service Plan. For TLS authentication, make sure that your web app is not in the F1 or D1 tier.

- The D1 (Shared) pricing tier does not support HTTPS.
- Globally unique host name for your web app under azurewebsites.net.

- az webapp/New-AzWebApp create command with the Azure CLI to perform this task from your local terminal or Azure Cloud Shell instance. Azure PowerShell is a common command-line alternative used to deploy web apps. You can use the New-AzWebApp cmdlet to script the deployment of standard or containerized web apps in App Service.
	- *Note that the supported file types are Windows (.cmd, .bat, .exe), PowerShell (.ps1), Bash (.sh), PHP (.php), Python (.py), Node.js (.js), Java (.jar).*

- You can move an app to another App Service plan, as long as the source plan and the target plan are in the same resource group and geographical region. The region in which your app runs is the region of the App Service plan it's in. However, you cannot change an App Service plan's region.

- In App Service, TLS termination of the request happens at the frontend load balancer. When forwarding the request to your app code with client certificates enabled, App Service injects an X-ARR-ClientCert request header with the client certificate. App Service does not do anything with this client certificate other than forwarding it to your app. Your app code is responsible for validating the client certificate.

- For ASP.NET, the client certificate is available through the HttpRequest.ClientCertificate property.

- For other application stacks (Node.js, PHP, etc.), the client cert is available in your app through a base64 encoded value in the X-ARR-ClientCert request header.

- App Service Tier If you’re planning to use authentication via Azure AD for your app, you have to be using HTTPS. Therefore, if you’re using a custom domain, you need your SSL bindings set (see Skill 4.2 “SSL/TLS”) and at least a B1 App Service Plan.

**Azure App Services - Azure Service Environment (ASE)**
- Container for up to 100 single instance App Services in a subscription
- Goes directly into customer Vnet subnet
- External or internal
- High scale, isolation and secure network access, high memory
- Integrate with WAF
- Dedicated environment for Windows/Linux Web Apps, Docker, mobile apps, functions

**Azure App Services - Deployment Slots (Not Swapped)**
- Publishing endpoints
- Custom domains
- SSL Cert and binding
- Scale setting
- Webjob scheduling
- Swap with preview or swap

**Configure Web App for Docker**

```sh
Az webapp container set --name --resource-group --docker-custom-image-name .azurecr.io. --docker-register-server-url http://.azurecr.io

Configure ACR and Push Docker Image
Az acr create --name <registry_name> --resource-group

Docker login .azurecr.io
Docker tag .azurecr.io/:vXXX
Docker push .azurecr.io/:vXXXX

```

## Webjob

### Azure Web App Diagnostics

**Application** -> Error, Warning, Information, Verbose (Application/)
- Application logging (Filesystem) Enable this setting to collect diagnostic traces from your web application code. Keep in mind this setting is used for interactive troubleshooting and when using real-time log streaming. This setting turns itself off after 12 hours.
- Application logging (Blob) To persist diagnostic log data for long periods of time, you can configure logging to Blob storage. Enabling this setting requires that you associate the configuration with your Azure Blog storage account.

**Web Server** -> Web Server Logging (http/RawLogs), Dedicated Error Message (DetailedError), Failed Request Tracing (W3SVC####/), Deployment (/Git)
- Obtain via FTP or CLI (az webapp log download)
- Web Server logging You can enable web server logging to gather diagnostic information specific to the web server hosting your application. These logs can be stored on the file system or in Azure Blob storage.
- Detailed error messages Enable this setting when you need more detailed error messages from the platform.
- Failed request tracing Enable this setting when you need in-depth diagnostic information about failed requests and other errors being thrown by your web applications.

### Azure Web App Application Insights Alerts
- Metrics
- Web Tests
- Proactive Diagnostics

### Application Setting
- Configure version of .NET and PHP
- Turn Java or Python on (off by def)
- Change to 64-bit platform (basic+)
- Turn on web socket
- Enable apps to always run (basic+)
- Auto-swap and move deployment into slot to prod automatically
- Custom domains associated w/ web apps
- Cookie affinity (on by def)

### Connection String
- Configure db per deployment slot or global
- Variable instead of file
- SQLCONNSTR_, MYSQLCONNSTR_, SQLAZURECONNSTR_, CUSTOMCONNSTR_

### Handler Mapping
- External script processes
- Extension, handler path, argument

### Virtual Application
- Subdirectories of app that do something specific
- Virtual directory, physical path, application

### General
- Run program or script in same context as Web App
- Continuous (start immediately, runs on all instances, remote debug)
- Triggered (manual/scheduled, single instance, no remote debug)
- CMD, Bash, PowerShell, Python, PHP, Node.js, Java
- Scale App Service supports the concept of autoscaling, so it’s possible that your web app might be powered by multiple virtual machines. If so, you can leave the Scale setting configured as Multi Instance, meaning your background jobs will run on every virtual machine hosting your web app.
- Configure your code to run on the single instance by using the key is_singleton:true either in the configuration file settings.job or adding an attribute programmatically that comes as part of Azure WebJob SDK


## Service Fabric application

Azure Service Fabric Stateful Service allows you to maintain the state reliably across the nodes of the service instance locally. Stateful service comes with a built-in state manager called Reliable Collections that enables you to write highly scalable, low-latency applications.

Reliable collections can store any .NET type or custom types. The data stored in the reliable collections must be serializable because the data is stored on the local disk of service fabric replicas. Azure Service Fabric allows you to configure an application as either stateless or stateful. The stateful service automatically manages the state using Reliable Collection locally on each fabric cluster.
