---
title: Microsoft Purview (formerly Azure Purview) accounts architecture and best practices
description: This article provides examples of accounts architectures and describes best practices for deploying Microsoft Purview (formerly Azure Purview).
author: zeinam
ms.author: zeinam
ms.service: purview
ms.subservice: purview-data-map
ms.topic: conceptual
ms.date: 10/12/2021
---

# Microsoft Purview accounts architectures and best practices  

To enable Microsoft Purview governance solutions, like Microsoft Purview Data Map and Data Catalog, in your environment, you'll deploy a Microsoft Purview (formerly Azure Purview) account in the Azure portal. You'll use this account to centrally manage data governance across your data estate, spanning both cloud and on-premises environments. To use Microsoft Purview as your centralized data governance solution, you may need to deploy one or more Microsoft Purview accounts inside your Azure subscription. We recommend keeping the number of Microsoft Purview instances as minimum, however, in some cases more Microsoft Purview instances are needed to fulfill business security and compliance requirements.

## Single Microsoft Purview account

Consider deploying minimum number of Microsoft Purview (formerly Azure Purview) accounts for the entire organization. This approach takes maximum advantage of the "network effects" where the value of the platform increases exponentially as a function of the data that resides inside the platform. 

Use [Microsoft Purview Data Map collections hierarchy](./concept-best-practices-collections.md) to lay out your organization's data management structure inside a single Microsoft Purview account. In this scenario, one account is deployed in an Azure subscription. Data sources from one or more Azure subscriptions can be registered and scanned inside the Microsoft Purview. You can also register and scan data sources from your on-premises or multi-cloud environments.

:::image type="content" source="media/concept-best-practices/accounts-single-account.png" alt-text="Screenshot that shows the single Microsoft Purview account."lightbox="media/concept-best-practices/accounts-single-account.png":::

## Multiple Microsoft Purview accounts

Some organizations may require setting up multiple Microsoft Purview accounts. Review the following scenarios as few examples when defining your Microsoft Purview accounts architecture:  

### Testing new features 

It's recommended to create a new account when testing scan configurations or classifications in isolated environments. For some scenarios, there's a "versioning" feature in some areas of the platform such as glossary, however, it would be easier to have a "disposable" instance of Microsoft Purview to freely test expected functionality and then plan to roll out the feature into the production instance.  

Additionally, consider using a test Microsoft Purview account when you can't perform a rollback. For example, currently you can't remove a glossary term attribute from a Microsoft Purview instance once it's added to your Microsoft Purview account. In this case, it's recommended using a test Microsoft Purview account first.
 
### Isolating Production and non-production environments 

Consider deploying separate instances of Microsoft Purview accounts for development, testing and production environments, specially when you have separate instances of data for each environment.  

In this scenario, production and non-production data sources can be registered and scanned inside their corresponding Microsoft Purview instances.

Optionally, you can register a data source in more than one Microsoft Purview instance, if needed.

:::image type="content" source="media/concept-best-practices/accounts-multiple-accounts.png" alt-text="Screenshot that shows multiple Microsoft Purview accounts based on environments."lightbox="media/concept-best-practices/accounts-multiple-accounts.png":::

### Fulfilling compliance requirements  

When you scan data sources in the Microsoft Purview Data Map, information related to your metadata is ingested and stored inside your data map in the Azure region where your Microsoft Purview account is deployed. Consider deploying separate instances of Microsoft Purview if you have specific regulatory and compliance requirements that include even having metadata in a specific geographical location.  

If your organization has data in multiple geographies and you must keep metadata in the same region as the actual data, you'll have to deploy multiple Microsoft Purview instances, one for each geography. In this case, data sources from each region should be registered and scanned in the Microsoft Purview account that corresponds to the data source region or geography.

:::image type="content" source="media/concept-best-practices/accounts-multiple-regions.png" alt-text="Screenshot that shows multiple Microsoft Purview accounts based on compliance requirements."lightbox="media/concept-best-practices/accounts-multiple-regions.png":::

### Having Data sources distributed across multiple tenants  

Currently, Microsoft Purview doesn't support multi-tenancy. If you have Azure data sources distributed across multiple Azure subscriptions under different Azure Active Directory tenants, it's recommended deploying separate Microsoft Purview accounts under each tenant. 

An exception applies to VM-based data sources and Power BI tenants.For more information about how to scan and register a cross tenant Power BI in a single Microsoft Purview account, see, [Register and scan a cross-tenant Power BI](./register-scan-power-bi-tenant.md). 

:::image type="content" source="media/concept-best-practices/accounts-multiple-tenants.png" alt-text="Screenshot that shows multiple Microsoft Purview accounts based on multi-tenancy requirements."lightbox="media/concept-best-practices/accounts-multiple-tenants.png"::: 

### Billing model 

Review [Microsoft Purview Pricing model](https://azure.microsoft.com/pricing/details/azure-purview) when defining budgeting model and designing an architecture for your organization. One billing is generated for a single Microsoft Purview account in the subscription where Microsoft Purview account is deployed. This model also applies to other Microsoft Purview costs such as scanning and classifying metadata inside Microsoft Purview Data Map.

Some organizations often have many business units (BUs) that operate separately, and, in some cases, they don't even share billing with each other. In those cases, the organization will end up creating a Microsoft Purview instance for each BU.

For more information about cloud computing cost model in chargeback and showback models, see: [What is cloud accounting?](/azure/cloud-adoption-framework/strategy/cloud-accounting).  

## Other considerations and recommendations 

- Keep the number of Microsoft Purview accounts low for simplified administrative overhead. If you plan building multiple Microsoft Purview accounts, you may require creating and managing extra scans, access control model, credentials, and runtimes across your Microsoft Purview accounts. Additionally, you may need to manage classifications and glossary terms for each Microsoft Purview account.

- Review your budgeting and financial requirements. If possible, use chargeback or showback model when using Azure services and divide the cost of Microsoft Purview across the organization to keep the number of Microsoft Purview accounts minimum. 

- Use [collections](concept-best-practices-collections.md) to define metadata access control inside Microsoft Purview Data Map for your organization's business users, data management and governance teams. For more information, see [Access control in Microsoft Purview](./catalog-permissions.md).

- Review [Microsoft Purview limits](./how-to-manage-quotas.md#microsoft-purview-limits) before deploying any new Microsoft Purview accounts. Currently, the default limit of Microsoft Purview accounts per region, per tenant (all subscriptions combined) is 3. You may need to contact Microsoft support to increase this limit in your subscription or tenant before deploying extra instances of Microsoft Purview.  

- Review [Microsoft Purview prerequisites](./create-catalog-portal.md#prerequisites) before deploying any new Microsoft Purview accounts in your environment.
  
## Next steps
-  [Create a Microsoft Purview account](./create-catalog-portal.md)
