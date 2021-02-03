---
title: Haute disponibilité - Bases de données OLTP en mémoire
description: Les bases de données SQL Server avec des tables à mémoire optimisée, avec ou sans procédures stockées compilées natives, sont entièrement prises en charge avec les groupes de disponibilité Always On.
ms.custom: seo-dt-2019
ms.date: 08/31/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 2113a916-3b1e-496c-8650-7f495e492510
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 96686e379f9d10058ebcedffd44405b6c2f8a18a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237574"
---
# <a name="high-availability-support-for-in-memory-oltp-databases"></a>Prise en charge de la haute disponibilité pour les bases de données OLTP en mémoire
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Les bases de données contenant des tables optimisées en mémoire, avec ou sans procédures stockées compilées natives, sont entièrement prises en charge avec les groupes de disponibilité Always On.  Il n’existe aucune différence dans la configuration et la prise en charge des bases de données contenant des objets [!INCLUDE[hek_2](../../includes/hek-2-md.md)] et celles qui n’en comportent pas.  

 Les modifications apportées aux tables optimisées en mémoire sur le réplica principal sont appliquées aux tables sur le réplica secondaire pendant la phase de restauration par progression. Cela permet un basculement rapide vers le réplica secondaire car les données sont déjà en mémoire. Les tables sont disponibles pour les requêtes de lecture sur les réplicas secondaires qui ont été configurés pour un accès en lecture.  

  
## <a name="always-on-availability-groups-and-in-memory-oltp-databases"></a>Groupes de disponibilité Always On et bases de données OLTP en mémoire  
 La configuration des bases de données avec des composants [!INCLUDE[hek_2](../../includes/hek-2-md.md)] offre les avantages suivants :  
  
-   **Expérience entièrement intégrée**   
    Vous pouvez configurer vos bases de données contenant des tables optimisées en mémoire à l’aide du même assistant et avec le même niveau de prise en charge pour les réplicas secondaires synchrones et asynchrones. En outre, le contrôle d’intégrité est fourni par le tableau de bord Always On que vous connaissez dans SQL Server Management Studio.  
  
-   **Temps de basculement comparable**   
    Les réplicas secondaires maintiennent l’état en mémoire des tables optimisées en mémoire durables. En cas de basculement automatique ou forcé, le temps de basculement vers le nouveau réplica principal est comparable aux tables sur disque car aucune récupération n'est nécessaire. Les tables mémoire optimisées créées en tant que SCHEMA_ONLY sont prises en charge dans cette configuration. Cependant, les modifications apportées à ces tables ne sont pas consignées : aucune donnée n’existera donc dans ces tables sur le réplica secondaire.  
  
-   **Secondaire accessible en lecture**   
    Vous pouvez accéder aux tables optimisées en mémoire sur le réplica secondaire et les interroger si le réplica secondaire a été configuré pour un accès en lecture. Dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], l’horodateur de lecture sur le réplica secondaire est étroitement synchronisé avec l’horodateur de lecture sur le réplica principal, ce qui signifie que les modifications apportées sur le réplica principal sont rapidement visibles sur le réplica secondaire. Ce comportement de synchronisation étroite est différent d’OLTP en mémoire [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] .  

### <a name="considerations"></a>Considérations

- SQL Server 2019 a introduit une restauration par progression parallèle pour les bases de données de groupe de disponibilité à mémoire optimisée. Dans SQL Server 2016 et 2017, les tables basées sur disque n’utilisent pas l’opération de restauration par progression parallèle si une base de données dans un groupe de disponibilité est également optimisée en mémoire. 
  
## <a name="failover-clustering-instance-fci-and-in-memory-oltp-databases"></a>Instance de clustering de basculement (FCI) et bases de données OLTP en mémoire  
 Pour bénéficier d’une haute disponibilité dans une configuration de stockage partagé, vous pouvez configurer une instance de cluster de basculement comportant des bases de données avec des tables mémoire optimisées. Tenez compte des facteurs suivants dans le cadre de la configuration d’une instance FCI :  
  
-   **Objectif de temps de récupération**   
    Le temps de basculement est susceptible d’être plus long, car les tables à mémoire optimisée doivent être chargées en mémoire avant que la base de données ne soit disponible.  
  
-   **Tables SCHEMA_ONLY**   
    Sachez que les tables SCHEMA_ONLY seront vides et ne comporteront aucune ligne après le basculement. C’est l’application qui conçoit et définit cela. Le comportement est identique quand vous redémarrez une base de données [!INCLUDE[hek_2](../../includes/hek-2-md.md)] avec une ou plusieurs tables SCHEMA_ONLY.  
  
## <a name="support-for-transaction-replication-in-in-memory-oltp"></a>Prise en charge de la réplication des transactions dans OLTP en mémoire  
 Les tables agissant comme des abonnés de réplication transactionnelle, à l'exclusion de la réplication transactionnelle d'égal à égal, peuvent être configurées en tant que tables mémoire optimisées. Les autres configurations de réplication ne sont pas compatibles avec les tables à mémoire optimisée.  Pour plus d’informations, consultez [Abonnés à la réplication de tables optimisées en mémoire](../../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Groupes de disponibilité Always On (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Secondaires actifs : réplicas secondaires lisibles (groupes de disponibilité AlwaysOn)](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [Abonnés à la réplication de tables optimisées en mémoire](../../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md)  
  
  
