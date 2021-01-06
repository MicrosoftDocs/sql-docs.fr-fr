---
title: Échec de l’opération d’ajout de fichier pour un groupe de disponibilité
description: Résolvez une opération d’ajout de fichier ayant échoué dans un groupe de disponibilité Always On. Les chemins d’accès aux fichiers peuvent différer entre les systèmes qui hébergent les réplicas principal et secondaire.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- secondary databases [SQL Server], Availability Groups
- Availability Groups [SQL Server], troubleshooting
ms.assetid: 31ceaebf-864b-4dd0-9112-0d047b0316ad
author: cawrites
ms.author: chadam
ms.openlocfilehash: b45d24bf9924c9e288402133a52f95b26576b99a
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642962"
---
# <a name="troubleshoot-a-failed-add-file-operation-always-on-availability-groups"></a>Résoudre une opération d’ajout de fichier ayant échoué (groupes de disponibilité Always On)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Pour certains déploiements de groupes de disponibilité Always On, les chemins des fichiers varient entre le système qui héberge le réplica principal et les systèmes qui hébergent un réplica secondaire. Si le chemin d'accès aux fichiers d'une opération d'ajout de fichier n'existe pas sur le réplica secondaire, l'opération d'ajout de fichier réussit sur la base de données principale. Cependant, l'opération d'ajout e fichier entraîne l'interruption de la base de données secondaire. Par voie de conséquence, le réplica secondaire passe à l'état NOT SYNCHRONIZING.  
  
> [!NOTE]  
>  Nous recommandons, si possible, que le chemin d'accès au fichier (y compris la lettre de lecteur) d'une base de données secondaire donnée soit identique au chemin d'accès de la base de données primaire correspondante.  
  
## <a name="problem-resolution"></a>Résolution des problèmes  
 Pour résoudre ce problème, le propriétaire de la base de données doit effectuer les opérations suivantes :  
  
1.  Supprimer la base de données secondaire du groupe de disponibilité. Pour plus d’informations, consultez [Supprimer une base de données secondaire d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md).  
  
2.  Sur la base de données secondaire existante, restaurer une sauvegarde complète du groupe de fichiers contenant le fichier ajouté à la base de données secondaire à l'aide de WITH NORECOVERY et de WITH MOVE (en spécifiant le chemin d'accès au fichier sur l'instance de serveur qui héberge le réplica secondaire). Pour plus d’informations, consultez [Restaurer une base de données à un nouvel emplacement &#40;SQL Server&#41;](../../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md).  
  
3.  Sauvegarder le journal des transactions qui contient l'opération d'ajout de fichier sur la base de données primaire, puis restaurer manuellement la sauvegarde du journal sur la base de données secondaire à l'aide de WITH NORECOVERY et de WITH MOVE.  
  
4.  Préparer la base de données secondaire pour rejoindre le groupe de disponibilité, en restaurant, à l'aide de WITH NORECOVERY, toutes les autres sauvegardes de journal en attente de la base de données primaire.  
  
5.  Réintégrer la base de données secondaire au groupe de disponibilité. Pour plus d’informations, consultez [Joindre une base de données secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)   
 [Dépanner des utilisateurs orphelins &#40;SQL Server&#41;](../../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md)   
 [Résoudre des problèmes de configuration des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)
