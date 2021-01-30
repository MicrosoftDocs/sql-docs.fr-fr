---
title: sys.dm_pdw_nodes_exec_query_plan (Transact-SQL) | Microsoft Docs
description: Vue de gestion dynamique qui retourne le Showplan au format XML pour le lot spécifié par le descripteur de plan. Le plan spécifié par le descripteur de plan peut être en cache ou en cours d'exécution.
ms.custom: ''
ms.date: 10/14/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: ''
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: f78f40922db54563206bfe4c9fb4966958b41840
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99141197"
---
# <a name="syspdw_nodes_dm_exec_query_plan-transact-sql"></a>sys.pdw_nodes_dm_exec_query_plan (Transact-SQL)
[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Retourne le plan d'exécution de requêtes au format XML pour le traitement spécifié par le descripteur de plan. Le plan spécifié par le descripteur de plan peut être en cache ou en cours d'exécution.  

> [!note] 
> Dans Synapse SQL, l’ajout d’espaces blancs dans une requête constitue une modification de requête qui permet de recalculer le hachage de la requête et de ne pas réutiliser le plan d’exécution mis en cache précédent.


## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**pdw_node_id**|**int**|ID numérique unique associé au nœud.| 
|**dbid**|**smallint**|ID de la base de données de contexte qui était en fonction lorsque l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] correspondant à ce plan a été compilée. Pour les instructions SQL non planifiées et préparées, ID de la base de données dans laquelle les instructions ont été compilées.<br /><br /> Colonne acceptant la valeur NULL.|  
|**objectid**|**int**|ID de l'objet (par exemple, procédure stockée ou fonction définie par l'utilisateur) pour ce plan de requête. Pour les traitements ad hoc et préparés, cette colonne est **null**.<br /><br /> Colonne acceptant la valeur NULL.|  
|**number**|**smallint**|Entier servant à la numérotation des procédures stockées. Pour les traitements ad hoc et préparés, cette colonne est **null**.<br /><br /> Colonne acceptant la valeur NULL.| 
|**chiffrées**|**bit**|Indique si la procédure stockée correspondante est chiffrée.<br /><br /> 0 = Non chiffrée.<br /><br /> 1 = Chiffrée.<br /><br /> Colonne n'acceptant pas la valeur NULL.|  
|**query_plan**|**xml**|Contient la représentation Showplan au moment de la compilation du plan d’exécution de requête spécifié avec *plan_handle*. Le plan d'exécution de requêtes est au format XML. Un plan est généré pour chaque traitement contenant par exemple des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc, des appels de procédures stockées et des appels de fonctions définies par l'utilisateur.<br /><br /> Colonne acceptant la valeur NULL.|  
  
## <a name="remarks"></a>Notes  
Les mêmes remarques dans [sys.dm_exec_query_plan](./sys-dm-exec-query-plan-transact-sql.md) s’appliquent.  
  
## <a name="permissions"></a>Autorisations  
 Exiger un rôle de serveur **sysadmin** ou une `VIEW SERVER STATE` autorisation sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Azure Synapse Analytics et les vues de gestion dynamique Parallel Data Warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  

 ## <a name="next-steps"></a>Étapes suivantes
 Pour obtenir d’autres conseils de développement, consultez [vue d’ensemble du développement Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-overview-develop).
