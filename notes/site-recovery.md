# Site Recovery

Site Recovery currently supports Azure Disk Encryption(ADE), with and without Azure Active Directory (AAD) for VMs running Windows operating systems. For Linux operating systems, we only support ADE without AAD. Moreover, for machines running ADE 1.1 (without AAD), the VMs must be using managed disks. VMs with unmanaged disks aren't supported. If you switch from ADE 0.1 (with AAD) to 1.1 , you need to disable replication and enable replication for a VM after enabling 1.1.

Scenarios
Region to region
VMWare/Hyper V/physical servers/Azure Stack to Azure
VMWare/Hyper V/SCVMM/Physical servers to second data center

Replication Policy
Recovery point retention is default of 24 hours (oldest 72 hours) for crash consistent and 60 minutes for app consistent
Associate with a configuration server

Azure Requirements
Recovery Services Vault
V1 Storage Account in same region as Vault
Existing Vnet

Common practice is to make the failover one-way and use ASR to move servers to Azure. Then you switch off their local counterparts, effectively migrating your environment to Azure. Only pay storage and instance charges

Ensure RDP is Enabled Before MigrationEnsuring the local system is configured to allow remote desktop connections before migrating it to Azure is worth the prerequisite checks. There will be considerable work to do, including a jump box local to the migrated VMs virtual network if these steps are not done before migration

Ensure that Storage and Networking are Available. A storage account and network are necessary within the specified subscription in the same Azure region as the Recovery Services vault.

Site Recovery doesn't support using an authentication proxy to control network connectivity.

If you're using a URL-based firewall proxy to control outbound connectivity, allow access to these URLs:

***.blob.core.windows.net**//*Allows data to be written from the VM to the cache storage account in the source region.*
**login.microsoftonline.com**//*Provides authorization and authentication to Site Recovery service URLs.*
***.hypervrecoverymanager.windowsazure.com** //*Allows the VM to communicate with the Site Recovery service.*
***.servicebus.windows.net**//*Allows the VM to write Site Recovery monitoring and diagnostics data.*

Verify the settings:

**Disk encryption key vaults**: By default, Site Recovery creates a new key vault on the source VM disk encryption keys, with an asr suffix. If the key vault already exists, it's reused.

**Key encryption key vaults:** By default, Site Recovery creates a new key vault in the target region. The name has an asr suffix, and is based on the source VM key encryption keys. If the key vault created by Site Recovery already exists, it's reused.
Select Customize to select custom key vaults.


