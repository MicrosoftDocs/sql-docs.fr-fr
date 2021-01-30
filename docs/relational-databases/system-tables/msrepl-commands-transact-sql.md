---
description: MSrepl_commands (Transact-SQL)
title: MSrepl_commands (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSrepl_commands
- MSrepl_commands_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSrepl_commands system table
ms.assetid: 53b9f9cd-9429-47a0-aba2-908fc60e7036
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9a64f06d234c5fe9bd037ad1a3aa1d142e688fd7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188739"
---
# <a name="msrepl_commands-transact-sql"></a>MSrepl_commands (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSrepl_commands** contient des lignes de commandes répliquées. Cette table est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publisher_database_id**|**int**|Identificateur de la base de données du serveur de publication.|  
|**xact_seqno**|**varbinary(16)**|Numéro de séquence de transaction.|  
|**type**|**int**|Type de commande.|  
|**article_id**|**int**|ID de l’article.|  
|**originator_id**|**int**|ID du point de départ.|  
|**command_id**|**int**|ID de la commande.|  
|**partial_command**|**bit**|Indique s'il s'agit d'une commande partielle|  
|**command**|**varbinary (1024)**|La valeur de commande.|  
|**hashkey**|**int**|À usage interne uniquement.|  
|**originator_lsn**|**varbinary(16)**|Identifie le LSN de la commande dans la publication d'origine. Utilisé dans la réplication transactionnelle d'égal à égal.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)  
  
  
