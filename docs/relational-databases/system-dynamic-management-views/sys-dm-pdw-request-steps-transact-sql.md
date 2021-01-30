---
description: sys.dm_pdw_request_steps (Transact-SQL)
title: sys.dm_pdw_request_steps (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/28/2020
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: cc563e88-0d34-436e-b914-b60d6ee0d50b
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 08d39aff859ad993ec78fba73bfbad800945b461
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99139529"
---
# <a name="sysdm_pdw_request_steps-transact-sql"></a>sys.dm_pdw_request_steps (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contient des informations sur toutes les étapes qui composent une requête ou une requête donnée dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] . Elle répertorie une ligne par étape de requête.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|request_id et step_index constituent la clé de cette vue.<br /><br /> ID numérique unique associé à la demande.|Consultez request_id dans [sys.dm_pdw_exec_requests &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|step_index|**int**|request_id et step_index constituent la clé de cette vue.<br /><br /> Position de cette étape dans la séquence d’étapes qui composent la demande.|0 à (n-1) pour une demande avec n étapes.|  
|plan_node_id|**int**|ID de nœud correspondant à l’ID d’opérateur de cette étape dans le plan d’exécution.|Aucun|  
|operation_type|**nvarchar(35)**|Type d’opération représenté par cette étape.|**Opérations du plan de requête DMS :** 'ReturnOperation', 'PartitionMoveOperation', 'MoveOperation', 'BroadcastMoveOperation', 'ShuffleMoveOperation', 'TrimMoveOperation', 'CopyOperation', 'DistributeReplicatedTableMoveOperation'<br /><br /> **Opérations du plan de requête SQL :** 'OnOperation', 'RemoteOperation'<br /><br /> **Autres opérations de plan de requête :** 'MetaDataCreateOperation', 'RandomIDOperation'<br /><br /> **Opérations externes pour les lectures :** 'HadoopShuffleOperation', 'HadoopRoundRobinOperation', 'HadoopBroadcastOperation'<br /><br /> **Opérations externes pour MapReduce :** 'HadoopJobOperation', 'HdfsDeleteOperation'<br /><br /> **Opérations externes pour les écritures :** 'ExternalExportDistributedOperation', 'ExternalExportReplicatedOperation', 'ExternalExportControlOperation'<br /><br /> Pour plus d’informations, consultez « comprendre les plans de requête » dans le [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] . <br /><br />  Un plan de requête peut également être affecté par les paramètres de base de données.  Pour plus d’informations, consultez [options ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md?bc=%252fazure%252fsql-data-warehouse%252fbreadcrumb%252ftoc.json&toc=%252fazure%252fsql-data-warehouse%252ftoc.json&view=azure-sqldw-latest&preserve-view=true) .|  
|distribution_type|**nvarchar(32)**|Type de distribution que cette étape doit subir.|'AllNodes', 'AllDistributions', 'AllComputeNodes', 'ComputeNode', 'distribution', 'SubsetNodes', 'SubsetDistributions', 'Unspecified'|  
|location_type|**nvarchar(32)**|Emplacement d’exécution de l’étape.|« COMPUTE », « Control », « DMS »|  
|status|**nvarchar(32)**|État de cette étape.|En attente, en cours d’exécution, terminé, en échec, UndoFailed, PendingCancel, annulé, annulé, abandonné|  
|error_id|**nvarchar (36)**|ID unique de l’erreur associée à cette étape, le cas échéant.|Consultez error_id de [sys.dm_pdw_errors &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-errors-transact-sql.md). NULL si aucune erreur ne s’est produite.|  
|start_time|**datetime**|Heure à laquelle l’étape a démarré l’exécution.|Inférieure ou égale à l’heure actuelle et supérieure ou égale à end_compile_time de la requête à laquelle cette étape appartient. Pour plus d’informations sur les requêtes, consultez [sys.dm_pdw_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|end_time|**datetime**|Heure à laquelle cette étape s’est terminée, a été annulée ou a échoué.|Inférieure ou égale à l’heure actuelle et supérieure ou égale à start_time. Affectez la valeur NULL pour les étapes en cours d’exécution ou en attente.|  
|total_elapsed_time|**int**|Durée totale d’exécution de l’étape de la requête, en millisecondes.|Entre 0 et la différence entre end_time et start_time. 0 pour les étapes en file d’attente.<br /><br /> Si total_elapsed_time dépasse la valeur maximale d’un entier, total_elapsed_time sera toujours la valeur maximale. Cette condition génère l’avertissement « la valeur maximale a été dépassée ».<br /><br /> La valeur maximale en millisecondes est équivalente à 24,8 jours.|  
|row_count|**bigint**|Nombre total de lignes modifiées ou retournées par cette demande.|Nombre de lignes affectées par l’étape.  Supérieur ou égal à zéro pour les étapes de l’opération de données.  -1 pour les étapes qui ne fonctionnent pas sur les données.|  
|estimated_rows|**bigint**|Nombre total de lignes de travail calculées lors de la compilation de la requête.|Nombre de lignes estimées par l’étape.  Supérieur ou égal à zéro pour les étapes de l’opération de données.  -1 pour les étapes qui ne fonctionnent pas sur les données.|  
|.|**nvarchar(4000)**|Contient le texte complet de la commande de cette étape.|Toute chaîne de demande valide pour une étape. NULL lorsque l’opération est du type MetaDataCreateOperation. Tronqué si plus de 4000 caractères.|  
  
 Pour plus d’informations sur le nombre maximal de lignes conservées par cette vue, consultez la section valeurs maximales de la vue système dans les valeurs minimale et maximale de la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] .  
  
## <a name="see-also"></a>Voir aussi  
 [Azure Synapse Analytics et les vues de gestion dynamique Parallel Data Warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
