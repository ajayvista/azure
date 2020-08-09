


# Azure AD



**Send Sign-In and Audit logs to Log Analytics**

Configure on Azure AD blade

Diagnostic Setting

Send to Log Analytics



**Enterprise State Roaming**

Requires AAD Premium

Synchronize user and app settings to Azure AD and encrypted with Azure RMS

Data retained for 90-180 days

Enable in Azure AD -> Devices -> Enterprise State Roaming



**Self-Service Password Portal**

- Enforce one or two authentication methods

- Option to require user registration and set re-confirmation from 0-730 days

- Notify users if password reset and all admins if any admin reset

- Write-back on-prem requires P1 or above

- Customize help desk link



**Authentication Methods**

- Mobile phone

- Office phone

- Email

- Security questions

- Mobile app code

- Mobile app notification



**Pricing Details**

- Free < 500,000 objects

- Basic -> SSPR, AAD Proxy

- P1 -> Advanced reports, write-back, MFA

- P2 -> Identity protection, PIM



- You can set a time period to allow authentication attempts after a user is authenticated by using the caching feature., You should configure a Caching Rule.



- Authorization code and Client_id will be required, client needs to successfully authenticate to a line of business application via an Application Programming Interface (API).



- As part of MFA, Fraud alert, PIN mode and One-Time Bypass are features that are available with a paid Azure AD subscription.



- Customers who utilize the free benefits of Azure AD can use security defaults to enable multi-factor authentication in their environment. These features are not available with MFA in the cloud.

- Azure AD is called multitenant because more than one Azure subscription can trust a single Azure AD tenant. By contrast, a single subscription can be associated with only one Azure AD instance at a time.

- Azure AD License Assignment Please remember that Azure AD editions are licensed per-user. That means that to use features like Azure AD PIM or password writeback, all involved users and administrators must be assigned the appropriate Azure AD license. You can assign licenses in many ways; one of the most straightforward is to navigate to the Licenses blade in your Azure AD tenant.



## Multi-Factor Authentication (MFA)



Azure Multi-Factor Authentication (MFA) helps safeguard access to data and applications while maintaining simplicity for users. It provides additional security by requiring a second form of authentication and delivers strong authentication via a range of easy to use authentication methods. Users may or may not be challenged for MFA based on configuration decisions that an administrator makes.



It works by requiring two or more of the following authentication methods:



- Something you know (typically a password)



- Something you have (a trusted device that is not easily duplicated, like a phone)



- Something you are (biometrics)



List of the features that are available in the various versions of Azure Multi-Factor Authentication.



- The one-time bypass feature allows a user to authenticate a single time without performing two-step verification. The bypass is temporary and expires after a specified number of seconds. In situations where the mobile app or phone is not receiving a notification or phone call, you can allow a one-time bypass so the user can access the desired resource.

Note:

- The user needs to sign in before the one-time bypass expires.

- Some applications, like Office 2010 or earlier and Apple Mail before iOS 11, don't support two-step verification. The apps aren't configured to accept a second verification. To use these applications you should Enable App Passwords.

- You should configure a Caching Rule. You can set a time period to allow authentication attempts after a user is authenticated by using the caching feature.

- App Password is used to configure apps that don't work with MFA.

- Configuring Conditional Access and MFA will allow you to specify security requirements for when users attempt to logon from locations outside of the US and also ensure that users use another factor such as security questions.



MFA verification options fo users:

- Call to phone

- Text message to phone

- Notification through mobile app

- Verification code from mobile app or hardware token



Password Reset Methods available to users:

- Mobile app notification

- Mobile app code

- Email

- Mobile phone

- Office phone

- Security questions

-

The use of MFA in Azure AD requires the use of a paid SKU for Azure AD.



■ **Account Lockout Settings** available to ensure incorrect logon attempts will trigger the account to be locked out and options for them to reset on their own.



■ **Block/Unblock Users** Add users to the block list who will be unable to log in. This will set all authentication attempts to be denied and remain in place for 90 days from the first blocked login.



■ **Fraud Alert** Configure fraud reporting for users receiving a second factor request they aren’t expecting and configure fraud reporting users be blocked on report of fraud.



■ **Notifications** Enter the recipient email address who will receive alerts generated by MFA.



The best practice here would be to use a distribution group for notification.



■ **OATH Tokens** Configure the secret keys (via upload) for associated hardware tokens to allow use of a hardware token as a second factor.



■ **Phone Call Settings** Configure the caller ID number (United States only), whether an operator is required to transfer for extensions, the number of pin attempts allowed per call, and greetings used with the phone calling service.



