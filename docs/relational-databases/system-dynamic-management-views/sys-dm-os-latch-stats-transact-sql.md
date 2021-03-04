---
description: sys.dm_os_latch_stats (Transact-SQL)
title: sys.dm_os_latch_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_latch_stats_TSQL
- dm_os_latch_stats_TSQL
- dm_os_latch_stats
- sys.dm_os_latch_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_latch_stats dynamic management view
ms.assetid: 2085d9fc-828c-453e-82ec-b54ed8347ae5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d1a8d34673b28e1da4fb15b0f469740aab378391
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839178"
---
# <a name="sysdm_os_latch_stats-transact-sql"></a>sys.dm_os_latch_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne des informations sur toutes les attentes de verrou interne, organisées par classe. 
  
> [!NOTE]  
> Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_os_latch_stats**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|latch_class|**nvarchar(60)**|Nom de la classe de verrou interne.|  
|waiting_requests_count|**bigint**|Nombre d'attentes pour les verrous internes de cette classe. Ce compteur est incrémenté au début d'une attente de verrou interne.|  
|wait_time_ms|**bigint**|Temps d'attente total sur les verrous internes de cette classe, en millisecondes.<br /><br /> **Remarque :** Cette colonne est mise à jour toutes les cinq minutes pendant l’attente d’un verrou et à la fin de l’attente d’un verrou.|  
|max_wait_time_ms|**bigint**|Durée maximum d'attente d'un objet de mémoire sur ce verrou interne. Si cette valeur est anormalement élevée, cela peut indiquer un blocage interne.|  
|pdw_node_id|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations  
Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
  
## <a name="remarks"></a>Notes  
 sys.dm_os_latch_stats peut être utilisé pour identifier l'origine d'une contention de verrouillage en examinant le nombre d'attentes et les temps d'attente relatifs pour les différentes classes de verrous internes. Dans certaines situations, vous pouvez être en mesure de résoudre ou de réduire les problèmes de contention de verrouillage. Il peut toutefois arriver que vous soyez obligé de contacter le Support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
Vous pouvez réinitialiser le contenu de sys.dm_os_latch_stats en utilisant `DBCC SQLPERF` de la manière suivante :  
  
```sql  
DBCC SQLPERF ('sys.dm_os_latch_stats', CLEAR);  
GO  
```  
  
 Cette commande remet tous les compteurs à 0.  
  
> [!NOTE]  
>  Ces statistiques ne sont pas conservées si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] redémarre. Toutes les données sont cumulées à partir de la dernière réinitialisation des statistiques ou à partir du démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="latches"></a><a name="latches"></a> Verrous  
 Un verrou est un objet de synchronisation légère interne similaire à un verrou, qui est utilisé par différents [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] composants. Un verrou est principalement utilisé pour synchroniser des pages de base de données pendant des opérations telles que la mémoire tampon ou l’accès aux fichiers. Chaque verrou interne est associé à une seule unité d'allocation. 
  
 Une attente de verrou interne se produit lorsqu'une demande de verrou interne ne peut pas être satisfaite immédiatement parce que le verrou en question est détenu par un autre thread qui crée une situation de conflit. Contrairement aux verrous externes, les verrous internes sont libérés dès la fin de l'opération (y compris dans le cas d'opérations d'écriture).  
  
 Les verrous internes sont regroupés en classes en fonction de leur utilisation et des composants. Dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il peut à tout moment exister zéro verrou, un verrou ou plusieurs verrous internes d'une classe donnée.  
  
> [!NOTE]  
> `sys.dm_os_latch_stats` n'assure pas le suivi des demandes de verrou qui ont été satisfaites immédiatement ou qui ont échoué sans attente.  
  
 Le tableau suivant fournit une description succincte des différentes classes de verrous internes.  
  
