---
description: MSrepl_transactions (Transact-SQL)
title: MSrepl_transactions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSrepl_transactions_TSQL
- MSrepl_transactions
dev_langs:
- TSQL
helpviewer_keywords:
- MSrepl_transactions system table
ms.assetid: d325288d-47ae-4488-8799-122f7ab43459
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6e9ca4af77c9cab4e64a515041e68e4eb3dae3b6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092409"
---
# <a name="msrepl_transactions-transact-sql"></a>MSrepl_transactions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSrepl_transactions** contient une ligne pour chaque transaction répliquée. Cette table est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publisher_database_id**|**int**|Identificateur de la base de données du serveur de publication.|  
|**xact_id**|**varbinary(16)**|ID de la transaction.|  
|**xact_seqno**|**varbinary(16)**|Numéro de séquence de la transaction.|  
|**entry_time**|**datetime**|Heure d'entrée de la transaction dans la base de données de distribution.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