■ **Providers** The provider settings were disabled as of September 1, 2018.



■ **MFA Server Settings** These settings bring the functionality of Azure AD MFA to an on-premises datacenter.



■ **Server Settings** The number of seconds before the two-factor code times out.



■ **One-Time Bypass** A list of user accounts that will bypass MFA authentication. The default bypass is for 300 seconds.



■ **Caching Rules** Configure MFA so that consecutive authentications don’t repeat MFA authentication.



■ **Server Status** Reports the status of an on-premises MFA server.



■ **Activity Report** Shows activity against Azure AD MFA and a connected MFA server.



Another option available on the app settings page for MFA allows users to have their devices remembered by MFA for a set number of days. Check the box at the bottom of the app settings screen and set the number of days (between 1 and 60).



## Active Directory Self-Service Password Reset (SSPR)



- Azure Active Directory Self-Service Password Reset (SSPR) can only be setup with a maximum of 2 methods.

- The securest methods available are to use a Mobile App code and Security Questions or email.

- Using a text SMS is the least secure method.



## AD Privileged Identity Management



For Identity Governance use Azure AD Privileged Identity Management. �PIM essentially helps you manage the who, what, when, where, and why for resources �that you care about. ***Key features of PIM include: Conduct access reviews to ensure users still need roles.***



**PIM Roles**

PIM Administrator -> manage role assignments in Azure AD and all aspects of PIM

Security Administrator -> read info and report and manage AD and O365



**General**

Enable PIM must be global admin and becomes PIM Administrator



**PIM and Azure RBAC**

Subscription must be enrolled in RBAC

Role assignment settings are eligible and active



**RBAC Settings**

Allow role to be permanent (eligible or active)

Set time for how long eligible or active

Require MFA

Require justification

Require approval



## AD Connect



- Pass-through authentication (PTA) and single sign-on (SSO) will provide a hybrid configuration between an on-premises environment and Azure Active Directory.

- Federation (AD FS) requires commissioning on-premises servers.

- Password hash synchronization (PHS) with SSO stores hashed passwords in the cloud.

- Password writeback is a feature that you can enable when you use Azure AD Connect that allows password changes in the Cloud to be written back to an existing on-premises directory in real time.

- �Extend your existing on-premises Active Directory infrastructure to Azure, by deploying a VM in Azure that runs AD DS as a �Domain Controller. This architecture is more common when the on-premises network and the Azure virtual network (VNet) are connected by a VPN or ExpressRoute connection. Use Azure AD to create an Active Directory domain in the cloud and connect it to your on-premises Active Directory domain.

- Azure AD Connect integrates your on-premises directories with Azure AD.

- On-premises network and the Azure virtual network (VNet) are connected by a VPN or ExpressRoute connection.



Note: In staging mode, the server is active for import and synchronization, but it does not run any exports. A server in staging mode is not running password sync or password writeback, even if you selected these features during installation. When you disable staging mode, the server starts exporting, enables password sync, and enables password writeback.



Note now the Enable Single Sign-On check box. If this is selected and Password Synchronization or Pass Through Authentication is also selected, with some more configuration steps, single sign-on capabilities of IWA can be available to your customers’ applications in the cloud.



Azure AD Global Admin credentials are only used during the installation and are not used after the installation has completed. It is used to create the Azure AD Connector account used for synchronizing changes to Azure AD. The account also enables sync as a feature in Azure AD.



The Azure Active Directory Connect Wizard automatically downloads and installs Microsoft SQL Server Express LocalDB for use as a back-end data store. At this point the utility installs the synchronization service; this is the “engine” that powers hybrid identity with Azure AD.



The sign-on methods are



■ Password Hash Synchronization This is the simplest option; each on-premises AD identity has an Azure AD representation, and Azure AD Connect keeps the passwords in sync. Specifically, Azure AD Connect maintains user account password hashes both in local AD and Azure AD.



■ Pass-Through Authentication This method is an alternative to password hash synchronization. With pass-through authentication, no password hashes are kept in Azure AD; this suits businesses whose security requirements prevent cloud-based password hash storage. Note that this sign-on method requires the deployment of agent software on-premises.



■ Federation With AD FS Azure AD Connect can largely automate the deployment of an Active Directory Federation Services (AD FS) farm to support token-based SSO.



■ Federation With PingFederate is a third-party alternative to AD FS; this option is aimed at businesses that already use PingFederate for token-based SSO.



- Must have both Azure AD Global Administrator and local AD Enterprise Administrator credentials to complete the configuration.



Here’s what the different source IDs mean:



