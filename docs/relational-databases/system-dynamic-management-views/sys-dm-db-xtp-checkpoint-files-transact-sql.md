---
title: sys.dm_db_xtp_checkpoint_files (Transact-SQL) | Microsoft Docs
description: Affiche des informations sur les fichiers de point de contrôle, y compris la taille de fichier, l’emplacement physique et l’ID de transaction. Découvrez en quoi cette vue diffère pour les versions de SQL Server.
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: in-memory-oltp
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_checkpoint_files
- sys.dm_db_xtp_checkpoint_files_TSQL
- dm_db_xtp_checkpoint_files_TSQL
- sys.dm_db_xtp_checkpoint_files
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_checkpoint_files dynamic management view
ms.assetid: ac8e6333-7a9f-478a-b446-5602283e81c9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bed4fa18b01a295815c07f6a0f37d4bc0b942c16
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092908"
---
# <a name="sysdm_db_xtp_checkpoint_files-transact-sql"></a>sys.dm_db_xtp_checkpoint_files (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Affiche des informations sur les fichiers de point de contrôle, y compris la taille de fichier, l'emplacement physique et l'ID de la transaction.  
  
> **Remarque :** Pour le point de contrôle actuel qui n’est pas fermé, la colonne d’état de s sera en cours `ys.dm_db_xtp_checkpoint_files` de construction pour les nouveaux fichiers. Un point de contrôle se ferme automatiquement lorsque la croissance du journal des transactions est suffisante depuis le dernier point de contrôle, ou si vous émettez la `CHECKPOINT` commande ([point de contrôle &#40;&#41;Transact-SQL ](../../t-sql/language-elements/checkpoint-transact-sql.md)).  
  
 Un groupe de fichiers à mémoire optimisée utilise en interne des fichiers ajoutés uniquement pour stocker des lignes insérées et supprimées pour les tables en mémoire. Il existe deux types de fichiers : Un fichier de données contient des lignes insérées alors qu’un fichier Delta contient des références à des lignes supprimées. 
  
 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] est fondamentalement différent des versions plus récentes et est abordé plus bas dans la rubrique [SQL Server 2014](#bkmk_2014).  
  
 Pour plus d’informations, consultez [création et gestion du stockage pour les objets Memory-Optimized](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md).  
  
##  <a name="sssql15-and-later"></a><a name="bkmk_2016"></a> [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et ultérieur  
 Le tableau suivant décrit les colonnes de `sys.dm_db_xtp_checkpoint_files` , à partir de **[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]** .  
  
|Nom de la colonne|Type|Description|  
|-----------------|----------|-----------------|  
|container_id|**int**|ID du conteneur (représenté en tant que fichier de type FILESTREAM dans sys.database_files) dont les données ou le fichier delta font partie. Jointures avec file_id dans [sys.database_files &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md).|  
|container_guid|**uniqueidentifier**|GUID du conteneur auquel appartient la racine, le fichier de données ou le fichier delta. Jointures avec file_guid dans la table sys.database_files.|  
|checkpoint_file_id|**uniqueidentifier**|GUID du fichier de point de contrôle.|  
|relative_file_path|**nvarchar (256)**|Chemin d’accès du fichier par rapport au conteneur auquel il est mappé.|  
|file_type|**smallint**|-1 gratuitement<br /><br /> 0 pour le fichier de données.<br /><br /> 1 pour le fichier DELTA.<br /><br /> 2 pour le fichier racine<br /><br /> 3 pour un fichier de données volumineux|  
|file_type_desc|**nvarchar(60)**|GRATUIT : tous les fichiers gérés gratuitement peuvent être alloués. La taille des fichiers gratuits peut varier en fonction des besoins prévus du système. La taille maximale est de 1 Go.<br /><br /> Les fichiers de données contiennent des lignes qui ont été insérées dans des tables optimisées en mémoire.<br /><br /> Les fichiers Delta-Delta contiennent des références à des lignes de fichiers de données qui ont été supprimées.<br /><br /> Les fichiers racine racine contiennent les métadonnées système pour les objets optimisés en mémoire et compilés en mode natif.<br /><br /> DONNÉES VOLUMINEUSEs : les fichiers de données volumineux contiennent des valeurs insérées dans les colonnes (n) varchar (max) et varbinary (max), ainsi que les segments de colonne qui font partie des index ColumnStore sur les tables optimisées en mémoire.|  
|internal_storage_slot|**int**|Index du fichier dans le tableau de stockage interne. NULL pour la racine ou pour l’État autre que 1.|  
|checkpoint_pair_file_id|**uniqueidentifier**|Fichier de données ou DELTA correspondant. NULL pour la racine.|  
|file_size_in_bytes|**bigint**|Taille du fichier sur le disque.|  
|file_size_used_in_bytes|**bigint**|Pour les paires de fichiers de point de contrôle dont le remplissage est toujours en cours, cette colonne est mise à jour après le point de contrôle suivant.|  
|logical_row_count|**bigint**|Pour les données, nombre de lignes insérées.<br /><br /> Pour Delta, nombre de lignes supprimées après la comptabilisation de drop table.<br /><br /> Pour la racine, NULL.|  
|state|**smallint**|0-PRÉCRÉÉ<br /><br /> 1-EN COURS DE CONSTRUCTION<br /><br /> 2 - ACTIVE<br /><br /> 3-CIBLE DE FUSION<br /><br /> 8-EN ATTENTE DE TRONCATION DU JOURNAL|  
|state_desc|**nvarchar(60)**|Precreated : un certain nombre de fichiers de point de contrôle sont pré-alloués pour réduire ou éliminer les attentes visant à allouer de nouveaux fichiers au fur et à mesure de l’exécution des transactions. La taille de ces fichiers précréés peut varier en fonction des besoins estimés de la charge de travail, mais ils ne contiennent pas de données. Il s’agit d’une surcharge de stockage dans les bases de données avec un groupe de fichiers MEMORY_OPTIMIZED_DATA.<br /><br /> En cours de CONSTRUCTION, ces fichiers de point de contrôle sont en cours de construction, ce qui signifie qu’ils sont remplis en fonction des enregistrements de journal générés par la base de données et qu’ils ne font pas encore partie d’un point de contrôle.<br /><br /> ACTIF : ils contiennent les lignes insérées/supprimées des points de contrôle fermés précédents. Ils contiennent le contenu des tables qui sont lues en mémoire avant l’application de la partie active du journal des transactions au redémarrage de la base de données. Nous pensons que la taille de ces fichiers de point de contrôle est approximativement de 2 fois la taille en mémoire des tables optimisées en mémoire, en supposant que l’opération de fusion respecte la charge de travail transactionnelle.<br /><br /> CIBLE de fusion-cible des opérations de fusion-ces fichiers de point de contrôle stockent les lignes de données consolidées à partir des fichiers sources qui ont été identifiés par la stratégie de fusion. Une fois que la fusion est installée, l'état MERGE TARGET passe à ACTIVE.<br /><br /> En attente de TRONCation du journal-une fois que la fusion a été installée et que la PCP cible de fusion fait partie d’un point de contrôle durable, les fichiers de point de contrôle de la source de fusion passent à cet État. Les fichiers dans cet État sont nécessaires à l’exactitude opérationnelle de la base de données avec la table optimisée en mémoire.  Par exemple, pour la récupération à partir d'un point de contrôle durable afin de remonter dans le temps.|  
|lower_bound_tsn|**bigint**|Limite inférieure de la transaction dans le fichier ; NULL si l’État n’est pas dans (1, 3).|  
|upper_bound_tsn|**bigint**|Limite supérieure de la transaction dans le fichier ; NULL si l’État n’est pas dans (1, 3).|  
|begin_checkpoint_id|**bigint**|ID du point de contrôle de début.|  
|end_checkpoint_id|**bigint**|ID du point de contrôle de fin.|  
|last_updated_checkpoint_id|**bigint**|ID du dernier point de contrôle qui a mis à jour ce fichier.|  
|encryption_status|**smallint**|0, 1, 2|  
|encryption_status_desc|**nvarchar(60)**|0 => NON CHIFFRÉ<br /><br /> 1 => CHIFFRÉE AVEC LA CLÉ 1<br /><br /> 2 => CHIFFRÉE AVEC LA CLÉ 2. Valide uniquement pour les fichiers actifs.|  
  
##  <a name="sssql14"></a><a name="bkmk_2014"></a> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 Le tableau suivant décrit les colonnes pour `sys.dm_db_xtp_checkpoint_files` , pour **[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]** .  
  
|Nom de la colonne|Type|Description|  
|-----------------|----------|-----------------|  
|container_id|**int**|ID du conteneur (représenté en tant que fichier de type FILESTREAM dans sys.database_files) dont les données ou le fichier delta font partie. Jointures avec file_id dans [sys.database_files &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md).|  
|container_guid|**uniqueidentifier**|GUID du conteneur dont les données ou le fichier delta font partie.|  
|checkpoint_file_id|**GUID**|ID du fichier de données ou delta.|  
|relative_file_path|**nvarchar (256)**|Chemin d'accès au fichier de données ou delta, relatif à l'emplacement du conteneur.|  
|file_type|**tinyint**|0 pour le fichier de données.<br /><br /> 1 pour le fichier delta.<br /><br /> NULL si la colonne d'état a la valeur 7.|  
|file_type_desc|**nvarchar(60)**|Type de fichier : DATA_FILE, DELTA_FILE ou NULL si la colonne d’État a la valeur 7.|  
|internal_storage_slot|**int**|Index du fichier dans le tableau de stockage interne. NULL si la colonne d'état n'a pas la valeur 2 ni 3.|  
|checkpoint_pair_file_id|**uniqueidentifier**|Fichier de données ou delta correspondant.|  
|file_size_in_bytes|**bigint**|Taille du fichier utilisée. NULL si la colonne d'état a la valeur 5, 6, ou 7.|  
|file_size_used_in_bytes|**bigint**|Taille utilisée du fichier. NULL si la colonne d'état a la valeur 5, 6, ou 7.<br /><br /> Pour les paires de fichiers de point de contrôle dont le remplissage est toujours en cours, cette colonne est mise à jour après le point de contrôle suivant.|  
|inserted_row_count|**bigint**|Nombre de lignes dans le fichier de données.|  
|deleted_row_count|**bigint**|Nombre de lignes supprimées dans le fichier delta.|  
|drop_table_deleted_row_count|**bigint**|Nombre de lignes dans les fichiers de données affectées par une table de suppression. S'applique aux fichiers de données lorsque la colonne d'état est égale à 1.<br /><br /> Affiche les lignes supprimées des tables supprimées. Les statistiques drop_table_deleted_row_count sont compilées après que l'opération de garbage collection pour libérer la mémoire des lignes des tables supprimées est terminée et un point de contrôle est effectué. Si vous redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avant que les statistiques des tables supprimées soient reflétées dans cette colonne, les statistiques seront mises à jour dans le cadre de la récupération. Le processus de récupération ne charge pas de lignes des tables supprimées. Les statistiques des tables supprimées sont compilées pendant la phase de chargement et reportées dans cette colonne lorsque la récupération est terminée.|  
|state|**int**|0-PRÉCRÉÉ<br /><br /> 1-EN COURS DE CONSTRUCTION<br /><br /> 2 - ACTIVE<br /><br /> 3-CIBLE DE FUSION<br /><br /> 4-SOURCE FUSIONNÉE<br /><br /> 5-REQUIS POUR LA SAUVEGARDE/HAUTE DISPONIBILITÉ<br /><br /> 6-EN TRANSITION VERS L’OBJET TOMBSTONE<br /><br /> 7-DÉSACTIVÉ|  
|state_desc|**nvarchar(60)**|Precreated : un petit ensemble de paires de fichiers de données et Delta, également appelé « paires de fichiers de point de contrôle » (paires), est conservé pré-alloué pour réduire ou éliminer les attentes d’allocation de nouveaux fichiers au fur et à mesure de l’exécution des transactions. Elles ont une taille complète avec un fichier de données de 128 Mo et un fichier delta de 8 Mo, mais ne contenant aucune donnée. Le nombre de paires de fichiers de point de contrôle est calculé en fonction du nombre de processeurs logiques ou de planificateurs (un par cœur, sans limitation) avec un minimum de 8. Il s'agit d'une charge de stockage fixe dans les bases de données avec des tables mémoire optimisées.<br /><br /> SOUS CONSTRUCTION : jeu de paires qui stocke les lignes de données nouvellement insérées et éventuellement supprimées depuis le dernier point de contrôle.<br /><br /> ACTIVE - Contient les lignes insérées et les lignes supprimées provenant des points de contrôle fermés précédents. Ces paires contiennent toutes les lignes insérées et supprimées requises avant d'appliquer la partie actuelle du journal des transactions au redémarrage de la base de données. La taille de ces paires de fichiers de point de contrôle sera approximativement 2 fois la taille en mémoire des tables mémoire optimisées, en supposant que l'opération de fusion est actualisée avec la charge de travail transactionnelle.<br /><br /> FUSIONNer la cible : la PCP stocke les lignes de données consolidées à partir de la PCP (s) qui ont été identifiées par la stratégie de fusion. Une fois que la fusion est installée, l'état MERGE TARGET passe à ACTIVE.<br /><br /> SOURCE FUSIONNée-une fois l’opération de fusion installée, les paires sources sont marqués comme SOURCE FUSIONNée. Notez que l'évaluateur de la stratégie de fusion peut identifier plusieurs fusions mais une paire de fichiers de point de contrôle ne peut participer qu'à une seule opération de fusion.<br /><br /> REQUIS pour BACKUP/HA-une fois que la fusion a été installée et que la PCP cible de fusion fait partie d’un point de contrôle durable, la source de fusion paires passe à cet État. Des paires de fichiers de point de contrôle ayant cet état sont nécessaires pour le bon fonctionnement de la base de données contenant la table mémoire optimisée.  Par exemple, pour la récupération à partir d'un point de contrôle durable afin de remonter dans le temps. Une paire de fichiers de point de contrôle peut être marquée pour l'opération de garbage collection une fois que le point de troncation de journal dépasse sa plage de transactions.<br /><br /> EN TRANSITION vers l’objet tombstone, ces paires ne sont pas nécessaires pour le moteur OLTP In-Memory et peuvent être récupérés par le garbage collector. Cet état indique que ces paires de fichiers de point de contrôle attendent que le thread d'arrière-plan les passe à l'état suivant, qui est TOMBSTONE.<br /><br /> TOMBSTONE : ces paires attendent d’être récupérés par le garbage collector FileStream. ([sp_filestream_force_garbage_collection &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md))|  
|lower_bound_tsn|**bigint**|Limite inférieure des transactions contenues dans le fichier. Null si la colonne d'état a une valeur autre que 2, 3 ou 4.|  
|upper_bound_tsn|**bigint**|Limite supérieure des transactions contenues dans le fichier. Null si la colonne d'état a une valeur autre que 2, 3 ou 4.|  
|last_backup_page_count|**int**|Nombre de pages logiques déterminé lors de la dernière sauvegarde. S'applique quand la colonne d'état a la valeur 2, 3, 4 ou 5. NULL si le nombre de pages est inconnu.|  
|delta_watermark_tsn|**int**|Transaction du dernier point de contrôle qui a écrit dans ce fichier delta. Il s'agit de la filigrane du fichier delta.|  
|last_checkpoint_recovery_lsn|**nvarchar (23)**|Numéro séquentiel dans le journal de récupération du dernier point de contrôle qui requiert toujours le fichier.|  
|tombstone_operation_lsn|**nvarchar (23)**|Le fichier sera supprimé une fois que le tombstone_operation_lsn passe derrière le numéro séquentiel de troncation du journal.|  
|logical_deletion_log_block_id|**bigint**|S'applique uniquement à l'état 5.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation `VIEW DATABASE STATE` sur le serveur.  
  
## <a name="use-cases"></a>Scénarios d’utilisation  
 Vous pouvez estimer le stockage utilisé par In-Memory OLTP comme suit :  
  
```  
-- total storage used by In-Memory OLTP  
SELECT SUM (file_size_in_bytes)/(1024*1024) as file_size_in_MB  
FROM sys.dm_db_xtp_checkpoint_files  
```  
  
  
Pour voir une répartition de l’utilisation du stockage par État et type de fichier, exécutez la requête suivante :
  
```  
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(file_size_in_bytes) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  

  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des tables à mémoire optimisée &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
