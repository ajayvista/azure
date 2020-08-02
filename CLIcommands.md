
# Azure frquently used CLI commands

### Reference URL
#Azure important CLI commands(https://docs.microsoft.com/en-us/cli/azure/reference-index?view=azure-cli-latest#az-rest)

## Az Commands - Managing Extensions

#### Get CLI modules installed into your shell
az extension list-available --output table

#### Create alias of options 
az alias create --name gp --command group

#### List possible options at any level
az webapp -h // can use -h to get help after that part
az extension --help

#### Get the details of a subscription
az account list

## Azure Configure
Reference: https://docs.microsoft.com/en-us/cli/azure/azure-cli-configuration?view=azure-cli-latest

### Configure default properties

```sh
/// Configure local and group properties default value 
/// if not passed into CLI commands this will be considered

az configure --defaults location=eastus group=RG01

/// Configure default output format
az configure --defaults output=table
```

### Configure default properties through configuration file

	output = table
	collect_telemetry = yes

	[default]
	location = eastus
	group = RG01

	[core]
	disable_confirm_prompt=Yes

	[logging]
	enable_log_file=yes
	log_dir=/var/log/azure

### Invoke a custom request using the logged-in credential 

```sh
az rest  --method get --url http://dummy.restapiexample.com/api/v1/employees
az rest --method get --url http://dummy.restapiexample.com/api/v1/employees --output-file t.txt
```
### Find CLI commands examples

```sh
az find "subscription"
az find "az storage"
```

### Interactive CLI

```sh
az extension add --name interactive
az interactive
```

## Resource Group

### Create Resource Group

```sh 
//Powershell
New-AzureRmResourceGroup -Name RG01 -Location "EastUs"

//bash
// if location not given, default location would be considered that you set 
	 az configure --defaults location=eastus

az group create -n RG01 
```

## Storage

### Create storage account

```sh
bash
az storage account create --name tlwstor170 --resource-group RG01 --kind StorageV2--access-tier Hot

Powershell

$sa = New-AzureRmStorageAccount -ResourceGroupName RG01 -AccountName tlwstor170 -Location EastUs -SkuName Standard_GRS

ref: https://docs.microsoft.com/en-us/rest/api/storagerp/srp_sku_types
```

### Sets a new storage account to Cool storage instead of Hot:
```sh
bash
az storage account update --name tlwstor170 --resource-group RG01 --access-tier Cool

Powershell
Set-AzStorageAccount --name tlwstor170 --resource-group RG01 --access-tier Cool
```

### Enable "Secure transfer required" setting for the Storage Account 

```sh


Powershell
Set-AzStorageAccount -Name "tlwstor170" -ResourceGroupName "RG01" -EnableHttpsTrafficOnly $True
```

### Get Access keys

```sh
Get-AzStorageAccountKey -name "tlwstor170" -ResourceGroupName "RG01"
```

TODO: Manage storage account keys with Key Vault 
https://docs.microsoft.com/en-us/azure/key-vault/secrets/overview-storage-keys-powershell


## VM

### Deploy a Virtual Machine from the Azure Marketplace

```sh
New-AzResourceGroupDeployment -ResourceGroupName "RG01" -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json"  -adminUsername  "azureuser" -adminPassword "adminPassword"  dnsLabelPrefix "dns"
```

### Create a new VM but not from the marketplace. 

```sh
New-AzVM  -ResourceGroupName "RG01" -Name "WVM01" -Location "EastUs" -Image win2016datacenter -Size Standard_D2s_v3 -OpenPorts 3389   -Verbose
```

### Get VMs in resource group

```sh
Get-AzVm -ResourceGroupName "RG01" | Select Name
```

### Resize VM

```sh
//Step 1:
$vm = Get-AzVm -ResourceGroupName "RG01" -VMName "SimpleWinVM"

//Step 2:
$vm.HardwareProfile.VmSize = "Standard_D2s_v3"

//Step 3:
Update-AzVM -VM $vm -ResourceGroupName "RG01"
```
### Enable disk caching for VM