■ Azure Active Directory This is a cloud-native identity that was created directly in the Azure AD tenant.

■ External Azure Active Directory This is an identity from another Azure AD tenant who was invited to the local directory via Active Directory Business-to-Business collaboration (Azure AD B2B).

■ Invited User This is an Azure AD B2B guest user who has not yet accepted the invitation to join the local directory.

■ Microsoft Account External Azure AD B2B guests become Microsoft accounts after they redeem their invitation.

■ Windows Server AD This is a local Active Directory identity that has been synchronized to the Azure AD tenant.



Configure Azure AD Connect High Availability

The Azure AD Connect Additional Tasks screen has an option called Configure Staging Mode. To provide some redundancy to your Azure AD Connect deployment, install Azure AD Connect on at least one other domain member server, but configure it for staging mode.

The secondary Azure AD Connect instance remains in “standby”; if the primary Azure AD Connect server goes offline, bring the standby out of staging mode to have it resume identity synchronization as usual.



By default, the Azure AD Sync scheduler runs every 30 minutes. You can find the scheduler service in the Service Control Manager list under Microsoft Azure AD Sync -



```sh

Set-ADSyncScheduler -SyncCycleEnabled $false

```



```sh

//Change Azure AD Connector Password

Add-ADSyncAADServiceAccount

```

Here is a super high-level overview of the authentication flow:

1. A user accesses an Azure AD-based application and types his or her email address (which we know, behind the scenes, represents the user’s on-premises AD UPN).

2. Because a federated trust exists, Azure AD redirects the user to the local AD FS farm, where the user may or may not be prompted for their password depending on the environment specifics.

3. User authentication happens strictly locally within AD; the AD FS security token service generates a vendor-neutral token that makes claims about the user’s identity, group memberships, and possibly other metadata.

4. The AD FS farm presents the token to Azure AD. Azure AD trusts local AD via the federation, so the user is granted access to the application. Do you see how that works? At most, all the user needs to provide to the Azure AD application is the username; no password hashes or Kerberos tickets are exchanged over the network connection.

5.

Azure AD Connect provides three other graphical utilities:

■ Synchronization Service Manager View and configure synchronization service properties.

■ Synchronization Service Web Service Configuration Tool Control the web services connection between local AD and Azure AD.

■ Synchronization Rules Editor Customize the mapping between local AD schema attributes and Azure AD user and group account attributes.




**How to perform an access review to audit Azure AD group membership or access to an Azure AD application**. It’s a bit confusing, but you configure access reviews in two different Azure portal locations, depending on what exactly you’re reviewing:

■ For Azure AD groups or applications Use the Access Reviews blade.

■ For Azure AD or Azure resource roles Use the Azure AD Privileged Identity Management blade.



## Azure AD Identity Protection (IdP)



Azure AD Identity Protection (IdP) is an Azure AD Premium P2 feature that enables you to detect potential vulnerabilities affecting your Azure AD identities and potentially configure automatic remediation for suspicious events (for example, attempted logons for the same user account from two geographical locations simultaneously).



Azure AD Identity Protection is an Azure AD Premium P2 feature that uses Microsoft’s Intelligent Security Graph to detect potential vulnerabilities with your user identities.



With respect to Azure AD IdP configuration, you can configure three policy types:

■ MFA registration policy Require your users to specify additional authentication methods to protect their accounts.

■ User risk policy Set a minimum user behavior risk threshold that triggers the policy; the policy result either blocks or allows sign-in to your Azure AD-backed apps.

■ Sign-in risk policy Set a minimum sign-in risk level to trigger the policy that, in turn, blocks or allows sign-in.



## Azure AD Join

- Azure AD Join is what it sounds like to you: the capacity of joining Windows 10 endpoint devices to your Azure AD tenant. You can join Windows 10 devices to Azure AD with any Azure AD SKU; however, **advanced features such as mobile device management (MDM) auto-enrollment require Azure AD Premium P1 or P2 licensing.**



- Enterprise State Roaming (an Azure AD Premium P1 or P2 feature) roughly as a public cloud equivalent of roaming user profiles in on-premises Active Directory.The idea here is your users with their Windows 10 Azure AD–joined devices can synchronize their user and application settings data to the cloud, and have the data be available to all their Windows devices.



Follow these steps to understand the Enterprise State Roaming “big picture”:

1. In your Azure AD tenant, browse to Devices, and then click Enterprise State Roaming.

2. Set the Users May Sync Settings And App Data Across Devices property to either All or Selected, depending on whether you want to enable Enterprise State Roaming for all or selected Azure AD users.

