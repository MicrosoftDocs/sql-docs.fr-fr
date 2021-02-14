---
description: Surveillance et résolution des problèmes de migration de données (Stretch Database)
title: Surveiller et résoudre les problèmes liés à la migration des données
ms.date: 06/14/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, monitoring
- monitoring Stretch Database
ms.assetid: 06950858-8c02-4ec6-9c59-42b787316a2d
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: f4d46ccfca9642a1be2f9e60d7f3382b4c49e90d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079900"
---
# <a name="monitor-and-troubleshoot-data-migration-stretch-database"></a>Surveillance et résolution des problèmes de migration de données (Stretch Database)
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Pour surveiller la migration des données dans Stretch Database Monitor, sélectionnez **Tâches | Stretch | Monitor** pour à une base de données dans SQL Server Management Studio.  
  
## <a name="check-the-status-of-data-migration-in-the-stretch-database-monitor"></a>Vérifier l’état de la migration des données dans Stretch Database Monitor  
 Sélectionnez **Tâches | Stretch | Monitor** pour une base de données dans SQL Server Management Studio pour ouvrir Stretch Database Monitor et surveiller la migration des données.  
  
-   La partie supérieure de l’écran contient des informations générales sur la base de données SQL Server Stretch ainsi que sur la base de données Azure distante.  
  
-   La partie inférieure de l’écran affiche l’état de la migration des données pour chaque table Stretch de la base de données.  
  
 ![Moniteur Stretch Database](../../sql-server/stretch-database/media/stretch-monitor.PNG "Moniteur Stretch Database")  
  
##  <a name="check-the-status-of-data-migration-in-a-dynamic-management-view"></a><a name="Migration"></a> Vérification de l’état de la migration des données dans une vue de gestion dynamique  
 Ouvrez la vue de gestion dynamique **sys.dm_db_rda_migration_status** pour afficher le nombre de lots et de lignes de données migrés. Pour plus d’informations, consultez [sys.dm_db_rda_migration_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/stretch-database-sys-dm-db-rda-migration-status.md).  
  
##  <a name="troubleshoot-data-migration"></a><a name="Firewall"></a> Résolution des problèmes liés à la migration des données  
 **Les lignes de ma table Stretch ne sont pas migrées vers Azure. Quel est le problème ?**  
 Plusieurs problèmes peuvent affecter la migration. Vérifiez les points suivants :  
  
-   Vérifiez la connectivité réseau de l’ordinateur SQL Server.  
  
-   Vérifiez que le pare-feu Azure n’empêche pas votre serveur SQL Server de se connecter au point de terminaison distant.  
  
-   Vérifiez l’état du dernier lot dans la vue de gestion dynamique **sys.dm_db_rda_migration_status** . Si une erreur s’est produite, vérifiez les valeurs error_severity, error_state et error_number du lot concerné.  
  
    -   Pour plus d’informations sur la vue, consultez [sys.dm_db_rda_migration_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/stretch-database-sys-dm-db-rda-migration-status.md).  
  
    -   Pour plus d’informations sur le contenu d’un message d’erreur SQL Server, consultez [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md).  
  
 **Le pare-feu Azure bloque les connexions à partir de mon serveur local.**  
 Vous devrez peut-être ajouter une règle dans les paramètres de pare-feu Azure du serveur Azure pour permettre à SQL Server de communiquer avec le serveur Azure distant.  
  
## <a name="see-also"></a>Voir aussi  
 [Gérer et dépanner Stretch Database](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)  
  
  
