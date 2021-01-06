---
title: Prise en charge des pilotes et de la connectivité cliente pour les groupes de disponibilité
description: 'Cette rubrique décrit les considérations relatives à la connectivité cliente aux groupes de disponibilité Always On, notamment les prérequis, les restrictions et les recommandations de configurations et de paramètres des clients. '
ms.custom: seodec18
ms.date: 07/28/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- Availability Groups [SQL Server], prerequisites and restrictions
- Availability Groups [SQL Server], client connectivity
ms.assetid: b456448d-1757-48c8-8bbb-2d1c2d6d61e9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 20a1d0074330552e4a87defb65e9e6e1082df40c
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642235"
---
# <a name="driver-and-client-connectivity-support-for-availability-groups"></a>Prise en charge des pilotes et de la connectivité cliente pour les groupes de disponibilité
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique décrit les considérations relatives à la connectivité cliente aux groupes de disponibilité Always On, notamment les prérequis, les restrictions et les recommandations de configurations et de paramètres des clients.  
  
 
##  <a name="client-connectivity-support"></a><a name="ClientConnSupport"></a> Prise en charge de la connectivité client  
 La section ci-dessous fournit des informations sur la prise en charge [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] de la connectivité client.  
  
 **Prise en charge de pilote**  
  
 Le tableau suivant récapitule les pilotes pris en charge pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]:  
  
|Pilote|Basculement de sous-réseaux multiples|Intention de l'application|Routage en lecture seule|Basculement de sous-réseaux multiples : basculement plus rapide du point de terminaison d'un sous-réseau unique|Basculement de sous-réseaux multiples : résolution d'instance nommée pour des instances de cluster SQL|  
|------------|----------------------------|------------------------|------------------------|--------------------------------------------------------------------|-----------------------------------------------------------------------------------|  
|SQL Native Client 11.0 ODBC|Oui|Oui|Oui|Oui|Oui|  
|SQL Native Client 11.0 OLEDB|Non|Oui|Oui|Non|Non|  
|ADO.NET avec .NET Framework 4.0 et correctif logiciel de connectivité*|Oui|Oui|Oui|Oui|Oui|  
|ADO.NET avec .NET Framework 3.5 SP1 et correctif logiciel de connectivité**|Oui|Oui|Oui|Oui|Oui|  
|[Pilote Microsoft ODBC 13.1+ pour SQL Server](../../../connect/odbc/microsoft-odbc-driver-for-sql-server.md)|Oui|Oui|Oui|Oui|Oui|
|[Microsoft JDBC Driver 4.0+ pour SQL Server](../../../connect/jdbc/microsoft-jdbc-driver-for-sql-server.md)|Oui|Oui|Oui|Oui|Oui| 
|[Microsoft OLE DB Driver pour SQL Server](../../../connect/oledb/oledb-driver-for-sql-server.md)|Oui|Oui|Oui|Oui|Oui| 
  
 \* Télécharger le correctif logiciel de connectivité pour ADO .NET avec .NET Framework 4.0 : [https://support.microsoft.com/kb/2600211](https://support.microsoft.com/kb/2600211).  
  
 ** Télécharger le correctif logiciel de connectivité pour ADO.NET avec .NET Framework 3.5 SP1 : [https://support.microsoft.com/kb/2654347](https://support.microsoft.com/kb/2654347).  
 
 *Téléchargez le nouveau pilote Microsoft OLE DB Driver pour SQL Server : [https://aka.ms/downloadmsoledbsql](../../../connect/oledb/download-oledb-driver-for-sql-server.md).  

> [!IMPORTANT]  
>  Pour se connecter à un écouteur de groupe de disponibilité, un client doit utiliser une chaîne de connexion TCP.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Création et configuration des groupes de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)  
  
-   [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Clustering de basculement et groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)   
 [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [À propos de l’accès de la connexion client aux réplicas de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   
 [Microsoft SQL Server Always On Solutions Guide for High Availability and Disaster Recovery (Guide de solutions Microsoft SQL Server Always On pour la haute disponibilité et la récupération d’urgence)](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))   
 [Blog de l’équipe SQL Server Always On : Blog officiel de l’équipe SQL Server Always On](/archive/blogs/sqlalwayson/)   
 [Un long délai se produit quand vous vous reconnectez à une connexion IPSec à partir d’un ordinateur qui exécute Windows Server 2003, Windows Vista, Windows Server 2008, Windows 7 ou Windows Server 2008 R2](https://support.microsoft.com/kb/980915)   
 [Il faut environ 30 secondes au service de cluster pour basculer des adresses IP IPv6 dans Windows Server 2008 R2](https://support.microsoft.com/kb/2578113)   
 [Ralentissez l’opération de basculement s’il n’existe aucun routeur entre le cluster et un serveur d’applications](https://support.microsoft.com/kb/2582281)  
  
