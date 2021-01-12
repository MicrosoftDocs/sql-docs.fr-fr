---
description: sys.dm_db_xtp_transactions (Transact-SQL)
title: sys.dm_db_xtp_transactions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_xtp_transactions
- sys.dm_db_xtp_transactions_TSQL
- dm_db_xtp_transactions
- dm_db_xtp_transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_transactions dynamic management view
ms.assetid: 5c1a0a7a-e851-4b6f-8dfd-c9655fbf5a51
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d18ccebebcb4803f42c442b45844964b5c0c19d0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099847"
---
# <a name="sysdm_db_xtp_transactions-transact-sql"></a>sys.dm_db_xtp_transactions (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Enregistre les transactions actives dans le moteur de base de données de l'OLTP en mémoire.  
  
 Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
    
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|xtp_transaction_id|**bigint**|ID interne de cette transaction dans le gestionnaire de transactions XTP.|  
|transaction_id|**bigint**|ID de la transaction. Jointures avec l'ID de la transaction dans d'autres vues DMV associées à la transaction, telles que sys.dm_tran_active_transactions.<br /><br /> 0 pour les transactions XTP uniquement, telles que les transactions commencées par des procédures stockées compilées en mode natif.|  
|session_id|**smallint**|Identificateur de la session qui exécute cette transaction. Jointures avec sys.dm_exec_sessions.|  
|begin_tsn|**bigint**|Numéro de série du début de la transaction.|  
|end_tsn|**bigint**|Numéro de série de fin de la transaction.|  
|state|**int**|État de la transaction :<br /><br /> 0=ACTIVE<br /><br /> 1=COMMITTED<br /><br /> 2=ABORTED<br /><br /> 3=VALIDATING|  
|state_desc|**nvarchar**|Description de l'état de la transaction.|  
|result|**int**|Résultat de cette transaction. Les valeurs possibles sont les suivantes.<br /><br /> 0 - IN PROGRESS<br /><br /> 1 - SUCCESS<br /><br /> 2 - ERROR<br /><br /> 3 - COMMIT DEPENDENCY<br /><br /> 4 - VALIDATION FAILED (RR)<br /><br /> 5 - VALIDATION FAILED (SR)<br /><br /> 6 - ROLLBACK|  
|result_desc|**nvarchar**|Résultat de cette transaction. Les valeurs possibles sont les suivantes.<br /><br /> IN PROGRESS<br /><br /> SUCCESS<br /><br /> ERROR<br /><br /> COMMIT DEPENDENCY<br /><br /> VALIDATION FAILED (RR)<br /><br /> VALIDATION FAILED (SR)<br /><br /> ROLLBACK|  
|last_error|**int**|À usage interne uniquement|  
|is_speculative|**bit**|À usage interne uniquement|  
|is_prepared|**bit**|À usage interne uniquement|  
|is_delayed_durability|**bit**|À usage interne uniquement|  
|memory_address|**varbinary**|À usage interne uniquement|  
|database_address|**varbinary**|À usage interne uniquement|  
|thread_id|**int**|À usage interne uniquement|  
|read_set_row_count|**int**|À usage interne uniquement|  
|write_set_row_count|**int**|À usage interne uniquement|  
|scan_set_count|**int**|À usage interne uniquement|  
|savepoint_garbage_count|**int**|À usage interne uniquement|  
|log_bytes_required|**bigint**|À usage interne uniquement|  
|count_of_allocations|**int**|À usage interne uniquement|  
|allocated_bytes|**int**|À usage interne uniquement|  
|reserved_bytes|**int**|À usage interne uniquement|  
|commit_dependency_count|**int**|À usage interne uniquement|  
|commit_dependency_total_attempt_count|**int**|À usage interne uniquement|  
|scan_area|**int**|À usage interne uniquement|  
|scan_area_desc|**nvarchar**|À usage interne uniquement|  
|scan_location|**int**|À usage interne uniquement|  
|dependent_1_address|**varbinary (8)**|À usage interne uniquement|  
|dependent_2_address|**varbinary (8)**|À usage interne uniquement|  
|dependent_3_address|**varbinary (8)**|À usage interne uniquement|  
|dependent_4_address|**varbinary (8)**|À usage interne uniquement|  
|dependent_5_address|**varbinary (8)**|À usage interne uniquement|  
|dependent_6_address|**varbinary (8)**|À usage interne uniquement|  
|dependent_7_address|**varbinary (8)**|À usage interne uniquement|  
|dependent_8_address|**varbinary (8)**|À usage interne uniquement|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW DATABASE STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des tables à mémoire optimisée &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
