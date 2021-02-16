---
title: 'Groupes de disponibilité Always On : interopérabilité'
description: Décrit les différentes fonctionnalités qui peuvent et ne peuvent pas fonctionner en même temps dans un groupe de disponibilité Always On.
ms.custom: seodec18
ms.date: 04/20/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
helpviewer_keywords:
- Availability Groups [SQL Server], about
- Availability Groups [SQL Server], interoperability
ms.assetid: daf87f90-2623-42ca-912c-b8f07d210510
author: cawrites
ms.author: chadam
ms.openlocfilehash: fea5426d48b9808612062fccae4cc7c1228a2e1d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343946"
---
# <a name="always-on-availability-groups-interoperability-sql-server"></a>Groupes de disponibilité Always On : interopérabilité (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Cette rubrique documente l’interopérabilité des [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] avec d’autres fonctionnalités de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

## <a name="features-that-interoperate-with-always-on-availability-groups"></a><a name="Interop"></a> Fonctionnalités qui interopèrent avec les groupes de disponibilité Always On

Le tableau suivant liste les fonctionnalités de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui interopèrent avec les [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Un lien dans la colonne **Informations supplémentaires** indique que des observations relatives à l'interopérabilité existent pour une fonctionnalité.

|Fonctionnalité|Informations complémentaires|
|:------|:---------------|
|Capture des données modifiées|[Réplication, suivi des modifications, capture de données modifiées et groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)|
|Suivi des modifications|[Réplication, suivi des modifications, capture de données modifiées et groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)|
|Bases de données autonomes|[Bases de données autonomes avec les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/contained-databases-with-always-on-availability-groups-sql-server.md)|
|Chiffrement de base de données|[Bases de données chiffrées avec groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/encrypted-databases-with-always-on-availability-groups-sql-server.md)|
|Instantanés de base de données|[Instantanés de base de données avec des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/database-snapshots-with-always-on-availability-groups-sql-server.md)|
|FILESTREAM et FileTable|[FILESTREAM et FileTable avec groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/filestream-and-filetable-with-always-on-availability-groups-sql-server.md)|
|Recherche en texte intégral|Remarque : les index de recherche en texte intégral sont synchronisés avec les bases de données secondaires Always On.|
|Copie des journaux de transaction|[Conditions préalables requises pour la migration de la copie des journaux de transaction vers les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md)|
|Magasin d'objets blob distants (RBS)|[Magasin d’objets blob distants&#40;RBS&#41; et groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remote-blob-store-rbs-and-always-on-availability-groups-sql-server.md)|
|Réplication|[Configurer la réplication pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)<br /><br /> [Gestion d’une base de données de publication Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/maintaining-an-always-on-publication-database-sql-server.md)<br /><br /> [Réplication, suivi des modifications, capture de données modifiées et groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)<br /><br /> [Abonnés de réplication et groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)|
|Analysis Services|[Analysis Services avec les groupes de disponibilité Always On](../../../database-engine/availability-groups/windows/analysis-services-with-always-on-availability-groups.md)|
|Reporting Services|Utilisez des réplicas secondaires en lecture seule comme source de données de création de rapports et réduisez la charge sur votre réplica principal en lecture-écriture.<br /><br /> [Reporting Services avec les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/reporting-services-with-always-on-availability-groups-sql-server.md)|
|Service Broker|[Service Broker avec les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/service-broker-with-always-on-availability-groups-sql-server.md)|
|SQL Server Agent|&nbsp;|

## <a name="features-that-interoperate-with-always-on-availability-groups-with-restrictions"></a><a name="restrictions"></a> Fonctionnalités qui interopèrent avec les groupes de disponibilité Always On avec des restrictions

Les fonctionnalités suivantes interopèrent avec les [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] avec des restrictions spécifiques. Consultez les rubriques associées pour plus d’informations.

- Transactions entre bases de données/transactions distribuées ([!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] et Windows Server 2016). Pour plus d’informations, consultez [Transactions entre bases de données et transactions distribuées pour des groupes de disponibilité Always On et la mise en miroir de bases de données &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).
- Le [collecteur de données système des statistiques de requête](../../../relational-databases/data-collection/system-data-collection-set-reports.md#Query) ne peut pas s’exécuter de manière fiable dans un environnement avec des secondaires non accessibles en lecture. Pour utiliser le collecteur de données système des statistiques de requête, paramétrez tous les réplicas de groupe de disponibilité secondaires afin d’autoriser l’[accès en lecture](configure-read-only-access-on-an-availability-replica-sql-server.md). 

## <a name="features-that-do-not-interoperate-with-always-on-availability-groups"></a><a name="NoInterop"></a> Fonctionnalités qui n’interopèrent pas avec les groupes de disponibilité Always On

[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] n'interagit pas avec les fonctionnalités suivantes :

- Mise en miroir de bases de données. Pour plus d’informations, consultez [Transactions entre bases de données et transactions distribuées pour des groupes de disponibilité Always On et la mise en miroir de bases de données &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).

## <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé

- **Blogs :**

  [Guide de migration : Migration vers le clustering de basculement SQL Server 2012 et les groupes de disponibilité à partir des déploiements de mise en miroir et de clustering antérieurs](/archive/blogs/sqlalwayson/now-available-migration-guide-migrating-to-sql-server-2012-failover-clustering-and-availability-groups-from-prior-clustering-and-mirroring-deployments)
  [Blogs de l’équipe de SQL Server Always On : Blog officiel de l’équipe de SQL Server Always On](/archive/blogs/sqlalwayson/)
  [Blogs des ingénieurs du Service clientèle et du Support technique de SQL Server](/archive/blogs/psssql/)

- **Livres blancs :**

  [Guide de migration : Migration vers les groupes de disponibilité Always On à partir des déploiements antérieurs combinant la mise en miroir de bases de données et la copie des journaux de transaction](/previous-versions/sql/sql-server-2012/jj635217(v=msdn.10))
  [Microsoft SQL Server Always On Solutions Guide for High Availability and Disaster Recovery (Guide de solutions Microsoft SQL Server Always On pour la haute disponibilité et la reprise d’activité)](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))
  [Livres blancs de Microsoft pour SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)
  [Livres blancs de l’équipe de consultants clients de SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)

## <a name="see-also"></a>Voir aussi

[Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)
[Prérequis, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)