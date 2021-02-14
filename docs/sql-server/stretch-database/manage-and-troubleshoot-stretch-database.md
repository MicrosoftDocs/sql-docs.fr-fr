---
description: Gestion et dépannage de Stretch Database
title: Gérer et dépanner
ms.date: 06/27/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, managing
- Stretch Database, troubleshooting
- managing Stretch Database
- troubleshooting Stretch Database
ms.assetid: 6334db3e-9297-44df-8d53-211187a95520
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 346bc660bc1fa1fd9a6e75a91caa182ff25e6c53
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081024"
---
# <a name="manage-and-troubleshoot-stretch-database"></a>Gestion et dépannage de Stretch Database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Pour gérer Stretch Database et résoudre ses problèmes, utilisez les méthodes et outils décrits dans cet article.  
## <a name="manage-local-data"></a>Gérer des données locales  
  
###  <a name="get-info-about-local-databases-and-tables-enabled-for-stretch-database"></a><a name="LocalInfo"></a> Obtenir des informations sur les bases de données et tables locales compatibles avec Stretch Database  
 Ouvrez les affichages catalogue **sys.databases** et **sys.tables** pour afficher des informations sur les tables et bases de données SQL Server compatibles avec Stretch Database. Pour plus d’informations, consultez [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) et [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md).  
 
 Pour afficher la quantité d’espace qu’utilise une table compatible Stretch dans SQL Server, exécutez l’instruction suivante.
 
 ```sql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'LOCAL_ONLY';
GO
 ```
   
## <a name="manage-data-migration"></a>Gérer la migration des données  
  
### <a name="check-the-filter-function-applied-to-a-table"></a>Vérifier la fonction de filtre appliquée à une table  
 Ouvrez l’affichage catalogue **sys.remote_data_archive_tables** et vérifiez la valeur de la colonne **filter_predicate** pour identifier la fonction que Stretch Database utilise pour sélectionner les lignes à faire migrer. Si la valeur est null, la table entière est éligible à la migration. Pour plus d’informations, consultez [sys.remote_data_archive_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-tables.md) et [Sélectionner les lignes à migrer à l’aide d’une fonction de filtre](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).  
  
