---
title: SQL Server, réplica de disponibilité | Microsoft Docs
description: Découvrez l’objet de performance SQLServer:Availability Replica, qui contient des compteurs de performances sur les réplicas de disponibilité des groupes de disponibilité Always On.
ms.custom: ''
ms.date: 08/25/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- performance counters [SQL Server], AlwaysOn Availability Groups
- SQLServer:Availability Replica
- Availability Groups [SQL Server], performance counters
ms.assetid: e402f996-c1fb-484a-b804-45c49972f2e0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 39df44f69e8a4cd05b5cefda27aefcd20d2100e1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338148"
---
# <a name="sql-server-availability-replica"></a>SQL Server, réplica de disponibilité

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  L’objet de performance **SQLServer:Availability Replica** contient des compteurs de performances qui recueillent des informations sur les réplicas de disponibilité des groupes de disponibilité Always On dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Tous les compteurs de performance du réplica de disponibilité s'appliquent au réplica principal et aux réplicas secondaires, et les compteurs d'envoi/réception reflètent le réplica local. Généralement, le réplica principal envoie la plupart des données, et les réplicas secondaires les reçoivent. Toutefois, les réplicas secondaires envoient des accusés de réception et d'autres informations d'arrière-plan sur le trafic aux réplicas principaux. Notez que sur un réplica de disponibilité donné, certains compteurs affichent la valeur zéro, suivant le rôle actuel, principal ou secondaire, du réplica local.  
  
|Nom de compteur|Description|  
|------------------|-----------------|  
|**Octets reçus du réplica par seconde**|**SQL Server 2012 et 2014 :** Nombre réel d’octets (compressés) reçus du réplica de disponibilité (synchrone ou asynchrone) par seconde. Les mises à jour d'état et requête ping génèrent un trafic sur le réseau, même sur des bases de données sur lesquelles n'a lieu aucune mise à jour utilisateur. <BR/> <BR/> **SQL Server 2016 et versions ultérieures :** Nombre réel d’octets (compressés si asynchrone, non compressés si synchrone) reçus du réplica de disponibilité par seconde.|  
|**Octets envoyés au réplica par seconde**|**SQL Server 2012 et 2014 :** Nombre réel d’octets (compressés) envoyés sur le réseau au réplica de disponibilité distant (synchrone ou asynchrone) par seconde. La compression est activée par défaut pour les réplicas synchrones et asynchrones. <BR/> <BR/> **SQL Server 2016 et versions ultérieures :** Nombre d'octets envoyés au réplica de disponibilité distant par seconde. Avant la compression pour le réplica asynchrone. (Nombre réel d’octets pour le réplica synchrone qui ne présente pas de compression.)|  
|**Octets envoyés au transport par seconde**|**SQL Server 2012 et 2014 :** Nombre réel d’octets (compressés) envoyés sur le réseau au réplica de disponibilité distant (synchrone ou asynchrone) par seconde. La compression est activée par défaut pour les réplicas synchrones et asynchrones. <BR/> <BR/> **SQL Server 2016 et versions ultérieures :** Nombre d’octets envoyés au réplica de disponibilité distant par seconde, avant la compression pour le réplica asynchrone. (Nombre réel d’octets pour le réplica synchrone qui ne présente pas de compression.)|  
|**Temps de contrôle du flux (ms/s)**|Temps en millisecondes passé par les messages de flux du journal en attente de contrôle du flux d'envoi au cours de la dernière seconde.|  
|**Contrôle de flux par seconde**|Nombre de fois où le contrôle de flux a démarré au cours de la dernière seconde. **Temps de contrôle de flux (ms/s)** divisé par **Contrôle de flux/s** correspond à la durée moyenne par attente.|  
|**Réceptions du réplica par seconde**|Nombre de messages Always On reçus du réplica par seconde.|  
|**Messages renvoyés par seconde**|Nombre de messages Always On renvoyés au cours de la dernière seconde.|  
|**Envois au réplica par seconde**|Nombre de messages Always On envoyés à ce réplica de disponibilité par seconde.|  
|**Envois au transport par seconde**|Nombre réel de messages Always On envoyés par seconde sur le réseau au réplica de disponibilité distant. Sur le réplica primaire, il s'agit du nombre de messages envoyés au réplica secondaire. Sur le réplica secondaire, il s'agit du nombre de messages envoyés au réplica primaire.|  
  
## <a name="see-also"></a>Voir aussi 
 
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [SQL Server, réplica de base de données](../../relational-databases/performance-monitor/sql-server-database-replica.md)   
 [Groupes de disponibilité SQL Server Always On (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