|Classe de verrou interne|Description|  
|-----------------|-----------------|  
|ALLOC_CREATE_RINGBUF|Verrous utilisés en interne par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour initialiser la synchronisation de la création d'un tampon d'allocation en anneau.|  
|ALLOC_CREATE_FREESPACE_CACHE|Verrous utilisés pour initialiser la synchronisation de caches d'espace libre internes pour les segments de mémoire.|  
|ALLOC_CACHE_MANAGER|Verrous utilisés pour synchroniser les tests de cohérence internes.|  
|ALLOC_FREESPACE_CACHE|Verrous utilisés pour synchroniser l'accès à un cache de pages contenant de l'espace libre pour des segments de mémoire et des objets volumineux binaires (BLOB). Sur les verrous de cette classe, il peut y avoir contention lorsque plusieurs connexions essaient simultanément d'insérer des lignes dans un segment de mémoire ou un objet BLOB. Vous pouvez réduire cette contention en partitionnant l'objet. Chaque partition a son propre verrou interne. Le partitionnement répartit les insertions entre plusieurs verrous internes.|  
|ALLOC_EXTENT_CACHE|Verrous utilisés pour synchroniser l'accès à un cache d'extensions qui contient des pages non allouées. Sur les verrous de cette classe, il peut y avoir contention lorsque plusieurs connexions essaient simultanément d'allouer des pages de données dans la même unité d'allocation. Vous pouvez réduire cette contention en partitionnant l'objet dont cette unité d'allocation fait partie.|  
|ACCESS_METHODS_DATASET_PARENT|Verrous utilisés pour synchroniser l'accès des datasets enfants au dataset parent pendant les opérations en parallèle.|  
|ACCESS_METHODS_HOBT_FACTORY|Verrous utilisés pour synchroniser l'accès à une table de hachage interne.|  
|ACCESS_METHODS_HOBT|Verrous utilisés pour synchroniser l'accès à la représentation d'un HoBt en mémoire.|  
|ACCESS_METHODS_HOBT_COUNT|Verrous utilisés pour synchroniser l'accès aux compteurs de pages et de lignes d'un HoBt.|  
|ACCESS_METHODS_HOBT_VIRTUAL_ROOT|Verrous utilisés pour synchroniser l'accès à l'abstraction de page racine d'un arbre B (B-tree) interne.|  
|ACCESS_METHODS_CACHE_ONLY_HOBT_ALLOC|Verrous utilisés pour synchroniser l'accès aux tables de travail.|  
|ACCESS_METHODS_BULK_ALLOC|Verrous utilisés pour synchroniser l'accès au sein d'allocateurs en bloc.|  
|ACCESS_METHODS_SCAN_RANGE_GENERATOR|Verrous utilisés pour synchroniser l'accès à un générateur de plages pendant des analyses parallèles.|  
|ACCESS_METHODS_KEY_RANGE_GENERATOR|Verrous utilisés pour synchroniser l'accès aux opérations de lecture anticipée pendant les analyses parallèles de plages de clés.|  
|APPEND_ONLY_STORAGE_INSERT_POINT|Verrous utilisés pour synchroniser les insertions dans les unités de stockage rapides en mode Append-Only.|  
|APPEND_ONLY_STORAGE_FIRST_ALLOC|Verrous utilisés pour synchroniser la première allocation pour une unité de stockage en mode Append-Only.|  
|APPEND_ONLY_STORAGE_UNIT_MANAGER|Verrous utilisés pour synchroniser l'accès à la structure interne des données au sein du gestionnaire d'unités de stockage rapide en mode Append-Only.|  
|APPEND_ONLY_STORAGE_MANAGER|Verrous utilisés pour synchroniser les opérations de compactage dans le gestionnaire d'unités de stockage rapide en mode Append-Only.|  
|BACKUP_RESULT_SET|Verrous utilisés pour synchroniser les jeux de résultats des sauvegardes parallèles.|  
|BACKUP_TAPE_POOL|Verrous utilisés pour synchroniser les pools de bandes de sauvegarde.|  
|BACKUP_LOG_REDO|Verrous utilisés pour synchroniser les opérations de restauration par progression des journaux de sauvegarde.|  
|BACKUP_INSTANCE_ID|Verrous utilisés pour synchroniser la génération d'ID d'instance pour les compteurs de l'analyseur de performances de sauvegarde.|  
|BACKUP_MANAGER|Verrous utilisés pour synchroniser le gestionnaire de sauvegarde interne.|  
|BACKUP_MANAGER_DIFFERENTIAL|Verrous utilisés pour synchroniser les opérations de sauvegarde différentielle avec DBCC.|  
|BACKUP_OPERATION|Verrous utilisés pour la synchronisation de la structure interne des données dans une opération de sauvegarde (de base de données, de journal ou de fichiers).|  
|BACKUP_FILE_HANDLE|Verrous utilisés pour synchroniser les opérations d'ouverture de fichier pendant une opération de restauration.|  
|BUFFER|Verrous utilisés pour synchroniser l'accès à court terme aux pages de base de données. Un verrou de mémoire tampon est nécessaire avant la lecture ou la modification d'une page de base de données. Une contention de verrou de ce type peut indiquer plusieurs problèmes, notamment des pages sensibles et des E/S lentes.<br /><br /> Cette classe de verrous englobe toutes les utilisations possibles des verrous de page. sys.dm_os_wait_stats fait une différence entre les attentes de verrous de page provoquées par des opérations d’e/s et des opérations de lecture et d’écriture sur la page.|  
|BUFFER_POOL_GROW|Verrous utilisés pour la synchronisation du gestionnaire de mémoires tampons interne pendant les opérations de développement du pool de mémoires tampons.|  
|DATABASE_CHECKPOINT|Verrous utilisés pour sérialiser les points de contrôle dans une base de données.|  
|CLR_PROCEDURE_HASHTABLE|À usage interne uniquement|  
|CLR_UDX_STORE|À usage interne uniquement|  
|CLR_DATAT_ACCESS|À usage interne uniquement|  
|CLR_XVAR_PROXY_LIST|À usage interne uniquement|  
|DBCC_CHECK_AGGREGATE|À usage interne uniquement|  
|DBCC_CHECK_RESULTSET|À usage interne uniquement|  
|DBCC_CHECK_TABLE|À usage interne uniquement|  
|DBCC_CHECK_TABLE_INIT|À usage interne uniquement|  
|DBCC_CHECK_TRACE_LIST|À usage interne uniquement|  
|DBCC_FILE_CHECK_OBJECT|À usage interne uniquement|  
|DBCC_PERF|Verrous utilisés pour synchroniser les compteurs de l'analyseur de performances interne.|  
|DBCC_PFS_STATUS|À usage interne uniquement|  
|DBCC_OBJECT_METADATA|À usage interne uniquement|  
|DBCC_HASH_DLL|À usage interne uniquement|  
|EVENTING_CACHE|À usage interne uniquement|  
|FCB|Verrous utilisés pour synchroniser l'accès au bloc de contrôle des fichiers.|  
|FCB_REPLICA|À usage interne uniquement|  
|FGCB_ALLOC|Verrous utilisés pour synchroniser l'accès aux informations d'allocation utilisant le mécanisme du tourniquet (round robin) au sein d'un groupe de fichiers.|  
|FGCB_ADD_REMOVE|Utilisez pour synchroniser l’accès aux groupes de fichiers pour les opérations d’ajout, de suppression, d’agrandissement et de réduction de fichier.|  
|FILEGROUP_MANAGER|À usage interne uniquement|  
|FILE_MANAGER|À usage interne uniquement|  
|FILESTREAM_FCB|À usage interne uniquement|  
|FILESTREAM_FILE_MANAGER|À usage interne uniquement|  
|FILESTREAM_GHOST_FILES|À usage interne uniquement|  
|FILESTREAM_DFS_ROOT|À usage interne uniquement|  
|LOG_MANAGER|À usage interne uniquement|  
|FULLTEXT_DOCUMENT_ID|À usage interne uniquement|  
|FULLTEXT_DOCUMENT_ID_TRANSACTION|À usage interne uniquement|  
|FULLTEXT_DOCUMENT_ID_NOTIFY|À usage interne uniquement|  
|FULLTEXT_LOGS|À usage interne uniquement|  
|FULLTEXT_CRAWL_LOG|À usage interne uniquement|  
|FULLTEXT_ADMIN|À usage interne uniquement|  
|FULLTEXT_ADMIN_COMMAND_CACHE|À usage interne uniquement|  
|FULLTEXT_LANGUAGE_TABLE|À usage interne uniquement|  
|FULLTEXT_CRAWL_DM_LIST|À usage interne uniquement|  
|FULLTEXT_CRAWL_CATALOG|À usage interne uniquement|  
|FULLTEXT_FILE_MANAGER|À usage interne uniquement|  
|DATABASE_MIRRORING_REDO|À usage interne uniquement|  
|DATABASE_MIRRORING_SERVER|À usage interne uniquement|  
|DATABASE_MIRRORING_CONNECTION|À usage interne uniquement|  
|DATABASE_MIRRORING_STREAM|À usage interne uniquement|  
|QUERY_OPTIMIZER_VD_MANAGER|À usage interne uniquement|  
|QUERY_OPTIMIZER_ID_MANAGER|À usage interne uniquement|  
|QUERY_OPTIMIZER_VIEW_REP|À usage interne uniquement|  
|RECOVERY_BAD_PAGE_TABLE|À usage interne uniquement|  
|RECOVERY_MANAGER|À usage interne uniquement|  
|SECURITY_OPERATION_RULE_TABLE|À usage interne uniquement|  
|SECURITY_OBJPERM_CACHE|À usage interne uniquement|  
|SECURITY_CRYPTO|À usage interne uniquement|  
|SECURITY_KEY_RING|À usage interne uniquement|  
|SECURITY_KEY_LIST|À usage interne uniquement|  
|SERVICE_BROKER_CONNECTION_RECEIVE|À usage interne uniquement|  
|SERVICE_BROKER_TRANSMISSION|À usage interne uniquement|  
|SERVICE_BROKER_TRANSMISSION_UPDATE|À usage interne uniquement|  
|SERVICE_BROKER_TRANSMISSION_STATE|À usage interne uniquement|  
|SERVICE_BROKER_TRANSMISSION_ERRORS|À usage interne uniquement|  
|SSBXmitWork|À usage interne uniquement|  
|SERVICE_BROKER_MESSAGE_TRANSMISSION|À usage interne uniquement|  
|SERVICE_BROKER_MAP_MANAGER|À usage interne uniquement|  
|SERVICE_BROKER_HOST_NAME|À usage interne uniquement|  
|SERVICE_BROKER_READ_CACHE|À usage interne uniquement|  
|SERVICE_BROKER_WAITFOR_MANAGER| Utilisé pour synchroniser un mappage au niveau de l’instance des files d’attente d’attente. Il existe une file d’attente par ID de base de données, version de base de données et tuple d’ID de file d’attente. Une contention sur les verrous internes de cette classe peut se produire lorsque de nombreuses connexions sont : dans un état d’attente WAITFOR (RECEIVE); appel de WAITFOR (RECEIVE); dépassement du délai d’attente WAITFOR ; réception d’un message ; validation ou restauration de la transaction qui contient le WAITFOR (réception); Vous pouvez réduire la contention en réduisant le nombre de threads dans un état d’attente WAITFOR (RECEIVE). |  
|SERVICE_BROKER_WAITFOR_TRANSACTION_DATA|À usage interne uniquement|  
|SERVICE_BROKER_TRANSMISSION_TRANSACTION_DATA|À usage interne uniquement|  
|SERVICE_BROKER_TRANSPORT|À usage interne uniquement|  
|SERVICE_BROKER_MIRROR_ROUTE|À usage interne uniquement|  
|TRACE_ID|À usage interne uniquement|  
|TRACE_AUDIT_ID|À usage interne uniquement|  
|TRACE|À usage interne uniquement|  
|TRACE_CONTROLLER|À usage interne uniquement|  
|TRACE_EVENT_QUEUE|À usage interne uniquement|  
|TRANSACTION_DISTRIBUTED_MARK|À usage interne uniquement|  
|TRANSACTION_OUTCOME|À usage interne uniquement|  
|NESTING_TRANSACTION_READONLY|À usage interne uniquement|  
|NESTING_TRANSACTION_FULL|À usage interne uniquement|  
|MSQL_TRANSACTION_MANAGER|À usage interne uniquement|  
|DATABASE_AUTONAME_MANAGER|À usage interne uniquement|  
|UTILITY_DYNAMIC_VECTOR|À usage interne uniquement|  
|UTILITY_SPARSE_BITMAP|À usage interne uniquement|  
|UTILITY_DATABASE_DROP|À usage interne uniquement|  
|UTILITY_DYNAMIC_MANAGER_VIEW|À usage interne uniquement|  
|UTILITY_DEBUG_FILESTREAM|À usage interne uniquement|  
|UTILITY_LOCK_INFORMATION|À usage interne uniquement|  
|VERSIONING_TRANSACTION|À usage interne uniquement|  
|VERSIONING_TRANSACTION_LIST|À usage interne uniquement|  
|VERSIONING_TRANSACTION_CHAIN|À usage interne uniquement|  
|VERSIONING_STATE|À usage interne uniquement|  
|VERSIONING_STATE_CHANGE|À usage interne uniquement|  
|KTM_VIRTUAL_CLOCK|À usage interne uniquement|  
  
## <a name="see-also"></a>Voir aussi  
[DBCC SQLPERF &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md)       
[SQL Server vues de gestion dynamique liées au système d’exploitation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)       
[SQL Server - Objet Latches](../../relational-databases/performance-monitor/sql-server-latches-object.md)