---
description: sys.dm_tran_session_transactions (Transact-SQL)
title: sys.dm_tran_session_transactions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_tran_session_transactions
- sys.dm_tran_session_transactions
- sys.dm_tran_session_transactions_TSQL
- dm_tran_session_transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_session_transactions dynamic management view
ms.assetid: c7157491-58c2-49fe-87d7-0c9723113adf
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4cb45465c0c4d094973a8e60d324dec17d2abbc5
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839323"
---
# <a name="sysdm_tran_session_transactions-transact-sql"></a>sys.dm_tran_session_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie les informations de corrélation des transactions et des sessions associées.  
  
> [!NOTE]  
>  Pour appeler cette valeur à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilisez le nom **sys.dm_pdw_nodes_tran_session_transactions**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|session_id|**int**|ID de la session dans laquelle la transaction est en cours d'exécution.|  
|transaction_id|**bigint**|ID de la transaction.|  
|transaction_descriptor|**Binary(8**|ID de la transaction utilisé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors de la communication avec le pilote du client.|  
|enlist_count|**int**|Nombre de requêtes actives dans la session qui effectue la transaction.|  
|is_user_transaction|**bit**|1 = La transaction a été lancée par une requête utilisateur.<br /><br /> 0 = Transaction système.|  
|is_local|**bit**|1 = Transaction locale.<br /><br /> 0 = Transaction distribuée ou transaction sur une session liée par une inscription.|  
|is_enlisted|**bit**|1 = Transaction distribuée inscrite.<br /><br /> 0 = Transaction distribuée non inscrite.|  
|is_bound|**bit**|1 = La transaction est active sur la session par l'intermédiaire de sessions liées.<br /><br /> 0 = La transaction n'est pas active sur la session par l'intermédiaire de sessions liées.|  
|open_transaction_count||Nombre de transactions ouvertes pour chaque session.|  
|pdw_node_id|**int**|**S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificateur du nœud sur lequel cette distribution se trouve.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   

## <a name="remarks"></a>Notes  
 Par l'intermédiaire de sessions liées et distribuées, il est possible à une transaction de s'exécuter dans plusieurs sessions. Dans ces cas, les transactions sys.dm_tran_session_transactions affichent plusieurs lignes pour le même identificateur transaction_id : une pour chaque session dans laquelle la transaction s'exécute.  
  
 En exécutant plusieurs requêtes en mode validation automatique utilisant plusieurs ensembles de résultats actifs (MARS), il est possible d'avoir plusieurs transactions actives sur une seule session. Dans ces cas, les transactions sys.dm_tran_session_transactions affichent plusieurs lignes pour le même identificateur session_id : une pour chaque transaction qui s'exécute dans cette session.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
