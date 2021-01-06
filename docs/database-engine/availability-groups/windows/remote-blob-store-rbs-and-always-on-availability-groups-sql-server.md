---
title: Magasin d’objets blob distants (RBS) avec les groupes de disponibilité
description: 'Description de l’utilisation du magasin d’objets blob distants (RBS) avec des bases de données qui font partie d’un groupe de disponibilité Always On. '
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: 01a70258-d4fd-40bc-bc44-c490b5d6c420
author: cawrites
ms.author: chadam
ms.openlocfilehash: 61316ca08eac7d24b1be4ef50c913aeba3358775
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642508"
---
# <a name="use-remote-blob-store-rbs-with-always-on-availability-groups"></a>Utiliser le magasin d’objets blob distants (RBS) avec les groupes de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] peut fournir une solution haute disponibilité et de récupération d’urgence pour les objets blob du [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)][Magasin d’objets blob distants (RBS)](../../../relational-databases/blob/remote-blob-store-rbs-sql-server.md) . [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] protège les métadonnées et schémas du magasin d’objets blob distants stockés dans une base de données de disponibilité en les répliquant sur des réplicas secondaires. Il s'agit de la base de données de contenu SharePoint. De manière générale, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] stocke ces métadonnées RBS indépendamment de l'objet blob.  
  
 La protection des données BLOB de RBS dépend de l’emplacement du magasin d’objets blob, comme suit :  
  
|Emplacement du magasin d'objets BLOB|Les groupes de disponibilité peuvent-ils protéger ces données BLOB ?|  
|-------------------------|-----------------------------------------------------|  
|La même base de données qui contient les métadonnées du magasin d'objets blob distants (stockées à l'aide d'un fournisseur FILESTREAM distant de RBS)|Oui|  
|Une autre base de données dans la même instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (enregistrée à l'aide d'un fournisseur FILESTREAM distant de RBS)|Oui<br /><br /> Nous vous recommandons de placer cette base de données dans le même groupe de disponibilité que la base de données qui contient les métadonnées du magasin d'objets blob distants.|  
|Une autre base de données dans une autre instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (enregistrée à l'aide d'un fournisseur FILESTREAM distant de RBS)|Oui<br /><br /> Cette base de données doit être dans un groupe de disponibilité distinct.|  
|Un magasin d'objets BLOB tiers|Non<br /><br /> Pour protéger ces données BLOB, utilisez les mécanismes de haute disponibilité du fournisseur de magasin d'objets blob.|  
  
##  <a name="limitations"></a><a name="Limitations"></a> Limitations  
  
-   Les chargés de maintenance RBS doivent être ciblés sur le réplica principal.  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Utilisez un écouteur de groupe de disponibilité. Pour plus d’informations, consultez [Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
  
-   [Maintenance du magasin d’objets BLOB distants](https://msdn.microsoft.com/library/gg316773\(SQL.105\).aspx) (dans la documentation en ligne de [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] )  
  
-   [Running RBS Maintainer (Exécution du chargé de maintenance RBS)](/archive/blogs/sqlrbs/running-rbs-maintainer) (blog)  
  
-   [Configure Remote BLOB Storage (RBS) with the FILESTREAM provider (SharePoint 2010) (Configurer le stockage d’objets blob distants avec le fournisseur FILESTREAM)](/archive/blogs/mvpawardprogram/configure-remote-blob-storage-rbs-with-the-filestream-provider-sharepoint-2010) (blog)  
  
## <a name="see-also"></a>Voir aussi  
 [Connectivité client Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)   
 [Magasin d’objets blob distants &#40;RBS&#41; &#40;SQL Server&#41;](../../../relational-databases/blob/remote-blob-store-rbs-sql-server.md)  
  
