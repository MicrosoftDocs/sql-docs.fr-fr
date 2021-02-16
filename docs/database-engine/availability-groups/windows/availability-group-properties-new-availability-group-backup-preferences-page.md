---
title: 'Propriétés d’un groupe de disponibilité : Page Préférences de sauvegarde'
description: Description des différentes propriétés disponibles dans la page « Préférence de sauvegarde » de l’Assistant « Nouveau groupe de disponibilité » de SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.availabilitygroupproperties.backuppreferences.f1
helpviewer_keywords:
- read-only routing
ms.assetid: 65fff22d-5963-4a8c-8b31-fe9ab247a03e
author: cawrites
ms.author: chadam
ms.openlocfilehash: c24ac8fac48de3a01d86371f56fcb86796b317db
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340844"
---
# <a name="availability-group-properties-new-availability-group-backup-preferences-page"></a>Propriétés d’un groupe de disponibilité : Nouveau groupe de disponibilité (page Préférences de sauvegarde)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Utilisez cette boîte de dialogue pour afficher et modifier les préférences de sauvegarde du groupe de disponibilité sélectionné.  
  
 **Pour afficher les propriétés d'un groupe de disponibilité**  
  
-   [Afficher les propriétés d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)  
  
-   [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](~/database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
## <a name="where-should-backups-occur"></a>Où les sauvegardes doivent-elles être effectuées ?  
 **Préférer secondaire**  
 Spécifie que les sauvegardes doivent se produire sur un réplica secondaire sauf lorsque le réplica principal est le seul réplica en ligne. Dans ce cas, la sauvegarde doit se produire sur le réplica principal. Il s'agit de l'option par défaut.  
  
 **Secondaire uniquement**  
 Spécifie que les sauvegardes ne doivent jamais être effectuées sur le réplica principal. Si le réplica principal est le seul réplica en ligne, la sauvegarde ne doit pas avoir lieu.  
  
 **Primaire**  
 Spécifie que les sauvegardes doivent toujours avoir lieu sur le réplica principal. Cette option est utile si vous avez besoin de fonctionnalités de sauvegarde, telles que la création de sauvegardes différentielles, qui ne sont pas prises en charge lorsque la sauvegarde est exécutée sur un réplica secondaire.  
  
 **Tout réplica**  
 Spécifie que vous préférez que les travaux de sauvegarde ignorent le rôle des réplicas de disponibilité lorsque vous choisissez le réplica pour effectuer les sauvegardes. Notez que les travaux de sauvegarde peuvent évaluer d'autres facteurs tels que la priorité de sauvegarde de chaque réplica de disponibilité en association avec son état opérationnel et son état connecté.  
  
> [!IMPORTANT]  
>  Il n'y a aucune contrainte du paramètre de préférence de sauvegarde. La traduction de cette préférence dépend de la logique, le cas échéant, que vous avez écrite dans les travaux de sauvegarde pour les bases de données dans un groupe de disponibilité donné. Pour plus d’informations, consultez [Secondaires actifs : Sauvegarde sur des réplicas secondaires &#40;groupes de disponibilité Always On&#41;](active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).  
  
## <a name="replica-backup-priorities"></a>Priorités de sauvegarde de réplica  
 Cette grille affiche la priorité de sauvegarde actuelle de chaque instance de serveur qui héberge un réplica pour le groupe de disponibilité. Utilisez cette grille pour modifier la priorité de sauvegarde d'un ou plusieurs réplicas de disponibilité.  
  
 **Instance de serveur**  
 Nom de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui héberge le réplica de disponibilité.  
  
 **Priorité de sauvegarde (Minimale=1, Maximale=100)**  
 Spécifie la priorité d'exécution des sauvegardes sur ce réplica par rapport aux autres réplicas dans le même groupe de disponibilité. La valeur est un entier compris entre 0 et 100. 1 indique la priorité la plus faible, 100 la priorité la plus élevée. Si **Priorité de sauvegarde** = 1, le réplica de disponibilité est choisi pour l’exécution des sauvegardes uniquement si aucun réplica de disponibilité ayant une priorité plus élevée n’est actuellement disponible.  
  
 **Exclure le réplica**  
 Sélectionnez cette option si vous ne souhaitez jamais que ce réplica de disponibilité soit choisi pour effectuer des sauvegardes. Cela est utile, par exemple, pour un réplica de disponibilité distant sur lequel vous ne souhaitez jamais basculer de sauvegardes.  
  
## <a name="see-also"></a>Voir aussi  
 [Secondaires actifs : sauvegarde sur les réplicas secondaires &#40;groupes de disponibilité AlwaysOn&#41;](active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)  
  
  

