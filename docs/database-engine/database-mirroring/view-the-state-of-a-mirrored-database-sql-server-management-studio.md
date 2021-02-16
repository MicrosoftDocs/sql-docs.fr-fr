---
title: Voir l’état d’une base de données mise en miroir
description: Découvrez comment voir l’état d’une base de données configurée pour la mise en miroir de bases de données dans l’interface graphique utilisateur de SSMS (SQL Server Management Studio).
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- states [SQL Server], database mirroring
- database mirroring [SQL Server], states
ms.assetid: 544f4194-253e-4c57-96ca-31c16301434f
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 991a87b45632c49853d9ac4c97b29c743e18e781
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352162"
---
# <a name="view-the-state-of-a-mirrored-database-sql-server-management-studio"></a>Afficher l'état d'une base de données mise en miroir (SQL Server Management Studio)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Pendant une session de mise en miroir de bases de données, vous pouvez afficher l’état dans la page **Mise en miroir** de la boîte de dialogue **Propriétés de la base de données** .  
  
### <a name="to-view-the-status-of-a-database-mirroring-session"></a>Pour afficher l'état d'une session de mise en miroir de bases de données  
  
1.  Après vous être connecté à l'instance du serveur principal, dans l'Explorateur d'objets, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez **Bases de données** et sélectionnez la base de données à mettre en miroir.  
  
3.  Cliquez avec le bouton droit sur la base de données, sélectionnez **Tâches**, puis cliquez sur **Miroir**. La page **Mise en miroir** de la boîte de dialogue **Propriétés de la base de données** s'affiche.  
  
4.  Quand la mise en miroir a commencé, le volet **État** affiche l’état de la session de mise en miroir de bases de données comme si vous aviez sélectionné la page **Mise en miroir** ou cliqué sur le bouton **Actualiser** . Les états possibles sont :  
  
    |États|Explication|  
    |------------|-----------------|  
    |\<blank>|Aucune session de mise en miroir de base de données n’existe et aucune activité n’est à signaler dans la page **Mise en miroir** .|  
    |Suspendu|La base de données principale est en cours d'exécution, mais n'envoie aucun journal au serveur miroir. La copie miroir de la base de données n'est pas disponible.|  
    |Aucune connexion|L'instance du serveur principal ne peut pas se connecter à son partenaire ou à l'instance du serveur témoin (le cas échéant)|  
    |Synchronisation|Le contenu de la base de données en miroir est décalé par rapport à celui de la base de données principale. L'instance de serveur principal envoie des enregistrements de journal à l'instance de serveur miroir, laquelle applique les modifications à la base de données miroir pour la restaurer par progression.<br /><br /> Au début d'une session de mise en miroir de bases de données, les bases de données miroir et serveur sont en état de synchronisation.|  
    |Basculement|Sur l'instance du serveur principal, un basculement manuel (échange de rôles) a commencé, mais n'a pas encore été accepté par le miroir.|  
    |Synchronisé|La base de données miroir contient les mêmes données que la base de données principale. Les basculements manuel et automatique sont possibles *uniquement* à l’état synchronisé.|  
  
## <a name="see-also"></a>Voir aussi  
 [États de la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/mirroring-states-sql-server.md)  
  
  
