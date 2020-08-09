
# Azure Migration

Azure Migrate offers a central hub to discover, assess, and migrate workloads to Azure. Azure Migrate supports assessment and migration of on-premises VMware VMs, Hyper-V VMs, physical servers, other virtualized VMs, databases, web apps, and virtual desktops.

## Assessment tools

- **Azure Migrate**:  is the tool that helps migrate  virtual machines, or physical servers from own data centre into Azure. The tool allows to migrate after you have done an environment assessment, or even without an assessment.
	- Features migration-specific capabilities (support for different types of workloads, agentless migration, integration with assessment tools, etc.). This is part of the Azure Migrate solution.

- **Azure Database Migration Service** is for migrating databases to Azure PaaS database services.
	[Data Migration Assistant](https://docs.microsoft.com/en-us/sql/dma/dma-overview?view=ssdt-18vs2017)

- **Service Map** - Azure Migrate uses Service Map to show dependencies between machines that the company wants to migrate.
[Service Map](https://docs.microsoft.com/en-us/azure/operations-management-suite/operations-management-suite-service-map)

**VMware converter** will not migrate VMs to Azure as it is a tool that performs Physical-to-Virtual (P2V) or virtual-to-virtual (V2V) within on-premises environments. "
	- VMWare assessment only (for Hyper-V use ASR deployment planner)
	- Upto 1500 VMs in a single discovery and project)
	- For larger environments, split the discovery into multiple assessments. You can execute upto 20 projects per a subscription
	- Projects can be only be created in the US Regions. Metadata is stored in West central US or East US"

**Comfort factor** - The buffer used during assessment. It's applied to the CPU, RAM, disk, and network utilization data for VMs. It accounts for issues like seasonal usage, short performance history, and likely increases in future usage.

For example, a 10-core VM with 20% utilization normally results in a two-core VM. With a comfort factor of 2.0, the result is a four-core VM instead.
https://docs.microsoft.com/en-us/azure/migrate/concepts-assessment-calculation"

**Port requirement - 443**
	- Collector Communicate with Azure Migrate Service on 443
	- Collector communicate with Vcenter Server on 443 (but can be updated)
	- On-Premise VM communicate with Log Analytiscs Workspace on 443"

**General**
	- Assess on-premises VM for Azure Migrate (VMWare only)
	- Provides size recommendations, monthly costs estimates, and visualize dependencies
	- Only create in East US and Central West US
	- OVA installed on VMWare Server
		- Open Virtualization Appliance(OVA) - An **OVA file** is an Open Virtualization Appliance that contains a compressed, "installable" version of a virtual machine. When you open an **OVA file** it extracts the VM and imports it into whatever virtualization software you have installed on your computer.
		- Open Virtualization Format (**OVF**) is an open standard for packaging and distributing virtual appliances or, more generally, software to be run in virtual machines.
	- Comfort factor can be adjust by 1.3x"

## High-level Steps

1) Set up the source and target environment for migration
2) Set up a replication policy
3) Enable replication
4) Run a test migration to make sure everything is working as expected
5) Run a one-time failover to Azure "

![enter image description here](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/migrate/azure-best-practices/media/contoso-migration-assessment/migration-assessment-architecture.png)
