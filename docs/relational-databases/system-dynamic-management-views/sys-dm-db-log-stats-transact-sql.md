---
description: sys.dm_db_log_stats (Transact-SQL)
title: sys.dm_db_log_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_log_stats_TSQL
- sys.dm_db_log_stats
- sys.dm_db_log_stats_TSQL
- dm_db_log_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_log_stats dynamic management function
ms.assetid: ''
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ddfd608b3f2d5bb5c4c259dfb0ec822f245f72b3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096560"
---
# <a name="sysdm_db_log_stats-transact-sql"></a>sys.dm_db_log_stats (Transact-SQL)   
[!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)]

Retourne des informations et des attributs de niveau récapitulatif sur les fichiers journaux des transactions des bases de données. Utilisez ces informations pour la surveillance et le diagnostic de l’intégrité du journal des transactions.   
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
 sys.dm_db_log_stats ( database_id )
```  
  
## <a name="arguments"></a>Arguments  

*database_id* | NULL | **Par défaut**

ID de la base de données. `database_id` a la valeur `int`. Les entrées valides sont le numéro d’identification d’une base de données, `NULL` ou `DEFAULT` . La valeur par défaut est `NULL`. `NULL` et `DEFAULT` sont des valeurs équivalentes dans le contexte de la base de données active.  
Vous pouvez spécifier la fonction intégrée [DB_ID](../../t-sql/functions/db-id-transact-sql.md). Lorsque vous utilisez `DB_ID` sans spécifier de nom de base de données, le niveau de compatibilité de la base de données actuelle doit être supérieur ou égal à 90.

  
## <a name="tables-returned"></a>Tables retournées  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|database_id    |**int**    |ID de base de données |  
|recovery_model |**nvarchar(60)**   |   Mode de récupération de la base de données. Les valeurs possibles incluent : <br /> SIMPLE<br /> BULK_LOGGED <br /> FULL |  
|log_min_lsn    |**nvarchar(24)**   |   [Numéro séquentiel](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) dans le journal de démarrage actuel dans le journal des transactions.|  
|log_end_lsn    |**nvarchar(24)**   |   [numéro séquentiel dans le journal (LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) du dernier enregistrement du journal des transactions.|  
|current_vlf_sequence_number    |**bigint** |   Numéro de séquence actuel du [fichier journal virtuel (fichier journal virtuel)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) au moment de l’exécution.|  
|current_vlf_size_mb    |**float**  |   Taille actuelle du [fichier journal virtuel (fichier journal virtuel)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) en Mo.|   
|total_vlf_count    |**bigint** |   Nombre total de [fichiers journaux virtuels (fichiers journaux virtuels)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) dans le journal des transactions. |  
|total_log_size_mb  |**float**  |   Taille totale du journal des transactions en Mo. |  
|active_vlf_count   |**bigint** |   Nombre total de [fichiers journaux virtuels actifs (fichiers journaux virtuels)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) dans le journal des transactions.|  
|active_log_size_mb |**float**  |   Taille totale du journal des transactions actives en Mo.|  
|log_truncation_holdup_reason   |**nvarchar(60)**   |   Raison de la troncation du journal retard. La valeur est identique à celle  `log_reuse_wait_desc` de la colonne de `sys.databases` .  (Pour obtenir des explications plus détaillées sur ces valeurs, consultez [le journal des transactions](../../relational-databases/logs/the-transaction-log-sql-server.md)). <br />Les valeurs possibles incluent : <br />NOTHING<br />CHECKPOINT<br />LOG_BACKUP<br />ACTIVE_BACKUP_OR_RESTORE<br />ACTIVE_TRANSACTION<br />DATABASE_MIRRORING<br />RÉPLICATION<br />DATABASE_SNAPSHOT_CREATION<br />LOG_SCAN<br />AVAILABILITY_REPLICA<br />OLDEST_PAGE<br />XTP_CHECKPOINT<br />AUTRE TEMPORAIRE |  
|log_backup_time    |**datetime**   |   Heure de la dernière sauvegarde du journal des transactions.|   
|log_backup_lsn |**nvarchar(24)**   |   Numéro séquentiel dans le journal de la dernière sauvegarde du journal des transactions [(LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch).|   
|log_since_last_log_backup_mb   |**float**  |   Taille du journal en Mo depuis le dernier numéro séquentiel dans le journal de sauvegarde du journal des transactions [(LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch).|  
|log_checkpoint_lsn |**nvarchar(24)**   |   Numéro séquentiel dans le journal du dernier point de contrôle [(LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch).|  
|log_since_last_checkpoint_mb   |**float**  |   Taille du journal en Mo depuis le dernier numéro séquentiel dans le journal de point de contrôle [(LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch).|  
|log_recovery_lsn   |**nvarchar(24)**   |   Numéro séquentiel dans le journal de récupération [(LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch) de la base de données. Si `log_recovery_lsn` se produit avant le LSN de point de contrôle, `log_recovery_lsn` est le LSN de la transaction active la plus ancienne, sinon `log_recovery_lsn` est le LSN de point de contrôle.|  
|log_recovery_size_mb   |**float**  |   Taille du journal en Mo depuis le numéro séquentiel dans le journal de récupération du journal [(LSN)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Logical_Arch).|  
|recovery_vlf_count |**bigint** |   Nombre total de [fichiers journaux virtuels (fichiers journaux virtuels)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) à récupérer, en cas de basculement ou de redémarrage du serveur. |  


## <a name="remarks"></a>Notes
En cas d’exécution `sys.dm_db_log_stats` sur une base de données qui participe à un groupe de disponibilité en tant que réplica secondaire, seul un sous-ensemble des champs décrits ci-dessus est renvoyé.  Actuellement, seuls `database_id` , `recovery_model` et `log_backup_time` sont retournés lorsqu’ils sont exécutés sur une base de données secondaire.   

## <a name="permissions"></a>Autorisations  
Nécessite l' `VIEW DATABASE STATE` autorisation dans la base de données.   
  
## <a name="examples"></a>Exemples  

### <a name="a-determining-databases-in-a-ssnoversion-instance-with-high-number-of-vlfs"></a>R. Détermination des bases de données dans une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance avec un nombre élevé de fichiers journaux virtuels   
La requête suivante retourne les bases de données contenant plus de 100 fichiers journaux virtuels dans les fichiers journaux. Un grand nombre d’fichiers journaux virtuels peut affecter le temps de démarrage, de restauration et de récupération de la base de données.

```sql  
SELECT name AS 'Database Name', total_vlf_count AS 'VLF count' 
FROM sys.databases AS s
CROSS APPLY sys.dm_db_log_stats(s.database_id) 
WHERE total_vlf_count  > 100;
```   

### <a name="b-determining-databases-in-a-ssnoversion-instance-with-transaction-log-backups-older-than-4-hours"></a>B. Détermination des bases de données dans une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance avec des sauvegardes du journal des transactions datant de plus de 4 heures   
La requête suivante détermine les heures de la dernière sauvegarde du journal pour les bases de données de l’instance.

```sql  
SELECT name AS 'Database Name', log_backup_time AS 'last log backup time' 
FROM sys.databases AS s
CROSS APPLY sys.dm_db_log_stats(s.database_id); 
```

## <a name="see-also"></a>Voir aussi  
[Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[Vues de gestion dynamique liées à la base de données &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)   
[sys.dm_db_log_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-info-transact-sql.md)    
  
