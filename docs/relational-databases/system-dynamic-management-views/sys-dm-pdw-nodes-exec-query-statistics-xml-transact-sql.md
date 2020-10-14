---
title: sys.dm_pdw_nodes_exec_query_statistics_xml (Transact-SQL) | Microsoft Docs
description: Vue de gestion dynamique qui retourne le plan d’exécution de requête pour les demandes en cours de vol. Utilisez cette DMV pour récupérer Showplan XML avec des statistiques temporaires.
ms.custom: ''
ms.date: 10/14/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: ''
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: ba671ad6d1d9c39e6b3b0dcaa2dd422d526ccaa0
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92035302"
---
# <a name="dm_pdw_nodes_exec_query_statistics_xml-transact-sql"></a>dm_pdw_nodes_exec_query_statistics_xml (Transact-SQL)
[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Retourne le plan d’exécution de requête pour les demandes en cours. Utilisez cette DMV pour récupérer Showplan XML avec des statistiques temporaires.

## <a name="table-returned"></a>Table retournée

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|
|node_id|**int**|ID numérique unique associé au nœud.|
|session_id|**smallint**|ID de la session. N'accepte pas la valeur NULL.|
|request_id|**int**|ID de la demande. N'accepte pas la valeur NULL.|
|sql_handle|**varbinary(64)**|Jeton qui identifie de façon unique le lot ou la procédure stockée dont fait partie la requête. Autorise la valeur Null.|
|plan_handle|**varbinary(64)**|Jeton qui identifie de façon unique un plan d’exécution de requête pour un lot en cours d’exécution. Autorise la valeur Null.|
|query_plan|**xml**|Contient la représentation Showplan du runtime du plan d’exécution de requête spécifié avec *plan_handle* contenant des statistiques partielles. Le plan d'exécution de requêtes est au format XML. Un plan est généré pour chaque traitement contenant par exemple des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc, des appels de procédures stockées et des appels de fonctions définies par l'utilisateur. Autorise la valeur Null.|

## <a name="remarks"></a>Notes
Les mêmes remarques dans [sys.dm_exec_query_statistics_xml](./sys-dm-exec-query-statistics-xml-transact-sql.md?view=sql-server-ver15) s’appliquent.   

## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation `VIEW SERVER STATE` sur le serveur.  

## <a name="see-also"></a>Voir aussi  
 [Azure Synapse Analytics et les vues de gestion dynamique Parallel Data Warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  

 ## <a name="next-steps"></a>Étapes suivantes
 Pour obtenir des conseils supplémentaires en matière de développement, consultez l’article [Vue d’ensemble sur le développement SQL Data Warehouse](/azure/sql-data-warehouse/sql-data-warehouse-overview-develop).