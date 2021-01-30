---
title: sys.database_automatic_tuning_options (Transact-SQL) | Microsoft Docs
description: Découvrez comment afficher les options de réglage automatique sur un SQL Database. Consultez autorisations requises et afficher les ressources disponibles supplémentaires.
ms.custom: ''
ms.date: 07/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- database_automatic_tuning_options_tsql
- database_automatic_tuning_options
- sys.database_automatic_tuning_options_tsql
- sys.database_automatic_tuning_options
dev_langs:
- TSQL
helpviewer_keywords:
- database_automatic_tuning_options catalog view
- sys.database_automatic_tuning_options catalog view
ms.assetid: 16b47d55-8019-41ff-ad34-1e0112178067
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0dbedd65c5f98016e4e001fd863845079da68578
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210374"
---
# <a name="sysdatabase_automatic_tuning_options-transact-sql"></a>sys. Database \_ \_ tuning_options automatique (Transact-SQL)
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  Retourne les options de réglage automatique pour cette base de données.  

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**nvarchar(128)**|Nom de l’option d’optimisation automatique. Reportez-vous à la rubrique [ALTER DATABASE SET AUTOMATIC_TUNING &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) pour obtenir les options disponibles.|  
|**desired_state**|**smallint**|Indique le mode d’opération souhaité pour l’option de réglage automatique, défini explicitement par l’utilisateur.<br />0 = désactivé<br />1 = activé|  
|**desired_state_desc**|**nvarchar(60)**|Description textuelle du mode d’opération souhaité de l’option d’optimisation automatique.<br />OFF<br />ACTIVÉ|  
|**actual_state**|**smallint**|Indique le mode d’opération de l’option de réglage automatique.<br />0 = désactivé<br />1 = activé|  
|**actual_state_desc**|**nvarchar(60)**|Description textuelle du mode d’opération réel de l’option d’optimisation automatique.<br />OFF<br />ACTIVÉ|  
|**reason**|**smallint**|Indique pourquoi les États réels et souhaités sont différents.<br />2 = DÉSACTIVÉ<br />11 = QUERY_STORE_OFF<br />12 = QUERY_STORE_READ_ONLY<br />13 = NOT_SUPPORTED|   
|**reason_desc**|**nvarchar(60)**|Description textuelle de la raison pour laquelle les États réels et souhaités sont différents.<br />Disabled = l’option est désactivée par le système<br />QUERY_STORE_OFF = Magasin des requêtes est désactivé<br />QUERY_STORE_READ_ONLY = Magasin des requêtes est en mode lecture seule<br />NOT_SUPPORTED = disponible uniquement dans l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] édition Enterprise| 
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation `VIEW DATABASE STATE`.  
  
## <a name="see-also"></a>Voir aussi  
 [Réglage automatique](../../relational-databases/automatic-tuning/automatic-tuning.md)   
 [ALTER DATABASE SET AUTOMATIC_TUNING &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.dm_db_tuning_recommendations &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md)   
 
