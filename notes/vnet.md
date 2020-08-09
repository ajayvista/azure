# VNET

Virtual Network (VNet) service endpoints extend your virtual network private address space and the identity of your VNet to the Azure services, over a direct connection. Endpoints allow you to secure your critical Azure service resources to only your virtual networks. Traffic from your VNet to the Azure service always remains on the Microsoft Azure backbone network.

- A customer can have up to 1,000 virtual networks per Azure region, per subscription.  you can have up to 3,000 subnets per virtual network.

- DNS serversAzure-provided name resolution enables you to resolve DNS hostnames for all resources within a single VNet. However, to facilitate other name resolution scenarios, such as
	- Active Directory communications, either within a VNet, between VNets, or between a VNet and on-premises
	- Ordinary DNS resolution for VMs spread across different VNetsyou need to configure your VNet properties with the IP address(es) of additional DNS servers.

Know to which Azure resources you can supply custom DNS server IP addresses. The answer is the virtual network and the virtual network interface.

- Service endpoints enable you to secure certain Azure resources to particular virtual networks.
- VNet peerings are intransitive. This means that, by default, two spoke VNets are unable to communicate through a hub (transit) virtual network.
- A virtual network (VNet) consists of one or more top-level IPv4 address spaces; the most common design pattern is to create a subnet for each application tier.
- You will have to recreate the VPN with a new IP address and Pre-Shared key if You want to change it to a route-based VPN from policy based. 
- You can setup 500 peerings per Virtual Network in the Azure Resource Manager Portal.

General

```sh
New-AzVirtualNetwork -ResourceGroupName -Name -Location -AddressPrefix
Add-AzVirtualNetworkSubnetConfig -Name -AddressPrefix -VirtualNetwork <Vnet_object>
<vnet_object> | Set-AzVirtualNetwork
Azure reserves first three IPs and last IPs
Apply DNS to NIC or VNET
```

## Vnet Peering

Peering is a good choice when a business needs to link VNets and has no requirement for end-to-end encryption.

```sh
Add-AzVirtualNetworkPeering
```

- Allow Forwarded Traffic From Enable this option to allow traffic forwarded to your current VNet to be forwarded to the VNet peer.
- Allow Gateway Transit Enable this option to allow the peered VNet to allow the current VNet’s VPN gateway. 
- In other words, enable this option only on your hub VNet. Use Remote Gateways This option is mutually exclusive from Allow Gateway Transit. You enable this option when you need to allow the local spoke VNet to forward traffic through a hub VNet, typically across a site-to-site VPN to an on-premises network.
- You can setup 500 peerings per Virtual Network in the Azure Resource Manager Portal.
- You cannot peer across clouds. For example, a VNet in Azure public cloud cannot be peered to a VNet in Azure China cloud.  
- Peering virtual networks in different regions is also referred to as Global VNet Peering.  
- Support for Basic Load Balancer only exists within the same region. Support for Standard Load Balancer exists for both, VNet Peering and Global VNet Peering. Services that use a Basic load balancer which will not work over Global VNet Peering  
- The virtual networks can be in the same, or different subscriptions. When you peer virtual networks in different subscriptions, both subscriptions can be associated to the same or different 
- Azure Active Directory tenant.  
- The virtual networks you peer must have non-overlapping IP address spaces.  
- You can't add address ranges to, or delete address ranges from a virtual network's address space once a virtual network is peered with another virtual network.  
- You can peer two virtual networks deployed through Resource Manager or a virtual network deployed through Resource Manager with a virtual network deployed through the classic deployment model. You cannot peer two virtual networks created through the classic deployment model.  
- You can't resolve names in peered virtual networks using default Azure name resolution. To resolve names in other virtual networks, you must use Azure DNS for private domains or a custom DNS server.  
- Resources in peered virtual networks in the same region can communicate with each other with the same bandwidth and latency as if they were in the same virtual network.  
- A virtual network can be peered to another virtual network, and also be connected to another virtual network with an Azure virtual network gateway. When virtual networks are connected through both peering and a gateway, traffic between the virtual networks flows through the peering configuration, rather than the gateway.  
- Point-to-Site VPN clients must be downloaded again after virtual network peering has been successfully configured to ensure the new routes are downloaded to the client.  
  
- There is a nominal charge for ingress and egress traffic that utilizes a virtual network peering.  
- The accounts you use to work with virtual network peering must be assigned to the following roles:  
- Network Contributor: For a virtual network deployed through Resource Manager.  
- Classic Network Contributor: For a virtual network deployed through the classic deployment model.