3. Click Save to complete the configuration.



- Azure AD Join enables you to join Windows 10 devices to your Azure AD tenant and optionally manage them by using Microsoft Intune or another mobile device management (MDM) platform.

The classic portal feature Edit Directory, that allows you to associate an existing subscription to your Azure Active Directory (AAD), is now available in Azure portal. It used to be available only to Service Admins with Microsoft accounts, but now it's available to users with AAD accounts as well.

When you connect a Windows device with Azure AD using an Azure AD join, Azure AD adds the following security principles to the local administrators group on the device: The Azure AD global administrator role

To delete a custom domain name, you must first ensure that no resources in your directory rely on the domain name. You can't delete a domain name from your directory if:


- Any user has a user name, email address, or proxy address that includes the domain name.

- Any group has an email address or proxy address that includes the domain name.

- Any application in your Azure AD has an app ID URI that includes the domain name.


## Azure Advanced Threat Protection (ATP)

Enables SecOp analysts and security professionals struggling to detect advanced attacks in hybrid environments to:

- Monitor users, entity behavior, and activities with learning-based analytics
- Protect user identities and credentials stored in Active Directory
- Identify and investigate suspicious user activities and advanced attacks throughout the kill chain
- Provide clear incident information on a simple timeline for fast triage

Azure ATP alerts.
A blob service and table service


## RBAC

**General**
Deny assignments block users from performing specified actions even if a role assignment grants them access.
Max of 2000 role assignments per subscription
Allow only, must use deny assignments to explicity deny something
Three authZ models: classic subscription administration roles, Azure RBAC roles, Azure AD Admin roles
Owner, contributor, reader, user access administrator
Azure AD Global Admin can take control of subs by settings "Global Admin can manage Azure subs and Management Groups" and become User Access Admin for all subs in tenant

**Role Assignments**
Security principal -> user, group, service principal, managed identity
Role definition (role) -> actions, notactions, dataactions, notdataactions
Scope -> management group, subscription, resource group, resource

```sh
PS - Get-AzRoleDefinitions,
Get-AzRoleAssignments
CLI - az role definition list

//Removes the Reader role assignment for the user user1@cycleshare.com at the subscription scope.

Remove-AzRoleAssignment -SignInName user1@cycleshare.com `
    -RoleDefinitionName "Reader" `
    -Scope $subScope

This cmdlet will return a list of tenant users who have not registered for MFA.
Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName
```

RBAC - Classic Subscription Administrative Roles
Account used to sign-up for Azure is Account Administrator/Service Administrator
Account Administrator (1), Service Administrator (1), Co-administrator (200)
Account Administrator -> billing owner, manage sub lifecycle
Service Administrator
Co-Administrator -> all but change Service Admin and associate sub w/ different directory

A **Helpdesk Administrator** can change passwords, invalidate refresh tokens, manage service requests, and monitor service health. Users with this role can change passwords for people who may have access to sensitive or private information or critical configuration inside and outside of Azure Active Directory. Changing the password of a user may mean the ability to assume that user's identity and permissions.

A user with **Password Administrator** role have limited ability to manage passwords. This role does not grant the ability to manage service requests or monitor service health. Password administrators can reset passwords of other users who are non-administrators or members of the following roles only:

Directory Readers
Guest Inviter
Password Administrator

A **Global Administrator** can reset the password for any user and all other administrators

A **User Administrator** can create users, and manage all aspects of users, and can update password expiration policies. Additionally, users with this role can create and manage all groups. This role also includes the ability to create and manage user views, manage support tickets, and monitor service health.

**Security Administrator** is a role used for administering security in Azure.

**Conditional Access Administrator** is used for configuring Conditional Access.

**Virtual Machine Contributor** is used for managing Virtual Machines.

The **Security Admin role**: In Security Center only: Can view security policies, view security states, edit security policies, view alerts and recommendations, dismiss alerts and recommendations. The Security Admin cannot deploy virtual networks.

**Network Contributor**, can create and manage networks, but not access to them.

**Owner** can create and manage resources of all types.

The **DevTest Labs User** role lets you connect, start, restart, and shutdown your virtual machines in your Azure DevTest Labs.

The **Logic App Contributor** role lets you read, enable and disable logic app.  The Contributor role lets you manage everything except access to resources. It allows you to create and manage resources of all types, including creating Azure logic apps.

