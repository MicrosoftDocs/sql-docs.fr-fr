---
description: Désactiver Stretch Database et récupérer des données distantes
title: Désactiver Stretch Database et récupérer des données distantes
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, disabling
- disabling Stretch Database
ms.assetid: c1bbb24e-47e3-46aa-b786-fcadf9fb65ce
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 315cfed40b1ccdadf349d9b371d6baac5928d3af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081023"
---
# <a name="disable-stretch-database-and-bring-back-remote-data"></a>Désactiver Stretch Database et récupérer des données distantes
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Afin de désactiver Stretch Database pour une table, sélectionnez **Stretch** pour une table dans SQL Server Management Studio. Sélectionnez ensuite l’une des options suivantes.  
  
-   **Désactiver | Récupérer les données à partir d’Azure** Copiez les données distantes pour la table d'Azure vers SQL Server, puis désactivez Stretch Database pour la table. Cette opération entraîne des coûts de transfert de données et ne peut pas être annulée.  
  
-   **Désactiver | Laisser les données dans Azure** Désactivez Stretch Database pour la table.  Abandonnez les données distantes pour la table dans Azure.  
  
 Vous pouvez également utiliser Transact-SQL pour désactiver Stretch Database pour une table ou une base de données.  
  
 Une fois que vous avez désactivé Stretch Database pour une table, la migration des données s’arrête et les résultats de requête n’incluent plus les résultats de la table distante.  
  
 Si vous voulez simplement interrompre la migration des données, consultez [Suspension et reprise de la migration de données &#40;Stretch Database&#41;](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).  
  
> [!NOTE]
> La désactivation de Stretch Database pour une table ou une base de données ne supprime pas l'objet distant. Si vous souhaitez supprimer la table distante ou la base de données distante, vous devez la supprimer à l'aide du portail de gestion Azure. Les objets distants continuent d’entraîner des coûts Azure tant qu’ils n’ont pas été supprimés. Pour plus d'informations, consultez la rubrique [Tarification de SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
## <a name="disable-stretch-database-for-a-table"></a>Désactiver Stretch Database pour une table  
  
### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-table"></a>Utiliser SQL Server Management Studio afin de désactiver Stretch Database pour une table  
  
1.  Dans SQL Server Management Studio, dans l'Explorateur d'objets, sélectionnez la table pour laquelle vous souhaitez désactiver Stretch Database.  
  
2.  Cliquez avec le bouton droit, sélectionnez **Stretch**, puis choisissez l’une des options suivantes.  
  
    -   **Désactiver | Récupérer les données à partir d’Azure** Copiez les données distantes pour la table d'Azure vers SQL Server, puis désactivez Stretch Database pour la table. Cette commande ne peut pas être annulée.  
  
        > [!NOTE]
        > La copie des données distantes pour la table d'Azure vers SQL Server entraîne des coûts de transfert de données. Pour plus d'informations, consultez la rubrique [Détails de la tarification des transferts de données](https://azure.microsoft.com/pricing/details/data-transfers/).  
  
         Une fois que toutes les données distantes ont été copiées d'Azure vers SQL Server, Stretch est désactivée pour la table.  
  
    -   **Désactiver | Laisser les données dans Azure** Désactivez Stretch Database pour la table.  Abandonnez les données distantes pour la table dans Azure.  
  
    > [!NOTE]
    > La désactivation de Stretch Database pour une table ne supprime pas l'objet distant ou la table distante. Si vous souhaitez supprimer la table distante, vous devez la supprimer à l'aide du portail de gestion Azure. La table distante continue d’entraîner des coûts Azure tant qu’elle n’a pas été supprimée. Pour plus d'informations, consultez la rubrique [Tarification de SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
### <a name="use-transact-sql-to-disable-stretch-database-for-a-table"></a>Utilisation de Transact-SQL pour désactiver Stretch Database sur une table  
  
-   Pour désactiver Stretch pour une table et copier les données distantes pour la table d’Azure vers SQL Server, exécutez la commande suivante. Une fois que toutes les données distantes ont été copiées d’Azure vers SQL Server, Stretch est désactivé pour la table.

    Cette commande ne peut pas être annulée.  
  
    ```sql  
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ; 
    GO 
    ```  
  
    > [!NOTE]
    > La copie des données distantes pour la table d'Azure vers SQL Server entraîne des coûts de transfert de données. Pour plus d'informations, consultez la rubrique [Détails de la tarification des transferts de données](https://azure.microsoft.com/pricing/details/data-transfers/).    
  
-   Pour désactiver Stretch pour une table et abandonner les données distantes, exécutez la commande suivante.  
  
    ```sql  
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ; 
    GO
    ```  
  
> [!NOTE]
> La désactivation de Stretch Database pour une table ne supprime pas l'objet distant ou la table distante. Si vous souhaitez supprimer la table distante, vous devez la supprimer à l'aide du portail de gestion Azure. La table distante continue d’entraîner des coûts Azure tant qu’elle n’a pas été supprimée. Pour plus d'informations, consultez la rubrique [Tarification de SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
## <a name="disable-stretch-database-for-a-database"></a>Désactiver Stretch Database pour une base de données  
 Avant de pouvoir désactiver Stretch Database pour une base de données, vous devez désactiver Stretch Database sur les tables Stretch individuelles dans la base de données.  
  
### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-database"></a>Utiliser SQL Server Management Studio pour désactiver Stretch Database pour une base de données  
  
1.  Dans SQL Server Management Studio, dans l'Explorateur d'objets, sélectionnez la base de données pour laquelle vous souhaitez désactiver Stretch Database.  
  
2.  Cliquez avec le bouton droit, puis sélectionnez **Tâches**, **Stretch** et **Désactiver**.  
  
> [!NOTE]
> La désactivation de Stretch Database pour une base de données ne supprime pas la base de données distante. Si vous souhaitez supprimer la base de données distante, vous devez le faire à l’aide du portail de gestion Azure. La base de données distante continue d’entraîner des coûts Azure tant qu’elle n’a pas été supprimée. Pour plus d'informations, consultez la rubrique [Tarification de SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
### <a name="use-transact-sql-to-disable-stretch-database-for-a-database"></a>Utilisation de Transact-SQL pour désactiver Stretch Database sur une base de données  
 Exécutez la commande suivante.  
  
```sql  
ALTER DATABASE <Stretch-enabled database name>  
    SET REMOTE_DATA_ARCHIVE = OFF ;  
GO 
```  
  
> [!NOTE]
> La désactivation de Stretch Database pour une base de données ne supprime pas la base de données distante. Si vous souhaitez supprimer la base de données distante, vous devez le faire à l’aide du portail de gestion Azure. La base de données distante continue d’entraîner des coûts Azure tant qu’elle n’a pas été supprimée. Pour plus d'informations, consultez la rubrique [Tarification de SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
## <a name="see-also"></a>Voir aussi  
 [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [Suspension et reprise de la migration de données &#40;Stretch Database&#41;](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)  
  
  