- Azure Global Virtual Network peering is now available. This allows you to peer virtual networks in different Azure regions to build a global private network in Azure.  
  
- Traffic that flows through this globally peered network is completely private and never leaves the Microsoft backbone. Global Virtual Network peering is easy to set up and manage. It's a low latency (no extra hops) connection enabling direct VM-to-VM connectivity targeting scenarios such as data replication, disaster recovery, and failover.

- VNet peering is their to connect VNets within the same Azure region.  
	- Forwarded Traffic - allow VNA in another Vnet to forward traffic to this Vnet over the peering  
	- Gateway Transit / Remote Gateway  
	- Create using az network peering create

- Virtual network peering allows communication between networks in Azure without need for a VPN, whereas Site-to-Site VPNs connect Azure to existing on-premises networks and ExpressRoute provides completely private connections to Microsoft services from an on-premises environment.

- Global VNet peering means the peered VNets can exist in different Azure regions or even different subscriptions.


## VPN Gateways

**General**
- Basic SKU
- Route-based VPN doesn't support RADIUS or IKEv2 for P2S and supports 10 tunnels
- Policy-based VPN supports 1 tunnel and no P2S

**VpnGw1-3**
- Supports route-based only
- Up to 30 tunnels, P2S, BGP, active-active, custom IPSec/IKE and ExpressRoute coexistence

**P2S VPN**
- Supports SSTP, IKEv2, SSL/TLS
- Authenticate with certificate or RADIUS
- Basic SKU only support SSTP while VpnGw1+ supports IKEv2
- Clients download a config file

- VPN An encrypted connection between two networks via the public Internet
- The virtual network gateway is a router endpoint specifically designed to manage inbound private connections. The resource requires the existence of a dedicated subnet, called the gateway subnet
- When you configure networking resources, there’s no way to deprovision them. Virtual machines can be turned off, but networking resources are always on and billing if they exist.
- If your organization has requirements for one-to-one access and connectivity, a network security group configured at the interface level for the VM might be necessary to ensure restricted access from one host to another.

## Route Table

- To route traffic from an Azure subnet to a virtual firewall appliance
- You can define the following next hop types in an Azure route table:
	- Following are available  Hop Types-  
		- Virtual Network Gateway, 
		- Virtual Network,
		- Internet, 
		- Virtual Appliance 

- Virtual network gateway Use this type when you need to force traffic through an Azure virtual network gateway.
- Virtual network Use this type when you need greater control of traffic between subnets in a VNet.
- Internet Use this type when you need to control Internet-bound traffic.
- Virtual appliance Use this type when you need to route traffic through a central network virtual appliance (NVA), such as an enterprise firewall VM deployed from Azure Marketplace.


## Network Security Group

- Network security groups also allow the collection of flow logs that capture information about the traffic entering and leaving the network via configured network security groups. To enable this, you need two additional resources for all features 
	- A storage account to collect the flow log data
	- A Log Analytics workspace for traffic analysis

- Rules evaluated by priority 100-4096 with lowest being evaluated first
	- Service Tags of VirtualNetwork, AzureLB, Internet
	- 100 NSGs per Sub (raise to 400)
	- 200 NSGs rules per NSG (raise to 500)
	- Vnets per Sub 50 (raise to 500)
	- PIP dynamic 60 PIP reserved 20

## Vnet-to-Vnet

- Consider a VNet-to-VNet VPN when security needs require the communications. 
	- VpnGw1-3 Supports BGP, up to 30 site-to-site VPN connections and up to 1.25 gigabits per second (Gbps) throughput for the Gw3 SKU. 
	- VpnGw1-3AZ These SKUs have the same feature set as VPNGw1-3 but are availability-zone aware.
   
  - Enable Active-Active mode This setting is used when you configure
   high-availability failover for an Azure VPN Gateway.
   
   - Shared Key (PSK) This is a (hopefully) strong passphrase you attach
   to each gateway to serve as a basic mutual authentication method.
   
   - If you enable default security rules for your NSG, you need to
   remember that two of these rules (AllowVNetInBound and
   AllowVnetOutBound) explicitly allow inter-VNet traffic. In the name
   of least-privilege security, you must work to ensure only required
   traffic flows are authorized in your Azure design.
   
   - VNet-to-VNet VPNs support name resolution for VMs on either side of a
   connection. If you’re doing peering, you need to take additional
   steps to support name resolution because Azure-provided name
   resolution can’t function across a peering. You can configure the
   following: 
   - A private Azure DNS zone This is a nonroutable
   Domain Name system (DNS) zone you can attach to both VNets.   
   - DNS servers Here you deploy a DNS server VM in each VNet, and configure
   each to forward name resolution requests to the other VNet’s name
   server. If you go this route (so to speak), make sure to update each
   VNet’s DNS server list accordingly.
   - Connection between two Vnets in same or different subscriptions that
   is encrypted with IPSec Uses Virtual Network Gateway and Vnet-to-Vnet
   connection Requires Route-Based VPN

