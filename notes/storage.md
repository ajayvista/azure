
# Storage


**General**  
- Page blob max size is 8TB  
- Block blob max size is 4.75TB  
- Containers have an access policy of Private, Blob (anonymous read access for blocks only), Container (anonymous read access for containers + blobs)  
 -Store accounts end with core.windows.net  
- Standard or Premium  
	- Premium Storage Accounts**  
	- Page blobs only  
	- Backed by SSD  
	- LRS replication only

﻿The three different storage account options are:  

- **General-purpose v2 (GPv2)** accounts, *
	- *accounts are storage accounts that support all of the latest features for blobs, files, queues, and tables.**  
	- *This is the most recommended storage account kind.*

- **General-purpose v1 (GPv1)** accounts 
	- *provide access to all Azure Storage services, but may not have the latest features or the lowest per gigabyte pricing.*   
	- *Legacy, no support for tiering*  
	- *This kind is intended for backward compatibility with older deployments; v1 accounts do not support access tiers or zone-level replication.* 
	
- **Blob storage accounts** - 
	- *support all the same block blob features as GPv2, but are limited to supporting only block blobs.*  
	- *Azure Blob storage is the best choice for audio files that need to be streamed in a browser.*  
	- *Blob storage Like General purpose v1, this is a legacy storage account that is used only for businesses that still maintain them. This storage account type can store only VM virtual hard disks, and nowadays it’s best practice to place VM disks in managed disk storage instead of in traditional storage accounts.* 

**Storage Tiers**  - Azure offers three storage tiers for blob object storage:  
  
- Hot storage tier: optimized for storing data that is accessed frequently.  for frequently accessed data; and get a discount on transaction cost.  
- Cool storage tier: optimized for data that are infrequently accessed and stored for at least 30 days.  for infrequently accessed data; and get discount on data storage.
- Archive storage tier: for data that are rarely accessed and stored for at least 180 days with flexible latency requirements.  

## Storage services

- **Table**: Storage is for structured data and uses keys.  
	- http://<account-name>.table.core.windows.net  
	- NoSQL data storage that is now part of Azure Cosmos DB product family  
- **Queue**: Storage is incorrect as it is for message storage.  
	- http://<account-name>.queue.core.windows.net  
	- Reliable messaging service to support a microservices application architecture  
- **Blob**: Storage is for unstructured data.  
	- Blob http://<account-name>.blob.core.windows.net  
	- Binary Large Object data; Intended for text and binary data, including log files, media files, database files, VM disks, and so forth  
	- Most object storage scenarios Documents, images, video, etc.  
	- Blob Storage would not easily be able to store files, or share them with Windows 10 devices.  
	- Defined by a list of blocks​  
	- Used predominantly to store “Objects”​  
	- 50K Blocks of up to 100 MB each = 4.75 TB ​  
	- Append Blob​  
	- Added for Azure Data Lakes  
	- Can add a block of up to 4 MB in a single operation​  
	- Usage increasing significantly – Cloud logging, IoT data, Distributed systems synchronization, etc​  
	- Page Blob​  
		- Collection of 512-byte pages optimized for random read and write operations  		
		- Page aligned random reads and writes IaaS Disks, Event Hub, Block level backup

- **File**: Storage is for files.  
	- CIFS is more suitable for use with Linux devices.  
	- Premium file share offer 100 TiB.
	- File http://<account-name>.file.core.windows.net  
	- Managed Server Message Block (SMB) file shares for cloud and on-premises servers
	- Ensure port 445 is open: The SMB protocol requires TCP port 445 to be open; connections will fail if port 445 is blocked.
	- Azure Files supports two storage tiers: 
		- Premium and 
			- Premium general purpose (GPv1 and GPv2) storage accounts are for premium page blobs only. 
		- Standard. 
			- Standard file shares are created in general purpose (GPv1 or GPv2) storage accounts and premium file shares are created in FileStorage storage accounts. You cannot create Azure file shares from Blob storage accounts or premium general purpose (GPv1 or GPv2) storage accounts. Standard Azure file shares must created in standard general purpose accounts only and premium Azure file shares must be created in FileStorage storage accounts only. 
