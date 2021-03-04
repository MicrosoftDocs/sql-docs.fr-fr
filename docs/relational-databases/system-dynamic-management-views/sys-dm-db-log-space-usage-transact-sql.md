---
description: sys.dm_db_log_space_usage (Transact-SQL)
title: sys.dm_db_log_space_usage (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_db_log_space_usage
- sys.dm_db_log_space_usage_TSQL
- dm_db_log_space_usage
- dm_db_log_space_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_log_space_usage dynamic management view
ms.assetid: f6b40060-c17d-472f-b0a3-3b350275d487
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9057a03c6a747184c8a4f884e9bd73404726cf41
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839379"
---
# <a name="sysdm_db_log_space_usage-transact-sql"></a>sys.dm_db_log_space_usage (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Retourne des informations sur l’utilisation de l’espace pour le journal des transactions. 
  
> [!NOTE]
> Tous les fichiers journaux de transactions sont combinés.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|database_id|**smallint**|ID de la base de données.|  
|total_log_size_in_bytes |**bigint** |Taille du journal  |
|used_log_space_in_bytes |**bigint** |Taille occupée du journal  |     
|used_log_space_in_percent |**real** |Taille occupée du journal sous la forme d’un pourcentage de la taille totale du journal |
|log_space_in_bytes_since_last_backup |**bigint** |Quantité d’espace utilisée depuis la dernière sauvegarde du journal <br />**S’applique à :** [!INCLUDE[sssql14-md](../../includes/sssql14-md.md)] via [!INCLUDE[sscurrent-md](../../includes/ssnoversion-md.md)] ,  [!INCLUDE[ssSDS](../../includes/sssds-md.md)] .|
    
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le compte d' [administrateur de serveur](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) ou le compte d' [administrateur Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   
  
## <a name="examples"></a>Exemples  
  
### <a name="a-determine-the-amount-of-free-log-space-in-tempdb"></a>R. Déterminer la quantité d’espace de journalisation libre dans tempdb   
La requête suivante retourne l’espace total libre du journal, en mégaoctets (Mo), disponible dans tempdb.

```sql
USE tempdb;  
GO  

SELECT (total_log_size_in_bytes - used_log_space_in_bytes)*1.0/1024/1024 AS [free log space in MB]  
FROM sys.dm_db_log_space_usage;  
```
  
## <a name="see-also"></a>Voir aussi  
[Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[Vues de gestion dynamique liées à la base de données &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_db_file_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-file-space-usage-transact-sql.md)    
[sys.dm_db_task_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md)   
[sys.dm_db_session_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)  
[sys.dm_db_log_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-info-transact-sql.md)    
[sys.dm_db_log_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-stats-transact-sql.md)