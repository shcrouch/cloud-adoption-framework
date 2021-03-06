---
title: Enterprise enrollment and Azure Active Directory tenants
description: Enterprise enrollment and Azure Active Directory tenants
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
---

# Enterprise enrollment and Azure Active Directory tenants

## Planning for enterprise enrollment

An enterprise enrollment, often referred to as an Enterprise Agreement, represents the commercial relationship between Microsoft and how the customer uses Azure. It provides the basis for billing across all customer subscriptions and impacts administration of the customer estate. Enterprise enrollment, also known as EA, is managed via an Azure enterprise portal. Azure enterprise enrollment often represents an organization's hierarchy, including departments, accounts, and subscriptions. This hierarchy represents cost-enrollment groups within an organization.

![Azure EA hierarchies](./media/ea.png)
_Figure 1: An Azure enterprise enrollment hierarchy._

- Departments help to segment costs into logical groupings and to set a budget or quota at the department level (note: the quota isn't enforced firmly and is used for reporting purposes).

- Accounts are organizational units in the Azure enterprise portal; they can be used to manage subscriptions and access reports.

- Subscriptions are the smallest unit in the Azure enterprise portal. They're containers for Azure services managed by the service administrator. They're where an organization deploys Azure services.

- Enterprise enrollment roles link users with their functional role. These roles are:
  - Enterprise administrator
  - Department administrator
  - Account owner
  - Service administrator
  - Notification contact

**Design considerations:**

- The enrollment provides a hierarchical organizational structure to govern the management of customers subscriptions.

- Multiple customers environments can be separated at an EA-account level to support holistic isolation.

- There can be multiple administrators appointed to a single enrollment.

- Each subscription must have an associated account owner.

- Each account owner will be made a subscription owner for any subscriptions provisioned under that account.

- A subscription can only belong to one account at any given time.

- A subscription can be suspended based on a specified set of criteria.

**Design recommendations:**

- Only use the authentication type `Work or school account` for all account types. Avoid using the `Microsoft account (MSA)` account type.

- Set up the notification contact email address to ensure notifications are sent to an appropriate group mailbox.

- Assign a budget for each account and establish an alert associated with the budget.

- An organization can have a variety of structures such as functional, divisional, geographic, matrix, or team structure. Use organizational structure to map organization structure to enterprise enrollment.

- Create a new department for IT if business domains have independent IT capabilities.

- Restrict and minimize the number of account owners within the enrollment to avoid the proliferation of admin access to subscriptions and associated Azure resources.

- If multiple Azure Active Directory (Azure AD) tenants are used, verify that the account owner is associated with the same tenant as where subscriptions for the account are provisioned.

- Set up Enterprise Dev/Test/production environments at an EA account level to support holistic isolation.

- Do not ignore notification emails sent to the notification account email address. Microsoft sends important EA-wide communications to this account.

- Do not move or rename an EA account in Azure AD.

- Periodically audit the EA portal to review who has access and avoid using a Microsoft account where possible.

## Define Azure AD tenants

An Azure AD tenant provides identity and access management, which is an important part of your security posture, to ensure that authenticated and authorized users have access to only the resources for which they have access permissions. Azure AD not only provides these services to applications and services deployed in Azure, but to services and applications deployed outside of Azure (such as on-premises or third-party cloud providers). Azure AD is also used by software as a service (SaaS) applications such as Microsoft 365 and the Azure Marketplace. Organizations already using on-premises Active Directory can use their existing infrastructure and extend authentication to the cloud by integrating with Azure AD. Each Azure AD directory has one or more domains. A directory can have many subscriptions associated with it, but only one Azure AD tenant.

It's important to ask basic security questions during the Azure AD design phase, such as how an organization manages credentials and how it controls human, application, and programmatic access.

**Design considerations:**

- Multiple Azure AD tenants can function in the same enterprise enrollment.

**Design recommendations:**

- Use Azure AD seamless single sign-on based on the selected [planning topology](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-topologies).

- If your organization doesn't have an identity infrastructure, start by implementing an Azure-ad-only identity deployment. Such deployment with [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services) and [Microsoft enterprise mobility + security](https://docs.microsoft.com/mem/intune/fundamentals/what-is-intune) provides end-to-end protection for SaaS and enterprise applications as well as for devices.

- Multi-factor authentication provides another layer of security and a second barrier of authentication. Enforce [multi-factor authentication](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks) and [conditional access policies](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) for all privileged accounts for greater security.

- Plan and implement for [emergency access](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access) or break-glass accounts to prevent tenant-wide account lockout.

- Use [Azure AD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) for identity and access management.

- If dev/test/production are going to be isolated environments from an identity perspective, then separate them at a tenant level via multiple tenants.

- Avoid creating a new Azure AD tenant unless there's a strong identity and access management justification and processes are already in place.
