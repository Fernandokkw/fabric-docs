---
title: Security in Microsoft Fabric
description: Learn how Microsoft Fabric security works, and what features are available.
author: KesemSharabi
ms.author: kesharab
ms.topic: overview
ms.date: 02/05/2024
---

# Security in Microsoft Fabric

[Microsoft Fabric](../get-started/microsoft-fabric-overview.md) is a software as a service (SaaS) platform that lets users get, create, share, and visualize data.

As a SaaS service, Fabric offers a complete security package for the entire platform. Fabric removes the cost and responsibility of maintaining your security solution, and transfers it to the cloud. With Fabric, you can use the expertise and resources of Microsoft to keep your data secure, patch vulnerabilities, monitor threats, and comply with regulations. Fabric also allows you to manage, control and audit your security settings, in line with your changing needs and demands.

As you bring your data to the cloud and use it with various analytic experiences such as Power BI, Data Factory, and the next generation of Synapse, Microsoft ensures that built-in security and reliability features secure your data at rest and in transit. Microsoft also makes sure that your data is recoverable in cases of infrastructure failures or disasters.

Fabric security is:

* Continuous - Fabric security is always on. Because it's embedded in the cloud, it doesn't rely on a team of experts to keep it running.

* Configurable - You can configure Fabric security in accordance with your organizational policies.

* Automated - Many of the Fabric security features are automated. Once configured, these features continue to work in the background.

* Evolving - Microsoft is constantly improving Fabric security, by adding new features and controls.

## Authenticate

Microsoft Fabric is a SaaS platform, like many other Microsoft services such as Azure, Microsoft Office, OneDrive and Dynamics. All these Microsoft SaaS services including Fabric, use [Microsoft Entra ID](/entra/verified-id/decentralized-identifier-overview) as their cloud-based identity provider. Microsoft Entra ID helps users connect to these services quickly and easily from any device and any network. Every request to connect to Fabric is authenticated with Microsoft Entra ID, allowing users to safely connect to Fabric from their corporate office, when working at home, or from a remote location.

## Configure network security

Fabric is SaaS service that runs in the Microsoft cloud. Some scenarios involve connecting to data that's outside of the Fabric platform. For example, viewing a report from your own network or connecting to data that's in another service. Interactions within Fabric use the internal Microsoft network and traffic outside of the service is protected by default.

### Inbound network security

Your organization might want to restrict and secure the network traffic coming into Fabric based on your organization's requirements.

#### Microsoft Entra ID

[Microsoft Entra ID](/entra/verified-id/decentralized-identifier-overview) provides Fabric with a robust network inbound security. Every interaction with Fabric, including logging in, using the Power BI mobile app, and running SQL queries through SQL Server Management Studio (SSMS), is authenticated using Microsoft Entra ID.

With Microsoft Entra ID you can set up a [Zero Trust](/security/zero-trust/zero-trust-overview) security solution for Fabric. Zero Trust assumes that you're not safe within the compound of your organization's network security. The Zero trust approach believes that your organization is constantly under attack, and that you face a continuous security breach threat. To combat this on-going threat, Fabric enforces the use of Microsoft Entra ID authentication. With Microsoft Entra ID, identity is the security perimeter and users can't use other authentication means such as account keys, shared access signatures (SAS), SQL authentication (usernames and passwords).

Microsoft Entra ID provides Fabric with [Conditional Access](/entra/identity/conditional-access/overview) which allows you to secure access to your data. Here are a few examples of access restrictions you can enforce using Conditional Access.

* Define a list of IPs for inbound connectivity to Fabric.

* Use Multifactor Authentication (MFA).

* Restrict traffic based on parameters such as country of origin or device type.

To understand more about authentication in Fabric, see [Microsoft Fabric security fundamentals](security-fundamentals.md).

### Outbound network security

Fabric has a set of tools that allow you to connect to external data sources and bring that data into Fabric in a secure way. This section lists different ways to import and connect to data from a secure network into fabric.

#### On-premises data gateway

With an on-premises data gateway you can use [Data Flows Gen 2](../data-factory/dataflows-gen2-overview.md) to connect to data sources that are behind firewalls and virtual networks.

#### Connect to an existing service

