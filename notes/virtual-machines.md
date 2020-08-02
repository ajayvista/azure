**Virtual Machines**

﻿For all Virtual Machines that have two or more instances deployed in the same Availability Set, we [Microsoft] guarantee you will have Virtual Machine Connectivity to at least one instance at least 99.95% of the time.  
  
﻿For all Virtual Machines that have two or more instances deployed across two or more Availability Zones in the same Azure region, we [Microsoft] guarantee you will have Virtual Machine Connectivity to at least one instance at least 99.99% of the time.

**General**  
- ACU - 100 for A1  
- Ultra SSD, Premium SSD, Standard SSD, Standard HDD  
- WinRMHTTPS 5986, WinRMHTTP 5985  
- OS Disk has max capacity of 2TB  
- Data Disk has max capacity of 32TB  
	- VM Storage  
		- Standard HDD -> 32TB, 500MB/s, 2,000 IOPS  
		- Standard SSD -> 32TB, 750MB/s, 6,000 IOPS  
		- Premium SSD -> 32TB, 900MB/s, 20,000 IOPS  
		- Ultra SSD -> 65TB, 2,000MB/s, 160,000 IOPS
		
	- Disk Caching  
		- Method for improving performance of VM  
		- Utilize RAM and SSD from underlying host  
		- Available for both standard and premium  
		- Read-only / Read-Write  
		- OS disk is by default Read-Write

- Temporary disk persists after successful reboot and uses /dev/sdb and E:  

**Support OS**  
Windows 2003+ supported but earlier than 2008 R2 must provide own images  
Server roles of DHCP, HyperV, RMS, WDS not supported

**Types**  
**A** - Basic (no support for autoscaling or load balancers) and Stanard  
**B** - Burstable  
**D** - General purpose  
**Dc** - confidentiality and integrity  - ﻿*DC-series virtual machines are a new family of VMs to protect the confidentiality and integrity of your data and code while it's processed in Azure through the use of secure enclaves.*  
**E** - in memory hyperthreading (SAP HANA)  
**F** - CPU opt  
**G** - memory and storage (big data)  
**H** - HPC  
**Ls** - Storage  
**M** -> Large memory  
**N** - GPU

**Accelerated Networking**  
- Bypass host and virtual switch and go directly to physical NIC  
- Supported only on some VM series  
- Enable on existing VM if it is Azure Gallery image and all VMS in VMSS must be stopped and deallocated  
- VMs with AdvNet can only be resized if the VM type being moved to supports it

**Modify Disk Caching**  
```sh
$vm = Get-AzVM -Name  
Set-AzDataDisk -VM $vm -Name "data disk name" -Caching ReadWrite | Update-AzVm
```

**Create VM from VHD**
```sh
Add-AzVhd -ResourceGroupName -Destination -LocalFilePath
New-AzDiskConfig -AccountType Standard_LRS -Location -CreateOption Import -SourceURI
New-AzDisk -DiskName -Disk <disk_config> -ResourceGroupName
Set-AzOSDisk -VM <vm_config> -ManagedDisk <os_disk>.id -StorageAccountType Standard_LRS -CreateOption Attach -Windows
```
```sh
New-AzResourceGroupDeployment //deploy a Virtual Machine from the Azure Marketplace
New-AzVm //is used to create a new VM but not from the marketplace. 
```

**In order to successfully move the resource between Resource Groups you will need the following:** 
```sh
Move-AzResource -DestinationResourceGroupName "<myDestinationResourceGroup>"  -ResourceId <ResourceId> 

DestinationSubscriptionId is only required if moving the resource between subscriptions. 
```

**Availability Sets**  
- Group 2+ machines to protect against failure  
- Max of 3 FD and 20 UD  
- Managed disks are automatically managed by availability set

**Virtual Machine Scale Sets**  
Max of 1,000 for gallery image and 600 for own image  
Low priority saves costs but can be evicted  
Load balance w/ load balancer or Application Gateway  
Set a min and max of VMs  
Scale out on metrics or a time schedule  
Metrics can be infrastructure or application metrics

PowerShell / CLI  
```sh
Get-AzRemoteDesktopFile - ResourceGroupName -Name -Launch
```

- Configure an alternative method of authentication which does not utilize passwords. (ssh public key)  
- We cannot just move a virtual machine between networks. What we need to do is identify the disk used by the VM, delete the VM itself while retaining the disk, and recreate the VM in the target virtual network and then attach the original disk to it.
- Before you upload a Windows virtual machines (VM) from on-premises to Microsoft Azure, you must prepare the virtual hard disk (VHD or VHDX). Azure supports only generation 1 VMs that are in the VHD file format and have a fixed sized disk. The maximum size allowed for the VHD is 1,023 GB. You can convert a generation 1 VM from the VHDX file system to VHD and from a dynamically expanding disk to fixed-sized.

- The encryption technology depends on the guest operating system: Windows VM disks are encrypted with BitLocker Drive Encryption, whereas Linux VM disks are encrypted with the open-source DM-Crypt library.You need to provision an Azure Key Vault before using ADE because Azure needs a secure location for the disk encryption keys. The key vault needs to reside in the same region as your target VMs. Encrypt the operating system and data disks of a running VM by running the following Azure CLI command:

- You assign public and private IP addresses to VMs through their associated virtual network interface, not the VM resource itself.

- When using Incremental mode in ARM deployment, Azure Resource Manager leaves unchanged, resources that exist in the Resource Group but are not specified in the template. 

- Complete mode is incorrect as it replaces the resources with those specified within the template. 