## Site-to-Site

﻿A Site-to-Site VPN gateway connection is used to connect your on-premises network to an Azure virtual network over an IPsec/IKE (IKEv1 or IKEv2) VPN tunnel. This type of connection requires a VPN device, a local network gateway, located on-premises that has an externally facing public IP address assigned to it. Finally, create a Site-to-Site VPN connection between your virtual network gateway and your on-premises VPN device.

Site-to-Site VPN is used to connect other sites. 
To configuring a Site-to-Site VPN between an on-premises environment and Azure we need Public IP Address of the Virtual Network Gateway and a Shared Key

## Point-to-site

Point-to-Site VPN is the best method to connect remote users to an Azure VPN.

Incorrect answers are:https://www.youtube.com/watch?v=cEbIvDrWnno

windows 10 : self sign certificate - https://gist.github.com/RomelSan/bea2443684aa0883b117c37bac1de520

## Azure Relay

You can connect the two applications by configuring an Azure Relay.  The Azure Relay service enables you to securely expose services that run in your corporate network to the public cloud. You can do so without opening a port on your firewall, or making intrusive changes to your corporate network infrastructure.

ExpressRoute, Site-to-Site VPN Gateway provide connections between your on-premises and Azure environments rather than connecting App to App. 

Azure AD Connect with Connect Health is used to sync identities from on-premises accounts to cloud.   

**General**  

Allows messages to be received by a public endpoint and relayed to on-premises applications  
Application installed on-premises and established outgoing session to listen for messages  
Applications on-premises are WCF and Hybrid Connections (Web Sockets)

Relay service allows messages to be passed from the internet inside the company firewall to specific endpoints such as WCF  
Doesn’t not require changes to the firewall  
Application inside network opens up outbound connection to Azure, and the connection stays open.  
Message pass over the link  
  
Types of relays  
  
Hybrid Connections - more sophisticated  
- Based on BizTalk service  
- Uses standard web sockets, and multi-platform support  
  
WCF Relay - is for Windows Communication Framework (WCF) services  
- Legacy Offering  
- Only Supports HTTP communication, limited platform support

Azure Relay services allows establishing a secure connection to the services running behind the corporate network without opening any firewall ports. The Relay service has two types:■ Hybrid Connections Hybrid connection is based on standard Http and Web Socket protocols and hence can be used in any platform and languages.■ WCF Relay WCFRelay is A legacy Relay offering that works only for .NET framework Windows Communication Foundation endpoints.

## Network Watcher

IP flow verify checks if a packet is allowed or denied to or from a virtual machine. The information consists of direction, protocol, local IP, remote IP, local port, and remote port. If the packet is denied by a security group, the name of the rule that denied the packet is returned. While any source or destination IP can be chosen, IP flow verify helps administrators quickly diagnose connectivity issues from or to the internet and from or to the on-premises environment.

General
Requires Network Watcher Agent be installed on Linux/Windows (VM)
Enabled on region by region basis and enabled in Network Watcher -> Overview blade

Capabilities
Topology
Connection Monitor - Continuously monitor connections from VM to URI, IP, FQDN
IP Flow Verify - Determine why packet allowed or denied and relevant NSG
Effective Security Rules - see effective NSG cumulative, subnet, NIC
VPN Troubleshooter
Packet capture and log to storage account or to VM file system
View Azure quotas and limits
View and enable NSG flow logs
Enable/Disable Diagnostic Logging for networking components
Enable and view Traffic Analytics

Network Performance Monitor
Log Analytics Solution
Monitor performance across cloud and on-premises
Monitor network connectivity using HTTP, HTTPS, TCP, ICMP
Monitor ExpressRoute