You can connect to Fabric using your existing Azure Platform as a Service (PaaS) service. For Synapse and Azure Data Factory (ADF) you can use [Azure Integration Runtime (IR)](/azure/data-factory/concepts-integration-runtime) or [Azure Data Factory managed virtual network](/azure/data-factory/managed-virtual-network-private-endpoint). You can also connect to these services and other services such as Mapping data flows, Synapse Spark clusters, Databricks Spark clusters and Azure HDInsight using [OneLake APIs](../onelake/onelake-access-api.md).

#### Azure service tags

Use [service Tags](security-service-tags.md) to ingest data without the use of data gateways, from data sources deployed in an Azure virtual network, such as Azure SQL Virtual Machines (VMs), Azure SQL Managed Instance (MI) and EST APIs. You can also use service tags to get traffic from a virtual network or an Azure firewall. For example, service tags can allow outbound traffic to Fabric so that a user on a VM can connect to Fabric SQL endpoints from SSMS, while blocked from accessing other public internet resources.

#### IP allowlists

If you have data that doesn't reside in Azure, you can enable an IP allowlist on your organization's network to allow traffic to and from Fabric. An IP allowlist is useful if you need to get data from data sources that don't support service tags, such as Azure Data Lake Storage (ADLS) and on-premises data sources. With these shortcuts, you can get data without copying it into OneLake using a [Lakehouse SQL endpoint](../data-engineering/lakehouse-sql-analytics-endpoint.md) or [Direct Lake](/power-bi/enterprise/directlake-overview).