﻿ 


Types  
GPv1 -> Legacy, no support for tiering  
Blob -> Old don't use, less features, no tiering  
GPv2 -> everything can go here  
Block Blob Storage -> blog only w/ premium performance  
FileStorage -> premium performance for files  
 
 ```sh
Enable "Secure transfer required" setting for the Storage Account named "blobssa1"
Set-AzStorageAccount -Name "blobssa1" -ResourceGroupName "Store2RG" -EnableHttpsTrafficOnly $True
```
 
## Replication

**LRS (Locally-Redundant Storage)** keeps data synchronously three times within a single physical location in the primary region. LRS is the least expensive replication option, but is not recommended for applications requiring high availability. 
  
**Geo-Redundant Storage (GRS)  replicates your data between two regions**, it would also allow the data to be synced to another region that is located outside primary region.  To ensure resiliency against a region failure you would need to enable Geo-Redundant Storage (GRS) and not Zone Redundant Storage (ZRS).  

Geo-redundant storage (GRS) brings additional redundancy to the data storage over both LRS or ZRS. Along with the three copies of your data stored within a single region, a further three copies are stored in the twinned Azure region. So using GRS means you get all the features of the LRS storage within your primary zone, but you also get a second LRS data storage in a neighbouring Azure region. This data is updated asynchronously, so there is a small lag between the 2 data sets, but for most cases this is acceptable.

**RA-GRS** - 2nd region and read 

**Zone-redundant storage (ZRS)** copies your data synchronously across three Azure availability zones in the primary region. For applications requiring high availability, Microsoft recommends using ZRS in the primary region, and also replicating to a secondary region.  Zone Redundant Storage does not provide redundancy against a region failure.  ZRS only provides redundancy against zone failures within the same region.
  
**Shared Access Signatures**  
- Account SAS (blob, file, table, queue) and Service SAS  
- Uses hash-based message encryption  
- Limit to set of IPs and a secure protocol  
  
**Stored Access Policies**  
- Supported only for Service SAS  
- SAS associated with policy inherit start/expiry team and permissions and revocation  
- Can be created with GUI using Storage Explorer

**Custom Domains**  
- Can be used to access blob data in storage account  
- One per storage account  
- Direct (create each CNAME) or indirect (no downtime and uses ASVERIFY subdomain)  
  
**Special Features**  
Require secure transfer  
Allows access from all networks or a single Vnet  
Soft delete for blobs  
Hierarchal namespace for DataLakes V2  
  
PowerShell CLI Commands  

```sh
New-AzStorageAccount -ResourceGroupName -Name -Location -SkuName (Standard_LRS, etc) -kind (StorageV2, etc)  
Az storage create --name --resource-group --location --sku --kind
```

## Storage Explorer

**Use Storage Explorer to create a snapshot of the Blob Storage**  
General  
GUI-based tool to navigate Azure storage  
Connect to storage account via Azure AD, connection string + SAS URI, or storage account name and key


**If storage account has virtual network associated, it would be only accessible from the vnet irrespective of having key or storage account is public. By default it is accessible from all vnet**

Allowing **Trusted Microsoft Services** to Access the Storage Account The Firewalls And Virtual Networks blade has an option, enabled by default, called Allow Trusted Microsoft Services To Access This Storage Account. This option allows connectivity to the storage account from other Azure services, such as Azure Monitor, Azure Backup, and Azure Event Hubs.

**Storage Service Encryption (SSE)** is data encryption at rest for Azure Storage. System-managed keys support all services; customer-managed keys are for blobs and files. Managed disks are automatically encrypted by SSE and do not support BYOK. Unmanaged disks must use BYOK. Azure security center flags unencrypted, unmanaged disks as a risk.