```sh
To create a custom role you need to In PowerShell run the New-AzRoleDefinition with a JSON file with the parameters of the role.

For example:

New-AzRoleDefinition -InputFile ""C:\CustomRoles\ReaderSupportRole.json"""
```
You can create role assignments using the Azure portal, Azure CLI, Azure PowerShell, Azure SDKs, or REST APIs. To create and remove role assignments, you must have **Microsoft.Authorization/roleAssignments/* permission.** This permission is granted through the Owner or User Access Administrator roles.

﻿The "**Microsoft.Support/***" operation will allow the user to create support tickets.

Azure AD Privileged Identity Management (PIM) enables you to manage, control, and monitor access to important resources in your organization. When you configure PIM you can give just-in-time and just enough access for your privileged accounts.

**Service principal** - A security identity used by applications or services to access specific Azure resources. You can think of it as a user identity (username and password or certificate) for an application.

**Managed identity** - An identity in Azure Active Directory that is automatically managed by Azure. You typically use managed identities when developing cloud applications to manage the credentials for authenticating to Azure services.  An identity in Azure Active Directory that is automatically managed by Azure describes a managed identity security principal.

Keep in mind that using a group for role assignments is much lower maintenance than individually assigning users to roles.

A management group in Azure is a resource that can cross subscription boundaries and allow a single point of management across subscriptions.

RBAC access is cumulative by default, meaning contributor access at the subscription level is inherited by resource groups and resources housed within a subscription. Inheritance is not required because permission can be granted at lower levels within a subscription all the way down to the specific resource level. In addition, permission can also be denied at any level; doing so prevents access to resources where permission was denied. If denial of permissions happens at a parent resource level, any resources underneath the parent will inherit the denial.

Adding a Coadministrator This is only necessary if Classic deployments are being used (the classic portal). Assigning Owner RBAC rights in the Resource Management portal achieves the same result.

Changing Management Groups may Require Permission Review When moving items from one management group to another, permissions can be affected negatively. Be sure to understand the impact of any changes before they are made to avoid removing necessary access to Azure resources.

Although the intent may be to ensure, for example, that all resources are created in a specified region within Azure, remember to overcommunicate any enforcement changes to those using Azure. The enforcement of policy generally happens when the Create button is clicked, not when the resource is discovered to be in an unsupported region.

Authorization code and Client_id will be required.

## Azure Policy

Policies in Azure help to ensure that resources can be audited for compliance and deployment controlled as required by the organization.

Azure Policy is a service you can use to create, assign, and manage policies. These policies apply and enforce rules that your resources need to follow. These policies can enforce these rules when resources are created, and can be evaluated against existing resources to give visibility into compliance.

These policies enforce different rules and effects over your resources so that those resources stay compliant with your corporate standards and service level agreements. Azure Policy meets this need by evaluating your resources for noncompliance with assigned policies.

Policy assignments are inherited by all child resources. This inheritance means that if a policy is applied to a resource group, it is applied to all the resources within that resource group. However, you can exclude a subscope from the policy assignment. For example, we could enforce a policy for an entire subscription and then exclude a few select resource groups.

Defining initiatives
Initiative definitions simplify the process of managing and assigning policy definitions by grouping a set of policies into a single item

**Azure Management Groups**

Azure Management Groups are containers for managing access, policies, and compliance across multiple Azure subscriptions. Management groups allow you to order your Azure resources hierarchically into collections, which provide a further level of classification that is above the level of subscriptions. All subscriptions within a management group automatically inherit the conditions applied to the management group. Management groups give you enterprise-grade management at a large scale no matter what type of subscriptions you might have.

Default allow and explicit deny
Assigned at management group, subscription, and resource group
Inherited unless excluded
Audit, deny, or deploy

Collections of policy definitions, called initiatives, are used to group like policy definitions to help achieve a larger goal of governance rather than assigning 10 policy definitions separately. To hit this goal, they can be grouped into one initiative.

If a policy runs against a scope and finds items in noncompliance, it may require remediation tasks to be performed. These tasks are listed under the remediation section in the Policy blade, and they’re only applicable to policies that will deploy resources if they are not found.


## Trusted Execution Environments (TEE)

A TEE can be at the hardware or software (hypervisor) level. In hardware, it’s part of the CPU instruction set. A TEE implementation gives an application the possibility to execute sections of code within a protected area called an enclave. The enclave pretty much acts like a bubble, protecting the application from the host machine.

Use cases that can be fulfilled currently within Azure are
- DC-series VMs running Open Enclave SDK, TEE enabled applications.
- SQL Server (2019+ IaaS) Always Encrypted with secure enclaves running on a DC-series VM. (Brings complex SQL searching, not currently available in Azure SQL.)
- Multi-source machine learning
