---
title: sys.dm_pdw_nodes_exec_sql_text (Transact-SQL) | Microsoft Docs
description: Vue de gestion dynamique qui retourne le texte du lot SQL identifié par le sql_handle spécifié.
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
ms.openlocfilehash: 4e66c08b7e4a4a179a53c853e2802759f0627471
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99140481"
---
# <a name="syspdw_nodes_dm_exec_sql_text-transact-sql"></a>sys.pdw_nodes_dm_exec_sql_text (Transact-SQL)
[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Retourne le texte du lot SQL identifié par le *sql_handle* spécifié. Cette fonction à valeur de table remplace la fonction système **fn_get_sql**.  
   
## <a name="table-returned"></a>Table retournée  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**pdw_node_id**|**int**|ID numérique unique associé au nœud.|
|**dbid**|**smallint**|ID de la base de données.<br /><br /> Pour les instructions SQL non planifiées et préparées, ID de la base de données dans laquelle les instructions ont été compilées.|  
|**objectid**|**int**|ID de l'objet.<br /><br /> Est NULL pour les instructions SQL ad hoc et préparées.|  
|**number**|**smallint**|Pour une procédure stockée numérotée, cette colonne retourne le numéro de la procédure stockée. Pour plus d’informations, consultez [sys.numbered_procedures &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-numbered-procedures-transact-sql.md).<br /><br /> Est NULL pour les instructions SQL ad hoc et préparées.|  
|**chiffrées**|**bit**|1 : le texte SQL est chiffré.<br /><br /> 0 : le texte SQL n’est pas chiffré.|  
|**text**|**nvarchar(max)**|Texte de la requête SQL.<br /><br /> NULL pour les objets chiffrés.|  

## <a name="remarks"></a>Notes  
Les mêmes remarques dans [sys.dm_exec_sql_text](./sys-dm-exec-sql-text-transact-sql.md) s’appliquent.  
  
## <a name="permissions"></a>Autorisations  
 Exiger un rôle de serveur **sysadmin** ou une `VIEW SERVER STATE` autorisation sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Azure Synapse Analytics et les vues de gestion dynamique Parallel Data Warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  

  ## <a name="next-steps"></a>Étapes suivantes
 Pour obtenir d’autres conseils de développement, consultez [vue d’ensemble du développement Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-overview-develop).