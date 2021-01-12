---
description: sys.dm_tran_database_transactions (Transact-SQL)
title: sys.dm_tran_database_transactions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_tran_database_transactions
- sys.dm_tran_database_transactions_TSQL
- dm_tran_database_transactions_TSQL
- sys.dm_tran_database_transactions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_database_transactions dynamic management view
ms.assetid: 82a44295-4cbc-4a5b-891a-8ebaf307b8f5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a9932a3f57c02307699ff75d53efaf81f25594e3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097491"
---
# <a name="sysdm_tran_database_transactions-transact-sql"></a>sys.dm_tran_database_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne les informations concernant les transactions au niveau de la base de données.  
  
> [!NOTE]  
>  Pour appeler cette DMV à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_tran_database_transactions**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|transaction_id|**bigint**|ID de la transaction au niveau de l'instance, et non au niveau de la base de données. Il n'est unique que dans les bases de données d'une instance, pas dans toutes les instances du serveur.|  
|database_id|**int**|ID de la base de données associée à la transaction.|  
|database_transaction_begin_time|**datetime**|Heure à laquelle la base de données a été impliquée dans la transaction. Il s'agit plus précisément de l'heure du premier enregistrement de journal dans la base de données pour la transaction.|  
|database_transaction_type|**int**|1 = transaction en lecture/écriture<br /><br /> 2 = transaction en lecture seule<br /><br /> 3 = transaction système|  
|database_transaction_state|**int**|1 = la transaction n'a pas été initialisée.<br /><br /> 3 = la transaction a été initialisée, mais n'a produit aucun enregistrement de journal.<br /><br /> 4 = la transaction a produit des enregistrements de journal.<br /><br /> 5 = la transaction a été préparée.<br /><br /> 10 = la transaction a été validée.<br /><br /> 11 = la transaction a été restaurée.<br /><br /> 12 = la transaction est en cours de validation. (L’enregistrement de journal est en cours de génération, mais n’a pas été matérialisé ou rendu persistant.)|  
|database_transaction_status|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|database_transaction_status2|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|database_transaction_log_record_count|**bigint**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Nombre d'enregistrements de journal produits dans la base de données pour la transaction.|  
|database_transaction_replicate_record_count|**int**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Nombre d’enregistrements de journal générés dans la base de données pour la transaction répliquée.|  
|database_transaction_log_bytes_used|**bigint**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Nombre d'octets utilisés jusqu'alors dans la base de données pour la transaction.|  
|database_transaction_log_bytes_reserved|**bigint**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Nombre d'octets réservés à l'utilisation du journal de la base de données pour la transaction.|  
|database_transaction_log_bytes_used_system|**int**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Nombre d'octets utilisés jusqu'alors dans le journal de la base de données pour les transactions système pour le compte de la transaction.|  
|database_transaction_log_bytes_reserved_system|**int**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Nombre d'octets réservés à l'utilisation du journal de la base de données pour les transactions système pour le compte de la transaction.|  
|database_transaction_begin_lsn|**numeric(25,0)**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Numéro séquentiel dans le journal (NSE) du premier enregistrement concernant la transaction dans le journal de la base de données.|  
|database_transaction_last_lsn|**numeric(25,0)**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Numéro de séquence d'enregistrement (NSE) de l'enregistrement le plus récent concernant la transaction dans le journal de la base de données.|  
|database_transaction_most_recent_savepoint_lsn|**numeric(25,0)**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Numéro de séquence d'enregistrement du plus récent point d'enregistrement pour la transaction dans le journal de la base de données.|  
|database_transaction_commit_lsn|**numeric(25,0)**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Numéro de séquence d'enregistrement (NSE) de l'enregistrement du journal de validation concernant la transaction dans le journal de la base de données.|  
|database_transaction_last_rollback_lsn|**numeric(25,0)**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Numéro de séquence d'enregistrement utilisé la dernière fois pour une restauration. Si aucune restauration n’a eu lieu, la valeur est MaxLSN.|  
|database_transaction_next_undo_lsn|**numeric(25,0)**|**S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.<br /><br /> Numéro de séquence d'enregistrement du prochain enregistrement à annuler.|  
|pdw_node_id|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   

## <a name="see-also"></a>Voir aussi  
 [sys.dm_tran_active_transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-transactions-transact-sql.md)   
 [sys.dm_tran_session_transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-session-transactions-transact-sql.md)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


