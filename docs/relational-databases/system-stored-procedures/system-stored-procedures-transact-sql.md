---
description: Procédures stockées système (Transact-SQL)
title: Procédures stockées système (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/21/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sql13.TSQLSysNoExpandPortal.f1
- sql13.TSQLSysNoExpandPortal.f1_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- stored procedures [SQL Server]
- APIs [SQL Server]
- stored procedures [SQL Server], categories
- system stored procedures [SQL Server], categories
- system stored procedures [SQL Server]
ms.assetid: a5c4d5b8-5a24-4a2d-99b4-d003b546ee3a
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 603a16d94c185783b3b90566d71b90e195949254
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203646"
---
# <a name="system-stored-procedures-transact-sql"></a>Procédures stockées système (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], de nombreuses tâches d'administration et d'information peuvent être effectuées à l'aide de procédures stockées système. Les procédures stockées système sont regroupées par catégories dans le tableau suivant.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Category|Description|  
|--------------|-----------------|  
|[Procédures stockées active Geo-Replication]()|Utilisé pour gérer pour gérer les configurations de Geo-Replication actives dans Azure SQL Database|  
|[Procédures stockées du catalogue](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)|Permettent d'implémenter les fonctions du dictionnaire de données ODBC et d'isoler les applications ODBC des modifications apportées aux tables système concernées.|  
|[Procédures stockées de capture des données modifiées](../../relational-databases/system-stored-procedures/change-data-capture-stored-procedures-transact-sql.md)|Permettent d'activer, de désactiver ou de créer des rapports sur des objets de capture des données modifiées.|  
|[Procédures stockées de curseur](../../relational-databases/system-stored-procedures/cursor-stored-procedures-transact-sql.md)|Permettent d'implémenter les fonctionnalités de variable de curseur.|  
|[Procédures stockées du collecteur de données](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)|Utilisé pour fonctionner avec le collecteur de données et les composants suivants : jeux d'éléments de collecte, éléments de collecte et types de collections.|  
|[Procédures stockées du moteur de base de données](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)|Servent à la maintenance générale du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].|  
|[Database Mail des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)|Permettent d'exécuter des opérations de messagerie électronique à partir d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[Procédures stockées de plan de maintenance de base de données](../../relational-databases/system-stored-procedures/database-maintenance-plan-stored-procedures-transact-sql.md)|Permettent de définir les tâches de maintenance principales nécessaires pour gérer les performances des bases de données.|  
|[Procédures stockées de requêtes distribuées](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)|Servent à implémenter et gérer les requêtes distribuées.|  
|[Procédures stockées FileStream et filetable &#40;Transact-SQL&#41;](./filestream-and-filetable-sp-filestream-force-garbage-collection.md)|Utilisé pour configurer et gérer les fonctionnalités FILESTREAM et FileTable.|  
|[Procédures stockées de règles de pare-feu &#40;Azure SQL Database&#41;](../../relational-databases/system-stored-procedures/firewall-rules-stored-procedures-azure-sql-database.md)|Utilisé pour configurer le pare-feu Azure SQL Database.|  
|[Procédures stockées de recherche en texte intégral](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)|Utilisées pour implémenter et effectuer les requêtes des index de texte intégral.|  
|[Procédures stockées étendues générales](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)|Permettent de fournir une interface depuis une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aux programmes externes pour plusieurs activités de maintenance.|  
|[Procédures stockées de copie des journaux de transaction](../../relational-databases/system-stored-procedures/log-shipping-stored-procedures-transact-sql.md)|Permettent de configurer, modifier et surveiller les configurations de la copie des journaux de transaction.|  
|[Procédures stockées de l’entrepôt de données de gestion &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/management-data-warehouse-stored-procedures-transact-sql.md)|Utilisé pour configurer l’entrepôt de données de gestion.|  
|[Procédures stockées OLE Automation](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)|Permettent d'activer des objets Automation standard pour une utilisation dans un lot [!INCLUDE[tsql](../../includes/tsql-md.md)] standard.|  
|[Procédures stockées de Gestion basée sur des stratégies](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)|Utilisées pour la Gestion basée sur des stratégies.|  
|[Procédures stockées PolyBase](./polybase-stored-procedures-sp-polybase-join-group.md)|Ajoutez ou supprimez un ordinateur d’un groupe de scale-out Polybase.|  
|[Procédures stockées du Magasin des requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)|Utilisé pour régler les performances.|  
|[Procédures stockées de réplication](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)|Servent à gérer la réplication.|  
|[Procédures stockées de sécurité](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)|Servent à gérer la sécurité.|  
|[Procédures stockées de sauvegarde d’instantané](./snapshot-backup-sp-delete-backup.md)|Utilisé pour supprimer la sauvegarde FILE_SNAPSHOT avec tous ses instantanés ou pour supprimer un instantané de fichier de sauvegarde individuel.|  
|[Procédures stockées d'index spatial](./spatial-index-stored-procedures-arguments-and-properties.md)|Utilisé pour analyser et améliorer les performances d'indexation d'index spatiaux.|  
|[Procédures stockées de l'Agent SQL Server](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)|Utilisées par [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pour contrôler les performances et l'activité.|  
|[Procédures stockées de SQL Server Profiler](../../relational-databases/system-stored-procedures/sql-server-profiler-stored-procedures-transact-sql.md)|Utilisées par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin de gérer les activités planifiées et liées aux événements.|  
|[Procédures stockées Stretch Database](../../relational-databases/system-stored-procedures/stretch-database-extended-stored-procedures-transact-sql.md)|Utilisé pour gérer les bases de données Stretch.|  
|[Procédures stockées des tables temporelles](./spatial-index-stored-procedures-arguments-and-properties.md)|Utilisation pour les tables temporelles|  
|[Procédures stockées XML](../../relational-databases/system-stored-procedures/xml-stored-procedures-transact-sql.md)|Permettent de gérer le texte XML.|  
  
> [!NOTE]  
>  Sauf spécification contraire, toutes les procédures stockées système retournent la valeur 0 pour indiquer la réussite d'une procédure. Pour indiquer un échec, la procédure retourne une valeur différente de zéro.  
  
## <a name="api-system-stored-procedures"></a>Procédures système API  
 Les utilisateurs qui exécutent le [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] sur des applications ADO, OLE DB et ODBC peuvent remarquer que celles-ci utilisent des procédures stockées système non abordées dans le manuel de référence [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ces procédures stockées sont utilisées par le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client et le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client pour implémenter les fonctionnalités d’une API de base de données. Ces procédures stockées sont simplement le mécanisme utilisé par le fournisseur ou le pilote afin de communiquer les requêtes des utilisateurs à une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Elles sont uniquement destinées à l'utilisation interne du fournisseur ou du pilote. Les appeler explicitement à partir d’une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] application basée sur ne sont pas pris en charge.  
  
 Les procédures stockées sp_createorphan et sp_droporphans sont utilisées pour le traitement des **images** , de **texte** et **ntext** ODBC.  
  
 La procédure stockée sp_reset_connection est utilisée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin de prendre en charge les appels de procédure stockée distante dans une transaction. Cette procédure stockée peut aussi déclencher des événements Audit Login et Audit Logout si une connexion est réutilisée dans un regroupement de connexions.  
  
 Les procédures stockées système recensées dans les tableaux suivants sont uniquement utilisées dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou par le biais d'API clientes et ne sont pas destinées à une utilisation générale. Elles peuvent faire l'objet de modifications et la compatibilité n'est pas garantie.  
  
 Les procédures stockées suivantes sont documentées dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
:::row:::
    :::column:::
        sp_catalogs
    :::column-end:::
    :::column:::
        sp_column_privileges
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_column_privileges_ex
    :::column-end:::
    :::column:::
        sp_columns
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_ex
    :::column-end:::
    :::column:::
        sp_databases
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_cursor
    :::column-end:::
    :::column:::
        sp_cursorclose
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_cursorexecute
    :::column-end:::
    :::column:::
        sp_cursorfetch
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_cursoroption
    :::column-end:::
    :::column:::
        sp_cursoropen
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_cursorprepare
    :::column-end:::
    :::column:::
        sp_cursorprepexec
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_cursorunprepare
    :::column-end:::
    :::column:::
        sp_execute
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_datatype_info
    :::column-end:::
    :::column:::
        sp_fkeys
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_foreignkeys
    :::column-end:::
    :::column:::
        sp_indexes
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_pkeys
    :::column-end:::
    :::column:::
        sp_primarykeys
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_prepare
    :::column-end:::
    :::column:::
        sp_prepexec
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_prepexecrpc
    :::column-end:::
    :::column:::
        sp_unprepare
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_server_info
    :::column-end:::
    :::column:::
        sp_special_columns
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_sproc_columns
    :::column-end:::
    :::column:::
        sp_statistics
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_privileges
    :::column-end:::
    :::column:::
        sp_table_privileges_ex
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables
    :::column-end:::
    :::column:::
        sp_tables_ex
    :::column-end:::
:::row-end:::

&nbsp;
  
Les procédures stockées suivantes ne sont pas documentées :  

:::row:::
    :::column:::
        sp_assemblies_rowset
    :::column-end:::
    :::column:::
        sp_assemblies_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_assemblies_rowset2
    :::column-end:::
    :::column:::
        sp_assembly_dependencies_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_assembly_dependencies_rowset_rmt
    :::column-end:::
    :::column:::
        sp_assembly_dependencies_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_bcp_dbcmptlevel
    :::column-end:::
    :::column:::
        sp_catalogs_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_catalogs_rowset;2
    :::column-end:::
    :::column:::
        sp_catalogs_rowset;5
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_catalogs_rowset_rmt
    :::column-end:::
    :::column:::
        sp_catalogs_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_check_constbytable_rowset
    :::column-end:::
    :::column:::
        sp_check_constbytable_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_check_constbytable_rowset2
    :::column-end:::
    :::column:::
        sp_check_constraints_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_check_constraints_rowset;2
    :::column-end:::
    :::column:::
        sp_check_constraints_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_column_privileges_rowset
    :::column-end:::
    :::column:::
        sp_column_privileges_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_column_privileges_rowset;5
    :::column-end:::
    :::column:::
        sp_column_privileges_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_column_privileges_rowset2
    :::column-end:::
    :::column:::
        sp_columns_90
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_90_rowset
    :::column-end:::
    :::column:::
        sp_columns_90_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_90_rowset2
    :::column-end:::
    :::column:::
        sp_columns_ex_90
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_rowset
    :::column-end:::
    :::column:::
        sp_columns_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_rowset;5
    :::column-end:::
    :::column:::
        sp_columns_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_columns_rowset2
    :::column-end:::
    :::column:::
        sp_constr_col_usage_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_datatype_info_90
    :::column-end:::
    :::column:::
        sp_ddopen;1
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;10
    :::column-end:::
    :::column:::
        sp_ddopen;11
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;12
    :::column-end:::
    :::column:::
        sp_ddopen;13
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;2
    :::column-end:::
    :::column:::
        sp_ddopen;3
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;4
    :::column-end:::
    :::column:::
        sp_ddopen;5
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;6
    :::column-end:::
    :::column:::
        sp_ddopen;7
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_ddopen;8
    :::column-end:::
    :::column:::
        sp_ddopen;9
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_foreign_keys_rowset
    :::column-end:::
    :::column:::
        sp_foreign_keys_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_foreign_keys_rowset;3
    :::column-end:::
    :::column:::
        sp_foreign_keys_rowset;5
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_foreign_keys_rowset_rmt
    :::column-end:::
    :::column:::
        sp_foreign_keys_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_foreign_keys_rowset3
    :::column-end:::
    :::column:::
        sp_indexes_90_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_indexes_90_rowset_rmt
    :::column-end:::
    :::column:::
        sp_indexes_90_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_indexes_rowset
    :::column-end:::
    :::column:::
        sp_indexes_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_indexes_rowset;5
    :::column-end:::
    :::column:::
        sp_indexes_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_indexes_rowset2
    :::column-end:::
    :::column:::
        sp_linkedservers_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_linkedservers_rowset;2
    :::column-end:::
    :::column:::
        sp_linkedservers_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_oledb_database
    :::column-end:::
    :::column:::
        sp_oledb_defdb
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_oledb_deflang
    :::column-end:::
    :::column:::
        sp_oledb_language
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_oledb_ro_usrname
    :::column-end:::
    :::column:::
        sp_primary_keys_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_primary_keys_rowset;2
    :::column-end:::
    :::column:::
        sp_primary_keys_rowset;3
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_primary_keys_rowset;5
    :::column-end:::
    :::column:::
        sp_primary_keys_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_primary_keys_rowset2
    :::column-end:::
    :::column:::
        sp_procedure_params_90_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_procedure_params_90_rowset2
    :::column-end:::
    :::column:::
        sp_procedure_params_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_procedure_params_rowset;2
    :::column-end:::
    :::column:::
        sp_procedure_params_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_procedures_rowset
    :::column-end:::
    :::column:::
        sp_procedures_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_procedures_rowset2
    :::column-end:::
    :::column:::
        sp_provider_types_90_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_provider_types_rowset
    :::column-end:::
    :::column:::
        sp_schemata_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_schemata_rowset;3
    :::column-end:::
    :::column:::
        sp_special_columns_90
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_sproc_columns_90
    :::column-end:::
    :::column:::
        sp_statistics_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_statistics_rowset;2
    :::column-end:::
    :::column:::
        sp_statistics_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_stored_procedures
    :::column-end:::
    :::column:::
        sp_table_constraints_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_constraints_rowset;2
    :::column-end:::
    :::column:::
        sp_table_constraints_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_privileges_rowset
    :::column-end:::
    :::column:::
        sp_table_privileges_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_privileges_rowset;5
    :::column-end:::
    :::column:::
        sp_table_privileges_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_privileges_rowset2
    :::column-end:::
    :::column:::
        sp_table_statistics_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_table_statistics_rowset;2
    :::column-end:::
    :::column:::
        sp_table_statistics2_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tablecollations
    :::column-end:::
    :::column:::
        sp_tablecollations_90
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_info_90_rowset
    :::column-end:::
    :::column:::
        sp_tables_info_90_rowset_64
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_info_90_rowset2
    :::column-end:::
    :::column:::
        sp_tables_info_90_rowset2_64
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_info_rowset
    :::column-end:::
    :::column:::
        sp_tables_info_rowset;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_info_rowset_64
    :::column-end:::
    :::column:::
        sp_tables_info_rowset_64;2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_info_rowset2
    :::column-end:::
    :::column:::
        sp_tables_info_rowset2_64
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_rowset;2
    :::column-end:::
    :::column:::
        sp_tables_rowset;5
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_tables_rowset_rmt
    :::column-end:::
    :::column:::
        sp_tables_rowset2
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_usertypes_rowset
    :::column-end:::
    :::column:::
        sp_usertypes_rowset_rmt
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_usertypes_rowset2
    :::column-end:::
    :::column:::
        sp_views_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_views_rowset2
    :::column-end:::
    :::column:::
        sp_xml_schema_rowset
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        sp_xml_schema_rowset2
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::  

## <a name="see-also"></a>Voir aussi  
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [Procédures stockées &#40;moteur de base de données&#41;](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)   
 [Exécution de procédures stockées &#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/stored-procedures-running.md)   
 [Exécution de procédures stockées](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)   
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Exécution de procédures stockées](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)  
  