###  <a name="check-the-status-of-data-migration"></a><a name="Migration"></a> Vérifier l’état de la migration des données  
 Sélectionnez **Tâches | Stretch | Monitor** pour une base de données dans SQL Server Management Studio afin de surveiller la migration des données dans Stretch Database Monitor. Pour plus d’informations, consultez [Surveiller et résoudre les problèmes de migration de données &#40;Stretch Database&#41;](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md).  
  
 Ou bien, ouvrez la vue de gestion dynamique **sys.dm_db_rda_migration_status** pour afficher le nombre de lots et de lignes de données qui ont migré.  
  
###  <a name="troubleshoot-data-migration"></a><a name="Firewall"></a> Résolution des problèmes liés à la migration des données  
 Pour obtenir des suggestions de résolution des problèmes, consultez [Surveiller et résoudre les problèmes de migration de données &#40;Stretch Database&#41;](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md).  
  
## <a name="manage-remote-data"></a>Gérer les données distantes  
  
###  <a name="get-info-about-remote-databases-and-tables-used-by-stretch-database"></a><a name="RemoteInfo"></a> Obtenir des informations sur les bases de données et tables distantes utilisées par Stretch Database  
 Ouvrez les affichages catalogue **sys.remote_data_archive_databases** et **sys.remote_data_archive_tables** pour afficher des informations sur les bases de données et tables distantes dans lesquelles sont stockées les données qui ont migré. Pour plus d’informations, consultez [sys.remote_data_archive_databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-databases.md) et [sys.remote_data_archive_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/stretch-database-catalog-views-sys-remote-data-archive-tables.md).  
 
Pour afficher la quantité d’espace qu’utilise une table compatible Stretch dans Azure, exécutez l’instruction suivante.
 
 ```sql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'REMOTE_ONLY';
GO
 ```

### <a name="delete-migrated-data"></a>Supprimer des données qui ont migré  
Si vous souhaitez supprimer des données qui ont déjà migré vers Azure, suivez les étapes décrites dans [sys.sp_rda_reconcile_batch](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-batch-transact-sql.md).  
  
## <a name="manage-table-schema"></a>Gérer le schéma de table

### <a name="dont-change-the-schema-of-the-remote-table"></a>Ne modifiez pas le schéma de la table distante  
 Ne modifiez pas le schéma d’une table Azure distante associée à une table SQL Server configurée pour Stretch Database. Surtout, ne modifiez ni le nom ni le type de données d’une colonne. La fonctionnalité Stretch Database émet plusieurs hypothèses sur le schéma de la table distante par rapport au schéma de la table SQL Server. Si vous modifiez le schéma, Stretch Database cesse de fonctionner pour la table modifiée.  

### <a name="reconcile-table-columns"></a>Rapprocher des colonnes de table  
Si vous avez accidentellement supprimé des colonnes de la table distante, exécutez **sp_rda_reconcile_columns** pour ajouter à la table distante des colonnes qui existent dans la table SQL Server compatible Stretch mais pas dans la table distante. Pour plus d’informations, consultez [sys.sp_rda_reconcile_columns](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-columns-transact-sql.md).  
  
  > [!IMPORTANT]
  > Quand **sp_rda_reconcile_columns** recrée des colonnes que vous avez supprimées par inadvertance de la table distante, les données qui se trouvaient précédemment dans les colonnes supprimées ne sont pas restaurées.
  
**sp_rda_reconcile_columns** ne supprime pas de la table distante les colonnes qui existent dans cette table distante, mais pas dans la table SQL Server compatible Stretch. Si des colonnes comprises dans la table Azure distante n’existent plus dans la table SQL Server compatible Stretch, ces colonnes supplémentaires n’empêchent pas Stretch Database de fonctionner normalement. Vous pouvez éventuellement supprimer manuellement les colonnes supplémentaires.  
 
## <a name="manage-performance-and-costs"></a>Gérer les coûts et performances  
  
### <a name="troubleshoot-query-performance"></a>Dépannage de la performance des requêtes  
  Attendez-vous à ce que les requêtes incluant des tables Stretch s’exécutent plus lentement que celle incluant des tables non compatibles avec Stretch. Si les performances des requêtes se dégradent considérablement, passez en revue les problèmes suivants.  
  
-   Votre serveur Azure se trouve-t-il dans une autre région géographique que votre serveur SQL Server ? Configurez votre serveur Azure dans la même région géographique que votre serveur SQL Server pour réduire la latence du réseau.  
  
-   Il se peut que votre réseau connaisse des problèmes de performance. Pour plus d’informations sur les pannes ou problèmes récents, contactez l’administrateur de votre réseau.  
  
### <a name="increase-azure-performance-level-for-resource-intensive-operations-such-as-indexing"></a>Augmenter le niveau de performances d’Azure pour les opérations gourmandes en ressources telles que l’indexation  
 Quand vous générez, générez à nouveau ou réorganisez un index sur une table volumineuse configurée pour Stretch Database et que vous vous attendez à une interrogation intensive des données qui ont migré dans Azure au même moment, envisagez d’augmenter le niveau de performance de la base de données Azure distante correspondante pendant la durée de l’opération. Pour plus d’informations sur les niveaux de performance et la tarification, consultez la page [Tarification de SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
  
### <a name="you-cant-pause-the-sql-server-stretch-database-service-on-azure"></a>Vous ne pouvez pas suspendre le service SQL Server Stretch Database sur Azure  
 Assurez-vous de sélectionner le niveau de performance et de tarification approprié. Si vous augmentez le niveau de performance temporairement pour une opération gourmande en ressources, restaurez le niveau précédent à l’issue de l’opération. Pour plus d’informations sur les niveaux de performance et la tarification, consultez la page [Tarification de SQL Server Stretch Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  
   
 ## <a name="change-the-scope-of-queries"></a>Modifier l’étendue des requêtes  
 Les requêtes sur les tables Stretch retournent des données à la fois locales et distantes par défaut. Vous pouvez modifier l’étendue des requêtes pour toutes les requêtes effectuées par tous les utilisateurs ou uniquement pour une seule requête effectuée par un administrateur.  
   
 ### <a name="change-the-scope-of-queries-for-all-queries-by-all-users"></a>Modifier l’étendue des requêtes pour toutes les requêtes effectuées par tous les utilisateurs  
 Pour modifier l’étendue de toutes les requêtes effectuées par tous les utilisateurs, exécutez la procédure stockée **sys.sp_rda_set_query_mode**. Vous pouvez réduire l’étendue pour interroger uniquement les données locales, désactiver toutes les requêtes ou restaurer le paramètre par défaut. Pour plus d’informations, consultez [sys.sp_rda_set_query_mode](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md).  
   
 ### <a name="change-the-scope-of-queries-for-a-single-query-by-an-administrator"></a><a name="queryHints"></a>Modifier l’étendue des requêtes pour une seule requête effectuée par un administrateur  
 Pour modifier l’étendue d’une seule requête effectuée par un membre du rôle db_owner, ajoutez l’indicateur de requête **WITH ( REMOTE_DATA_ARCHIVE_OVERRIDE = *valeur* )** à l’instruction SELECT. L’indicateur de requête REMOTE_DATA_ARCHIVE_OVERRIDE peut avoir les valeurs suivantes.  
 -   **LOCAL_ONLY**. Interroge uniquement les données locales.  
   
 -   **REMOTE_ONLY**. Interroge uniquement les données distantes.  
   
 -   **STAGE_ONLY**. Interroge uniquement les données de la table où Stretch Database effectue une copie intermédiaire des lignes éligibles à la migration et conserve les lignes qui ont migré pendant la période spécifiée après la migration. Cet indicateur de requête est le seul moyen d’interroger la table de mise en lots.  
  
Par exemple, la requête suivante retourne uniquement des résultats locaux.  
  
 ```sql  
USE <Stretch-enabled database name>;
GO
SELECT * FROM <Stretch_enabled table name> WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = LOCAL_ONLY) WHERE ... ;
GO
```  
   
 ## <a name="make-administrative-updates-and-deletes"></a><a name="adminHints"></a>Effectuer des suppressions et mises à jour administratives  
 Par défaut, vous ne pouvez pas mettre à jour ou supprimer des lignes éligibles à la migration, ou des lignes qui ont déjà migré, dans une table compatible Stretch. Quand vous devez corriger un problème, un membre du rôle db_owner peut exécuter une opération UPDATE ou DELETE en ajoutant l’indicateur de requête **WITH ( REMOTE_DATA_ARCHIVE_OVERRIDE = *valeur* )** à l’instruction. L’indicateur de requête REMOTE_DATA_ARCHIVE_OVERRIDE peut avoir les valeurs suivantes.  
 -   **LOCAL_ONLY**. Met à jour ou supprime des données locales uniquement.  
   
 -   **REMOTE_ONLY**. Met à jour ou supprime des données distantes uniquement.  
   
 -   **STAGE_ONLY**. Met à jour ou supprime uniquement les données de la table où Stretch Database effectue une copie intermédiaire des lignes éligibles à la migration et conserve les lignes qui ont migré pendant la période spécifiée après la migration.  
  
## <a name="see-also"></a>Voir aussi  
 [Surveiller et résoudre les problèmes de migration de données &#40;Stretch Database&#41;](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md)   
[Sauvegarder des bases de données Stretch (Stretch Database)](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)  
[Restaurer des bases de données Stretch (Stretch Database)](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)  
  
  
