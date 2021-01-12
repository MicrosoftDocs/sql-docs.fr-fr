---
description: sys.dm_tran_active_transactions (Transact-SQL)
title: sys.dm_tran_active_transactions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_tran_active_transactions
- sys.dm_tran_active_transactions_TSQL
- dm_tran_active_transactions_TSQL
- dm_tran_active_transactions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_active_transactions dynamic management view
ms.assetid: 154ad6ae-5455-4ed2-b014-e443abe2c6ee
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 91d4f683782b4b309d21b0604f19ba10ea34a416
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101442"
---
# <a name="sysdm_tran_active_transactions-transact-sql"></a>sys.dm_tran_active_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne des informations sur les transactions liées à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_tran_active_transactions**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|transaction_id|**bigint**|ID de la transaction au niveau de l'instance, et non au niveau de la base de données. Cet ID est unique pour toutes les bases de données d'une instance, mais pas pour toutes les instances du serveur.|  
|name|**nvarchar(32)**|Nom de la transaction. Ce nom est remplacé si la transaction est marquée et que le nom marqué remplace le nom de la transaction.|  
|transaction_begin_time|**datetime**|Heure de début de la transaction.|  
|transaction_type|**int**|Type de transaction.<br /><br /> 1 = transaction en lecture/écriture<br /><br /> 2 = transaction en lecture seule<br /><br /> 3 = transaction système<br /><br /> 4 = transaction distribuée|  
|transaction_uow|**uniqueidentifier**|Identificateur de l'unité de travail de la transaction pour les transactions distribuées. MS DTC utilise cet identificateur pour agir sur la transaction distribuée.|  
|transaction_state|**int**|0 = La transaction n'a pas encore été complètement initialisée.<br /><br /> 1 = La transaction a été initialisée mais n'a pas démarré.<br /><br /> 2 = La transaction est active.<br /><br /> 3 = La transaction est terminée. Cette valeur est utilisée pour les transactions en lecture seule.<br /><br /> 4 = Le processus de validation a été lancé sur la transaction distribuée. Cette valeur s'applique aux transactions distribuées uniquement. La transaction distribuée est toujours active, mais aucun traitement ultérieur ne peut avoir lieu.<br /><br /> 5 = La transaction est en état préparé et attend d'être résolue.<br /><br /> 6 = la transaction a été validée.<br /><br /> 7 = La transaction est en cours de restauration.<br /><br /> 8 = la transaction a été restaurée.|  
|transaction_status|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|transaction_status2|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|dtc_state|**int**|**S’applique à**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (version initiale jusqu’à la [version actuelle](/previous-versions/azure/ee336279(v=azure.100))).<br /><br /> 1 = ACTIF<br /><br /> 2 = PRÉPARÉ<br /><br /> 3 = VALIDÉ<br /><br /> 4 = ABANDONNÉ<br /><br /> 5 = RÉCUPÉRÉ|  
|dtc_status|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|dtc_isolation_level|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|filestream_transaction_id|**varbinary(128)**|**S’applique à**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (version initiale jusqu’à la [version actuelle](/previous-versions/azure/ee336279(v=azure.100))).<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|pdw_node_id|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_tran_session_transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-session-transactions-transact-sql.md)   
 [sys.dm_tran_database_transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