You can get the list of Fabric IPs from [Service tags on-premises](/azure/virtual-network/service-tags-overview#service-tags-on-premises). The list is available as a JSON file, or programmatically with REST APIs, PowerShell, and Azure Command-Line Interface (CLI).

## Secure Data

In Fabric, all data that is stored in OneLake is encrypted at rest. All data at rest is stored in your home region, or in one of your capacities at a remote region of your choice so that you can meet data at rest sovereignty regulations. For more information, see [Microsoft Fabric security fundamentals](security-fundamentals.md).

### Understand tenants in multiple geographies

Many organizations have a global presence and require services in multiple [Azure geographies](/azure/reliability/availability-zones-service-support). For example, a company can have its headquarters in the United States, while doing business in other geographical areas, such as Australia. To comply with local regulations, businesses with a global presence need to ensure that data remains stored at rest in several regions. In Fabric, this is called *multi-geo*.

The query execution layer, query caches, and item data assigned to a multi-geo workspace remain in the Azure geography of their creation. However, some metadata, and processing, is stored at rest in the tenant's home geography.

Fabric is part of a larger Microsoft ecosystem. If your organization is already using other cloud subscription services, such as Azure, Microsoft 365, or Dynamics 365, then Fabric operates within the same [Microsoft Entra tenant](/microsoft-365/education/deploy/intro-azure-active-directory#what-is-a-microsoft-entra-tenant). Your organizational domain (for example, contoso.com) is associated with Microsoft Entra ID. Like all Microsoft cloud services.

Fabric ensures that your data is secure across regions when you're working with several tenants that have multiple capacities across a number of geographies.

* **Data logical separation** - The [Fabric platform](security-fundamentals.md#fabric-platform) provide logical isolation between tenants to protect your data.

* **Data sovereignty** - To start working with multi-geo, see [Configure Multi-Geo support for Fabric](../admin/service-admin-premium-multi-geo.md).

## Access data

Fabric controls data access using [workspaces](../get-started/workspaces.md). In workspaces, data appears in the form of Fabric items, and users can't view or use items (data) unless you give them access to the workspace.

### Workspace roles

Workspace access is listed in the table below. It includes [workspace roles](../get-started/roles-workspaces.md) and [OneLake security](../onelake/onelake-security.md#workspace-security). Users with a viewer role can run SQL, Data Analysis Expressions (DAX) or Multidimensional Expressions (MDX) queries, but they can't access Fabric items or run a [notebook](../data-engineering/how-to-use-notebook.md)

| Role                           | Workspace access                       | OneLake access                                                        |
|--------------------------------|----------------------------------------|-----------------------------------------------------------------------|
| Admin, member, and contributor | Can use all the items in the workspace | :::image type="icon" source="../media/yes-icon.svg" border="false"::: |
| Viewer                         | Can see all the items in the workspace | :::image type="icon" source="../media/no-icon.svg" border="false":::  |

### Share items

You can [share Fabric items](../get-started/share-items.md) with users in your organization that don't have any workspace role. Sharing items gives restricted access, allowing users to only access the shared item in the workspace.

### Limit access

You can limit viewer access to data using [Row-level security (RLS)](/power-bi/enterprise/service-admin-rls), [Column level security (CLS)](../data-warehouse/column-level-security.md) and [Object level security (OLS)](/power-bi/enterprise/service-admin-ols). With RLS, CLS and OLS, you can create user identities that have access to certain portions of your data, and limit SQL results returning only what the user's identity can access.

You can also add RLS to a DirectLake dataset. If you define security for both SQL and DAX, DirectLake falls back to DirectQuery for tables that have RLS in SQL. In such cases, DAX, or MDX results are limited to the user's identity.

To expose reports using a DirectLake dataset with RLS without a DirectQuery fallback, use direct dataset sharing or [apps in Power BI](/power-bi/consumer/end-user-apps). With apps in Power BI you can give access to reports without viewer access. This kind of access means that the users can't use SQL. To enable DirectLake to read the data, you need to [switch the data source credential](/power-bi/enterprise/directlake-fixed-identity) from Single Sign On (SSO) to a fixed identity that has access to the files in the lake.

## Protect data

Fabric supports sensitivity labels from Microsoft Purview Information Protection. These are the labels, such as *General*, *Confidential*, and *Highly Confidential*, that are widely used in Microsoft Office apps such as Word, PowerPoint, and Excel to protect sensitive information. In Fabric, you can classify items that contain sensitive data using these same sensitivity labels. The sensitivity labels then follow the data automatically from item to item as it flows through Fabric, all the way from data source to business user. The sensitivity label follows even when the data is exported to supported formats such as PBIX, Excel, PowerPoint, and PDF, thus ensuring that your data remains protected. Only authorized users will be able to open the file. For more information, see [Governance and compliance in Microsoft Fabric](../governance/governance-compliance-overview.md).

To help you govern, protect, and manage your data, you can use [Microsoft Purview](../governance/microsoft-purview-fabric.md). Microsoft Purview and Fabric work together letting you store, analyze, and govern your data from a single location, the [Microsoft Purview hub](../governance/use-microsoft-purview-hub.md).

## Recover data

Fabric data resiliency ensures that your data is available in case of a disaster. Fabric also enables you to recover your data in case of a disaster, Disaster recovery. For more information see [Reliability in Microsoft Fabric](/azure/reliability/reliability-fabric).

## Administer Fabric

As an [administrator in Fabric](../admin/admin-overview.md), you get to control capabilities for the entire organization. Fabric enables delegation of the admin role to capacities, workspaces and domains. By delegating admin responsibilities to the right people, you can implement a model that lets several key admins control general Fabric settings across the organization, while other admins who are in charge of settings related to specific areas.

Using a variety of tools, admins can also [monitor](../admin/admin-overview.md#monitor) key Fabric aspects such as capacity consumption. You can also [view audit logs](../admin/track-user-activities.md) to monitor user activities and investigate unexpected incidents if needed.

## Capabilities

Review this section for a list of some of the security features available in Microsoft Fabric.

| Capability |Description |
|------------|------------|
| [Conditional access](security-conditional-access.md)  | Secure your apps by using Microsoft Entra ID |
| [Lockbox](security-lockbox.md)  | Control how Microsoft engineers access your data                   |
| [OneLake security](../onelake/onelake-security.md) | Learn how to secure your data in OneLake. |
| [Resiliency](az-resiliency.md) | Reliability and regional resiliency with Azure availability zones   |
| [Service tags](security-service-tags.md) | Enable an Azure SQL Managed Instance (MI) to allow incoming connections from Microsoft Fabric |

## Related content

* [Security fundamentals](../security/security-fundamentals.md)

* [Admin overview](../admin/admin-overview.md)

* [Governance and compliance overview](../governance/governance-compliance-overview.md)

* [Microsoft Fabric licenses](../enterprise/licenses.md)
