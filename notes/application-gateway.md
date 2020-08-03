
# Application Gateway


- Application Gateway's work on layer 7 of the OSI layer and can balance traffic based on the URL.  

**General**  
- Personal Information Exchange or .pfx certificates are the only format that Azure Application Gateway accepts.  
- Cookie-based session affinity  
- SSL offload  
- End to end SSL  
- Comes in standard, standardv2, WAF, WAFv2  
- v1 SKU Application Gateway doesnt support autoscaling  
- V2 supports AZ - Autoscaling and Zone redundancy are features which are only available with the Standard_v2 SKU of the Application Gateway.  
- ![enter image description here](https://docs.microsoft.com/en-gb/azure/application-gateway/media/application-gateway-autoscaling-zone-redundant/application-gateway-autoscaling-zone-redundant.png)

- In addition to Autoscaling and Zone redundancy the Standard_v2 SKU also introduces Static VIP, Azure Kubernetes Service (AKS) Ingress controller, Azure Key Vault integration, Rewrite HTTP(S) headers.  
- HTTP response status code match & HTTP response body match are valid criteria for configuring custom health probes.  
- URL-based content filtering  
- Requires its own subnet already exists  
- Connection draining  
  
**Components**  
	- Front-end IP -> single public/private IP or both  
	- Listener -> port, protocol, host, IP, which further sends based on request routing rule (1 to 1)  
	- Request routing rule -> basic or path based  
	- Backend pool -> NIC, VMSS, Public IP, Private IP, FQDN, multi-tenant backend

- Azure Application Gateway supports end-to-end encryption of traffic. Application Gateway terminates the SSL connection at the application gateway. The gateway then applies the routing rules to the traffic, re-encrypts the packet, and forwards the packet to the appropriate back-end server based on the routing rules ﻿defined. Any response from the web server goes through the same process back to the end user.

- ﻿An Azure Application Gateway that has a web application firewall (WAF) Azure Application Gateway offers a web application firewall (WAF) that provides centralized protection of your web applications from common exploits and vulnerabilities. Web applications are increasingly targeted by malicious attacks that exploit commonly known vulnerabilities. SQL injection and cross-site scripting are among the most common attacks.

- **Application Gateway operates as an application delivery controller (ADC). It offers Secure Sockets Layer (SSL) termination, cookie-based session affinity, round-robin load distribution, content-based routing, ability to host multiple websites, and security enhancements.**  

- When using the Firewall Settings in WAF mode, enabling detection mode requires diagnostics to be enabled to review the logged settings. 
- At Least One Back-End Service is Needed. If all back-end services are unhealthy, the application gateway is unable to route around the issue. 
- Azure Application Gateway can be configured with an Internet-facing VIP or with an internal endpoint that isn't exposed to the Internet. 
- An internal endpoint uses a private IP address for the frontend, which is also known as an internal load balancer (ILB) endpoint.
- Protocol Set HTTP or HTTPS here. If you choose HTTPS, you need to upload a certificate to the application gateway.

**Custom Error Page:**  
https://docs.microsoft.com/en-us/azure/application-gateway/custom-error

Adds a global custom error page to the existing Azure Application Gateway for a status code of 502

```sh
$updatedgateway = Add-AzApplicationGatewayCustomError -ApplicationGateway $appgw -StatusCode HttpStatus502 -CustomErrorPageUrl $customError502Url 
```

Lab:
https://docs.microsoft.com/en-us/azure/application-gateway/custom-error

The **New-AzApplicationGatewayIPConfiguration**  - Creates an IP configuration for an application gateway. The IP configuration contains the subnet in which application gateway is deployed.
 
The **New-AzApplicationGatewayFrontendIPConfig** is used when creating the front-end IP configuration.

The **New-AzApplicationGatewayBackendAddressPool** is used when configuring the back-end IP address pool.

The **New-AzApplicationGatewayFrontendPort** is used when configuring the front-end IP port. 

Personal Information Exchange or .pfx certificates are the only format that Azure Application Gateway accepts.

[SKU Comparison](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/application-gateway/application-gateway-autoscaling-zone-redundant.md)

If you require "SSL offloading", application layer treatment, or wish to delegate certificate management to Azure, you should use Azure's layer 7 load balancer  
Application Gateway instead of the Load Balanacer.