```sh
//Step 1: Get VM context
$vm = Get-AzVm -ResourceGroupName "RG01" -VMName "SimpleWinVM"

//Step 2: Know attach disk and name
$vm.StorageProfile.DataDisks

//Step 3: Set the property
Set-AzureRMVMDataDisk -VM $vm -Name "<DiskName>" -Caching ReadWrite | Update-AzureRMVM
```

### Move AzResource from one resoure group to another

```sh
Step 1: Get the resource id
$Resource = Get-AzResource -ResourceName "WVM1"

Step 2: If target resource not exist, create
New-AzureRmResourceGroup -Name RG02 -Location "EastUs"

Step 3: Move to destination resource group, there is another parameter subscription id - which is needed when move to different subcription
Move-AzResource -ResourceId $Resource.ResourceId -DestinationResourceGroupName "RG02"
```

### Disk Encryption Status

The Azure CLI statement "az vm encryption show" will check the encryption status of a IaaS VM. 
```sh
bash-
az vm encryption show --name "VMApp01" --resource-group "VMResourceGroup"

Powershell
Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName "RG02" -VMName "WVM01"
```

### Create Key Vault

```sh

///Create new key vault
New-AzKeyVault -Name "RG01KeyVault" -ResourceGroupName "RG02" -Location "EastUs"  -EnabledForDiskEncryption -Sku "Standard"

// Update existing key vault
Set-AzKeyVaultAccessPolicy -VaultName "RG01KeyVault" -ResourceGroupName "RG02" -EnabledForDiskEncryption

//Permission
Set-AzureRmKeyVaultAccessPolicy -VaultName 'RG01KeyVault' -UserPrincipalName 'ajac@public.net' -PermissionsToKeys create,import,delete,get,update,list,encrypt,decrypt -PermissionsToSecrets set,delete,get -PassThru
```
### Add  key to vault

```sh
Add-AzKeyVaultKey -Name "RG01KeyVaultDiskEncryptionKey" -VaultName "RG01KeyVault" -Destination "HSM"

Get-AzKeyVaultKey -VaultName "RG01KeyVault" -Name RG01KeyVaultDiskEncryptionKey
```

### Enable soft deletion for a Key Vault

```sh
($vault = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName $keyVaultName).ResourceId).Properties ` | Add-Member -MemberType NoteProperty -Name enableSoftDelete -Value 'True' 

Set-AzResource -resourceid $vault.ResourceId -Properties $vault.Properties -Force
```

■   --enable-soft-delete If a secret or an entire vault is deleted, you can recover it for up to 90 days after deletion.
■   --enable-purge-protection If a secret or an entire vault is deleted and has gone into a soft-delete, it cannot be purged until the 90-day period because deletion has passed.


### Encrypts an existing Virtual Machine

```sh
$keyVault = Get-AzKeyVault -VaultName "RG01KeyVault" -ResourceGroupName "RG01"; 
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri; 
$keyVaultResourceId = $keyVault.ResourceId; 
$keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName "RG01KeyVault" -Name RG01KeyVaultDiskEncryptionKey).Key.kid;
 
Set-AzVMDiskEncryptionExtension -ResourceGroupName "RG02" -VMName "WVM01" -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $keyVaultResourceId  
```

## App Service plan

```sh
az appservice plan create --name ShayneServicePlan --sku B1 --is-linux -g RG01 	
az webapp create -n shayneapp -g RG01  -p ShayneServicePlan -r "node|8.11"
```
download sample app:https://github.com/Azure-Samples/nodejs-docs-hello-world

```sh
az webapp deployment source config-zip -n shayneapp --src nodejs-docs-hello-world-master.zip -g RG01
az webapp browse -n shayneapp
az webapp show -n shayneapp -g RG01 -o json
```
## Monitoring

```sh
$SelectedVm = Get-AzVm -ResourceGroupName "RG02" -Name WVM01

