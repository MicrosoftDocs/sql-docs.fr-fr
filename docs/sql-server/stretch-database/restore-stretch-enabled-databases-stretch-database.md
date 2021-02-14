---
description: Restaurer des bases de données Stretch (Stretch Database)
title: Restaurer des bases de données Stretch
ms.date: 07/06/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: cebc1f6d-d5ea-460d-ae60-d047d29c2723
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 18c4d602edea0f414863e3fc54bfa58d1e6ca2cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079790"
---
# <a name="restore-stretch-enabled-databases-stretch-database"></a>Restaurer des bases de données Stretch (Stretch Database)
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Restaurez une base de données sauvegardée quand cela est nécessaire pour effectuer une récupération après de nombreux types d’échecs, d’erreurs et d’incidents.
  
  Pour plus d’informations sur la sauvegarde, consultez [Sauvegarder des bases de données Stretch](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md).

> [!TIP]
> La sauvegarde ne constitue qu’une partie d’une solution complète de haute disponibilité et de continuité d’activité. Pour plus d’informations sur la haute disponibilité, consultez [Solutions haute disponibilité](../../database-engine/sql-server-business-continuity-dr.md).

## <a name="restore-your-sql-server-data"></a>Restaurer vos données SQL Server
Pour effectuer une récupération après une défaillance matérielle ou un endommagement, restaurez la base de données Stretch SQL Server à partir d’une sauvegarde. Vous pouvez continuer à utiliser les méthodes de restauration de SQL Server dont vous vous servez habituellement. Pour plus d’informations, consultez [Vue d’ensemble de la restauration et de la récupération](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).

Après avoir restauré la base de données SQL Server, vous devez exécuter la procédure stockée **sys.sp_rda_reauthorize_db** pour rétablir la connexion entre la base de données Stretch SQL Server et la base de données Azure distante. Pour plus d’informations, consultez [Restaurer la connexion entre la base de données SQL Server et la base de données Azure distante](#reconnect).

## <a name="restore-your-remote-azure-data"></a>Restaurer vos données Azure distantes

### <a name="recover-a-live-azure-database"></a>Récupérer une base de données Azure active
Le service SQL Server Stretch Database sur Azure effectue une capture instantanée de toutes les données actives au moins toutes les 8 heures à l’aide de captures instantanées Azure Storage. Ces captures instantanées sont conservées pendant 7 jours. Vous pouvez ainsi restaurer les données à un des 21 points (au moins) dans le temps au cours des 7 derniers jours jusqu’à l’heure à laquelle la dernière capture instantanée a été effectuée.

Pour restaurer une base de données Azure active à un point antérieur dans le temps à l’aide du portail Azure, procédez comme suit.

1. Connectez-vous au [portail Azure][].
2. Sur le côté gauche de l’écran, sélectionnez **PARCOURIR** , puis sélectionnez **Bases de données SQL**.
3. Accédez à votre base de données et sélectionnez-la.
4. En haut du panneau de la base de données, cliquez sur **Restaurer**.
5. Spécifiez un nouveau **Nom de base de données**, sélectionnez un **Point de restauration** , puis cliquez sur **Créer**.
6. Le processus de restauration de la base de données commence et peut être surveillé à l’aide de **NOTIFICATIONS**.

### <a name="recover-a-deleted-azure-database"></a>Récupérer une base de données Azure supprimée
Le service SQL Server Stretch Database sur Azure effectue une capture instantanée d’une base de données avant que celle-ci ne soit supprimée et la conserve pendant 7 jours. Ensuite, il ne conserve plus les captures instantanées de la base de données active. Ainsi, vous pouvez restaurer une base de données supprimée au point où elle a été supprimée.

Pour restaurer une base de données Azure supprimée au point où elle a été supprimée à l’aide du portail Azure, procédez comme suit.

1. Connectez-vous au [portail Azure][].
2. Sur le côté gauche de l’écran, sélectionnez **PARCOURIR** , puis sélectionnez **Serveurs SQL**.
3. Accédez à votre serveur et sélectionnez-le.
4. Faites défiler le panneau de votre serveur jusqu’à Opérations, puis cliquez sur la vignette **Bases de données supprimées** .
5. Sélectionnez la base de données supprimée à restaurer.
5. Spécifiez un nouveau **Nom de base de données** , puis cliquez sur **Créer**.
6. Le processus de restauration de la base de données commence et peut être surveillé à l’aide de **NOTIFICATIONS**.

## <a name="restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database"></a><a name="reconnect"></a>Restaurer la connexion entre la base de données SQL Server et la base de données Azure distante

1.  Si vous souhaitez vous connecter à une base de données Azure restaurée avec un nom différent ou dans une autre région, exécutez la procédure stockée [sys.sp_rda_deauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-deauthorize-db-transact-sql.md) pour vous déconnecter de la base de données Azure précédente.  
  
2.  Exécutez la procédure stockée [sys.sp_rda_reauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) pour reconnecter la base de données Stretch locale à la base de données Azure.  
  
    -   Fournissez les informations d’identification limitées à la base de données existante en tant que valeur sysname ou varchar (128). (N’utilisez pas varchar(max).) Vous pouvez rechercher le nom des informations d’identification dans la vue **sys.database_scoped_credentials**.  
  
    -   Indiquez si vous souhaitez effectuer une copie des données distantes et vous connecter à la copie (recommandée).  
  
    ```sql  
    USE <Stretch-enabled database name>;
    GO
    EXEC sp_rda_reauthorize_db
        @credential = N'<existing_database_scoped_credential_name>',
        @with_copy = 1 ;  
    GO  
    ```  
    
  ## <a name="see-also"></a>Voir aussi  
 [Sauvegarder des bases de données Stretch](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)  
 [Gérer Stretch Database et résoudre ses problèmes](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)   
 [sys.sp_rda_reauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) 
 [sys.sp_rda_deauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-deauthorize-db-transact-sql.md)  
 [Sauvegarder et restaurer des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)  
 
 [Azure portal]: https://portal.azure.com/
 
