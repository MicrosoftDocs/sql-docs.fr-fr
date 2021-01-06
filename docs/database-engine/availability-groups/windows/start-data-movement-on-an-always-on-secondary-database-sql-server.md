---
title: Démarrer le déplacement des données sur une base de données secondaire
description: Découvrez comment démarrer le déplacement des données sur une base de données secondaire participant à un groupe de disponibilité Always On sur SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], databases
ms.assetid: 498eb3fb-6a43-434d-ad95-68a754232c45
author: cawrites
ms.author: chadam
ms.openlocfilehash: 262e797573690f8a648b552e782b0161f41ca7cc
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642990"
---
# <a name="start-data-movement-on-an-always-on-secondary-database-sql-server"></a>Démarrer un déplacement de données sur une base de données secondaire AlwaysOn (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique contient des informations sur le démarrage de la synchronisation des données après avoir ajouté une base de données à un groupe de disponibilité Always On. Pour chaque nouveau réplica principal, les bases de données secondaires doivent être préparées sur les instances de serveur qui hébergent les réplicas secondaires. Puis, chacune de ces bases de données secondaires doit être attachée manuellement au groupe de disponibilité.  
  
> [!NOTE]  
>  Si les chemins d’accès aux fichiers sont identiques sur chaque instance de serveur qui héberge un réplica de disponibilité pour un groupe de disponibilité, [l’Assistant Nouveau groupe de disponibilité](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md), [l’Assistant Ajouter un réplica au groupe de disponibilité](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)ou [l’Assistant Ajouter une base de données au groupe de disponibilité](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md) peut être en mesure de démarrer automatiquement la synchronisation des données.  
  
 Pour démarrer la synchronisation des données manuellement, vous devez vous connecter, tour à tour, à chaque instance de serveur qui héberge un réplica secondaire pour le groupe de disponibilité et procéder comme suit :  
  
1.  Restaurez les sauvegardes actuelles de chaque base de données primaire et de son journal des transactions (à l'aide de RESTORE WITH NORECOVERY). Vous pouvez utiliser l'une des autres approches suivantes :  
  
    -   Restaurez manuellement une sauvegarde récente de la base de données primaire à l'aide de RESTORE WITH NORECOVERY, puis restaurez chaque sauvegarde de journal suivante à l'aide de RESTORE WITH NORECOVERY. Effectuez cette séquence de restauration sur chaque instance de serveur qui héberge un réplica secondaire pour le groupe de disponibilité.  
  
         **Pour plus d'informations :**  
  
         [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
    -   Si vous ajoutez une ou plusieurs bases de données principales pour la copie des journaux de transaction à un groupe de disponibilité, vous pourrez peut-être effectuer une migration d’une ou de plusieurs des bases de données secondaires correspondantes depuis la copie des journaux de transaction vers des groupes de disponibilité Always On. La migration d'une base de données secondaire pour la copie des journaux de transaction nécessite l'utilisation du même nom de base de données que la base de données principale et sa présence sur une instance de serveur qui héberge un réplica secondaire pour le groupe de disponibilité. Par ailleurs, le groupe de disponibilité doit être configuré de sorte que le réplica principal corresponde à la préférence pour les sauvegardes et qu'il soit candidat à l'exécution de sauvegardes (autrement dit, que sa priorité de sauvegarde soit >0). Une fois que le travail de sauvegarde a été exécuté sur la base de données principale, vous devez le désactiver. De même, une fois que le travail de restauration a été exécuté sur une base de données secondaire donnée, vous devez le désactiver.  
  
        > [!NOTE]  
        >  Après avoir créé toutes les bases de données secondaires pour le groupe de disponibilité, si vous souhaitez effectuer des sauvegardes sur des réplicas secondaires, vous devez reconfigurer la préférence de sauvegarde automatisée du groupe de disponibilité.  
  
         **Pour plus d'informations :**  
  
         [Conditions préalables requises pour la migration de la copie des journaux de transaction vers les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md)  
  
         [Configurer la sauvegarde sur des réplicas de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
2.  Dès que possible, attachez chaque base de données secondaire récemment préparée au groupe de disponibilité.  
  
     **Pour plus d'informations :**  
  
     [Joindre une base de données secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
##  <a name="related-tasks"></a><a name="LaunchWiz"></a> Tâches associées  
  
-   [Utiliser la boîte de dialogue Nouveau groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Utiliser l’Assistant Ajouter un réplica au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Utiliser l’Assistant Ajouter une base de données au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