Set-AzDiagnosticSetting -ResourceId $SelectedVm.Id -ServiceBusRuleId serbuazh740 -Enabled $true

Set-AzDiagnosticSetting -ResourceId $keyVault.ResolurceId -StorageAccountId $storageaccount.Id -Enabled $true -Category AuditEvent
```

## Azure AD

### Disable the Azure AD Sync Scheduler service.

```sh
Set-ADSyncScheduler -SyncCycleEnabled $false
```

### Returns tenant users who have not registered for MFA

```sh
Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName
```

### Managed Service Identity

Create the managed identity
Create keyvault and assign the identity
Create VM and login

Get the token: 
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -H Metadata:true

Get the secret value from vault:
curl https://kvvaultexample.vault.azure.net/secrets/kvsecretexample/b8f1xxxxxxxxxxxxxxxx99c?api-version=2016-10-01 -H "Authorization: Bearer eyJxxxxx"

## Networking

### Adds a global custom error page to the existing Azure Application Gateway for a status code of 502
```sh
$updatedgateway = Add-AzApplicationGatewayCustomError -ApplicationGateway $appgw -StatusCode HttpStatus502 -CustomErrorPageUrl $customError502Url 
```
### Create an IP configuration for an application gateway. 

```sh
New-AzApplicationGatewayIPConfiguration
```

### The New-AzApplicationGatewayFrontendIPConfig is used when creating the front-end IP configuration.

```sh
New-AzApplicationGatewayFrontendIPConfig
```

### The New-AzApplicationGatewayBackendAddressPool is used when configuring the back-end IP address pool.

```sh
New-AzApplicationGatewayBackendAddressPool
```

### The New-AzApplicationGatewayFrontendPort is used when configuring the front-end IP port. 

```sh
New-AzApplicationGatewayFrontendPort
```

### List of supported connectivity providers for setting up ExpressRoute

```sh
Get-AzExpressRouteServiceProvider
```

## RBAC

### Get operations provided by providers

```sh
Get-AzProviderOperation "Microsoft.Compute/virtualMachines/*" | select Operation,OperationName
```

### Create a new AD Group and assign role

```sh
New-AzureADGroup -DisplayName "RBAC Tutorial Group" -MailEnabled $false -SecurityEnabled $true -MailNickName "TutorialGroup"

Get-AzureADGroup  -SearchString "RBAC"
$groupId = "<new-group-id>" 
$subScope="/subscriptions/<subscription-id>"

New-AzRoleAssignment -ObjectId $groupId   -RoleDefinitionName "Reader"   -Scope $subScope
New-AzRoleAssignment -ObjectId $groupId   -RoleDefinitionName "Contributor"   -Scope $subScope
New-AzRoleAssignment -ObjectId $groupId -RoleDefinitionName Contributor  -ResourceGroupName "RG01"

```
### Get Role Assignment on resource

```sh
Get-AzRoleAssignment -ObjectId $groupId -Scope $subScope
```
### Remove role assignment

```sh
Remove-AzRoleAssignment -ObjectId $groupId -RoleDefinitionName "Reader" -Scope $subScope
```
### Remove AD group

```sh
Remove-AzureADGroup -ObjectId $groupId
```

### Create custom role


```sh
Option #1

$customRole = get-azroledefinition | where { $_.name -eq "Virtual Machine Contributor" }
$customRole.id = $null
$customRole.name = "Custom - Virtual Machine Administrator"
$Customrole.Description = "Can manage and access virtual machines"
$customRole.Actions.Add("Microsoft.Compute/VirtualMachines/*/read")
$customRole.AssignableScopes.Clear()
$CustomRole.AssignableScopes = "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>"

