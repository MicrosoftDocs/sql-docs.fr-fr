---
description: sys.dm_pdw_exec_sessions (Transact-SQL)
title: sys.dm_pdw_exec_sessions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/22/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 31c262b3-7e4d-44c4-af71-aaef0fd1a980
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: d155f836abb975b39ef7b2396057a43e59686f9b
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92035337"
---
# <a name="sysdm_pdw_exec_sessions-transact-sql"></a>sys.dm_pdw_exec_sessions (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contient des informations sur toutes les sessions actuellement ou récemment ouvertes sur l’appliance. Elle répertorie une ligne par session.  
  
|Nom de la colonne|Type de données|Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|session_id|**nvarchar(32)**|ID de la requête actuelle ou de la dernière exécution de la requête (si la session est ARRÊTée et que la requête a été exécutée au moment de l’arrêt). Clé pour cette vue.|Unique pour toutes les sessions dans le système.|  
|status|**nvarchar(10**|Pour les sessions actives, indique si la session est actuellement active ou inactive. Pour les sessions passées, l’état de la session peut indiquer fermé ou arrêté (si la session a été fermée de force).|« ACTIVE », « CLOSED », « IDLE », « TERMINATEED »|  
|request_id|**nvarchar(32)**|ID de la requête actuelle ou de la dernière exécution de la requête.|Unique pour toutes les demandes dans le système. NULL si aucun n’a été exécuté.|  
|security_id|**varbinary(85)**|ID de sécurité du principal qui exécute la session.||  
|login_name|**nvarchar(128)**|Nom de connexion du principal qui exécute la session.|Toute chaîne conforme aux conventions d’affectation de noms d’utilisateurs.|  
|login_time|**datetime**|Date et heure de la connexion de l’utilisateur et de la création de cette session.|**DateTime** valide avant l’heure actuelle.|  
|query_count|**int**|Capture le nombre de requêtes/la session requeststhis a été exécuté depuis sa création.|Supérieur ou égal à 0.|  
|is_transactional|**bit**|Capture si une session est actuellement dans une transaction ou non.|0 pour la validation automatique, 1 pour transaction.|  
|client_id|**nvarchar(255)**|Capture les informations client pour la session.|Toute chaîne valide.|  
|app_name|**nvarchar(255)**|Capture les informations de nom de l’application, éventuellement définies dans le cadre du processus de connexion.|Toute chaîne valide.|  
|sql_spid|**int**|Numéro d’identification du SPID. Utilisez `session_id` cette session. Utilisez la `sql_spid` colonne pour joindre à **sys.dm_pdw_nodes_exec_sessions**.<br /><br /> AVERTISSEMENT Cette colonne contient des SPID fermés. ** \* \* \* \* **||  
  
 Pour plus d’informations sur le nombre maximal de lignes conservées par cette vue, consultez la section métadonnées dans la rubrique [limites de capacité](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) .  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation `VIEW SERVER STATE`.  
  
## <a name="see-also"></a>Voir aussi  
 [Azure Synapse Analytics et les vues de gestion dynamique Parallel Data Warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
