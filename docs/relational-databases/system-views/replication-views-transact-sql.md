---
title: Vues de réplication (Transact-SQL) | Microsoft Docs
description: Les vues de réplication contiennent des informations qui sont utilisées par la réplication dans SQL Server. Les vues facilitent l'accès aux données dans les tables système de réplication.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- distribution databases [SQL Server replication], system views
- metadata [SQL Server], views
- views [SQL Server], replication
- replication views [SQL Server]
- publications [SQL Server replication], system views
- articles [SQL Server replication], system views
- replication metadata [SQL Server]
- subscriptions [SQL Server replication], system views
- system views [SQL Server], replication
ms.assetid: 93e5056d-0d93-4a48-ba33-72762eb995d8
author: stevestein
ms.author: sstein
ms.openlocfilehash: 64fcd7c57c57dec9978695b52db1af2cc2c8ebba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160354"
---
# <a name="replication-views-transact-sql"></a>Vues de réplication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ces vues contiennent des informations qui sont utilisées par la réplication dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les vues permettent un accès plus facile aux données dans les [tables système de réplication](../../relational-databases/system-tables/replication-tables-transact-sql.md). Les vues sont créées dans une base de données utilisateur lorsque celle-ci est activée en tant que base de données de publication ou d'abonnement. Tous les objets de réplication sont supprimés de la base de données utilisateur lorsque celle-ci est supprimée d'une topologie de réplication. La méthode recommandée pour accéder aux métadonnées de réplication consiste à utiliser des [procédures stockées de réplication](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md).  
  
> [!IMPORTANT]  
>  Les vues système ne doivent pas être modifiées directement par l'utilisateur.  
  
## <a name="replication-views"></a>Vues de réplication  
 Les informations suivantes regroupent par base de données les vues système utilisées par la réplication.  
  
### <a name="replication-views-in-the-msdb-database"></a>Vues de réplication de la base de données msdb  

:::row:::
    :::column:::
        [MSdatatype_mappings &#40;Transact-SQL&#41;](../../relational-databases/system-views/msdatatype-mappings-transact-sql.md)
    :::column-end:::
    :::column:::
        [sysdatatypemappings &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysdatatypemappings-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="replication-views-in-the-distribution-database"></a>Vues de réplication de la base de données de distribution  

:::row:::
    :::column:::
        [IHextendedArticleView &#40;Transact-SQL&#41;](../../relational-databases/system-views/ihextendedarticleview-transact-sql.md)

        [IHextendedSubscriptionView &#40;Transact-SQL&#41;](../../relational-databases/system-views/ihextendedsubscriptionview-transact-sql.md)

        [IHsyscolumns &#40;Transact-SQL&#41;](../../relational-databases/system-views/ihsyscolumns-transact-sql.md)

        [MSdistribution_status &#40;Transact-SQL&#41;](../../relational-databases/system-views/msdistribution-status-transact-sql.md)

        [sysarticlecolumns &#40;vue système&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysarticlecolumns-system-view-transact-sql.md)
    :::column-end:::
    :::column:::
        [sysarticles &#40;vue système&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md)

        [sysextendedarticlesview &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysextendedarticlesview-transact-sql.md)

        [syspublications &#40;vue système&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-views/syspublications-system-view-transact-sql.md)

        [syssubscriptions &#40;vue système&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-views/syssubscriptions-system-view-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="replication-views-in-the-publication-database"></a>Vues de réplication de la base de données de publication  

:::row:::
    :::column:::
        [sysmergeextendedarticlesview &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysmergeextendedarticlesview-transact-sql.md)

        [systranschemas &#40;Transact-SQL&#41;](../../relational-databases/system-views/systranschemas-transact-sql.md)
    :::column-end:::
    :::column:::
        [sysmergepartitioninfoview &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysmergepartitioninfoview-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="replication-views-in-the-subscription-database"></a>Vues de réplication de la base de données d'abonnement  

:::row:::
    :::column:::
        [sysmergeextendedarticlesview &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysmergeextendedarticlesview-transact-sql.md)

        [sysmergepartitioninfoview &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysmergepartitioninfoview-transact-sql.md)
    :::column-end:::
    :::column:::
        [systranschemas &#40;Transact-SQL&#41;](../../relational-databases/system-views/systranschemas-transact-sql.md)
    :::column-end:::
:::row-end:::
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