New-AzRoleDefinition -role $CustomRole
```


Option #2

```sh
Get-AzRoleDefinition "Virtual Machine Contributor" | ConvertTo-Json
Copy json into text editor and modify 
New-AzRoleDefinition -InputFile "C:\CustomRoles\ReaderSupportRole.json"
```

### Remove Role and scope
```sh
Remove-AzRoleAssignment -SignInName ajachoud@publicisgroupe.net -RoleDefinitionName "Contributor" -Scope $subScope
```

## Azure AKS

### Create a Kubernetes cluster with an existing SSH public key
```sh
az aks create -g MyResourceGroup -n MyManagedCluster --ssh-key-value /path/to/publickey
```

### Upload image to Azure Container Registry (ACR)

```sh
Step 1: Sample app: git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
Step 2: docker-compose up -d  //
Step 3: docker images//docker ps
Step 4: az login in cli and create resource groupe 

az group create --name AZContainerRG --location eastus

Step 5: Create Azure component registry

az acr create --resource-group AZContainerRG --name AZDemoACR007 --sku basic
az acr list

Step 6: Login into ACR registry

az acr login --name AZDemoACR007

Step 7: Tag image with acr/image name

docker tag azure-vote-front azdemoacr007.azurecr.io/azure-vote-front:v1

Step 8: push image into ACR

docker push azdemoacr007.azurecr.io/azure-vote-front:v1

Step 9: list images into ACR

az acr repository list --name AZDemoACR007

Step 10: show tags in registry

az acr repository show-tags --name AZDemoACR007 --repository azure-vote-front --output table

Step 11: Service principal access 	

service princpal so that cluster can access other azure resources
az ad sp create-for-rbac --skip-assignment
az acr show --resource-group AZContainersRG --name AZDemoACR007
az role assignment create --assignee <appid> --scope <scope> --role Reader


Step 11: 
create Kuberneteas cluster: AZDemoACRCluster007 
and Service Princiap: AZDemoACRClusterSp007/ Managed Identity
and give readonly permission to cluster to download image from ACR

deploy service

kubectl apply -f azure-vote-all-in-one-redis.yaml
kubectl get service azure-vote-front --watch

```

### Get the container log
```sh
az container logs --resource-group AZContainersRG --name AZDemoACR007
```

## Cosmos Db

### Create an Azure Cosmos account, database and container
https://docs.microsoft.com/bs-latn-ba/azure/cosmos-db/scripts/cli/sql/create?toc=/cli/azure/toc.json

### Change throughput
https://docs.microsoft.com/bs-latn-ba/azure/cosmos-db/scripts/cli/sql/throughput?toc=/cli/azure/toc.json

### Add or failover regions
https://docs.microsoft.com/bs-latn-ba/azure/cosmos-db/scripts/cli/common/regions?toc=/cli/azure/toc.json


### Account keys and connection strings 
https://docs.microsoft.com/bs-latn-ba/azure/cosmos-db/scripts/cli/common/keys?toc=/cli/azure/toc.json


### Secure with IP firewall
https://docs.microsoft.com/bs-latn-ba/azure/cosmos-db/scripts/cli/common/ipfirewall?toc=/cli/azure/toc.json


### Secure new account with service endpoints
https://docs.microsoft.com/bs-latn-ba/azure/cosmos-db/scripts/cli/common/service-endpoints?toc=/cli/azure/toc.json

### Secure existing account with service endpoints
https://docs.microsoft.com/bs-latn-ba/azure/cosmos-db/scripts/cli/common/service-endpoints-ignore-missing-vnet?toc=/cli/azure/toc.json


### Lock resources from deletion
https://docs.microsoft.com/bs-latn-ba/azure/cosmos-db/scripts/cli/sql/lock?toc=/cli/azure/toc.json


## SQL

```sh
Set-AzSqlDatabase
```

```sh
Set-AzSqlElasticPool - modifies properties of an elastic pool For example, use the StorageMB property to modify the max storage of an elastic pool.
```

```sh
Get-AzSqlDatabase - retrieves one or more databases.
```

```sh
New-AzSqlElasticPool - creates an elastic pool.
```

## Others

Get-AzLocation | select Location
Get-AzureADGroup  -SearchString ""
Get-AzSubscription
$Msolcred = Get-credential
invoke-restmethod -Uri
