---
description: managed_backup.fn_available_backups managed_backup (Transact-SQL)
title: managed_backup managed_backup.fn_available_backups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- smart_admin.fn_available_backups
- smart_admin.fn_available_backups_TSQL
- fn_available_backups_TSQL
- fn_available_backups
dev_langs:
- TSQL
helpviewer_keywords:
- fn_available_backups
- smart_admin.fn_available_backups
ms.assetid: 7aa84474-16e5-49bd-a703-c8d1408ef107
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: e64cba484413a0d5ebc1a8365d0e843c49ba7692
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207438"
---
# <a name="managed_backupfn_available_backups-transact-sql"></a>managed_backup.fn_available_backups managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Retourne une table de 0, une ou plusieurs lignes de fichiers de sauvegarde disponibles pour la base de données spécifiée. Les fichiers de sauvegarde retournés sont créés par la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
managed_backup.fn_available_backups ([@database_name = ] 'database name')  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Arguments  
 @database_name  
 Nom de la base de données. Est de type @database_name nvarchar (512).  
  
## <a name="table-returned"></a>Table retournée  
 La table a une contrainte cluster unique sur (database_guid, backup_start_date et first_lsn, backup_type).   
Si une base de données est supprimée, puis recréée, les jeux de sauvegarde de toutes les bases de données sont retournés. La sortie est classée par database_guid, qui identifie de façon unique chaque base de données.   
S'il existe des ruptures dans la séquence des LSN, indiquant qu'il existe une rupture dans la séquence de journaux de transactions consécutifs, la table contiendra une ligne spéciale pour chaque segment LSN manquant.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|Backup_path|NVARCHAR(260) COLLATE Latin1_General_CI_AS_KS_WS|URL du fichier de sauvegarde.|  
|backup_type|NVARCHAR (6)|« DB » pour la sauvegarde de base de données « LOG » pour la sauvegarde de fichier journal|  
|expiration_date|DATETIME|Date à laquelle ce fichier doit être supprimé. Elle est déterminée en fonction de la capacité à récupérer la base de données à un point précis dans le temps au sein de la période de rétention spécifiée.|  
|database_guid|UNIQUEIDENTIFIER|Valeur GUID pour la base de données spécifiée.  Le GUID identifie de manière unique une base de données.|  
|first_lsn|NUMERIC(25, 0)|Numéro séquentiel dans le journal correspondant au premier enregistrement ou à l'enregistrement le plus ancien du journal dans le jeu de sauvegardes Sa valeur peut être NULL.|  
|last_lsn|NUMERIC(25, 0)|Numéro séquentiel dans le journal correspondant à l'enregistrement du journal suivant après le jeu de sauvegarde. Sa valeur peut être NULL.|  
|backup_start_date|DATETIME|Date et heure de début de la sauvegarde|  
|backup_finish_date|NVARCHAR(128)|Date et heure de fin de la sauvegarde|  
|machine_name|NVARCHAR(128)|Nom de l'ordinateur où l'instance de SQL Server est installée et exécute la [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|last_recovery_fork_id|UNIQUEIDENTIFIER|Numéro d'identification du branchement de récupération de fin.|  
|first_recovery_fork_id|UNIQUEIDENTIFIER|ID du branchement de récupération de début. Pour les sauvegardes de données, first_recovery_fork_guid équivaut à last_recovery_fork_guid.|  
|fork_point_lsn|NUMERIC(25, 0)|Si first_recovery_fork_id n'équivaut pas à last_recovery_fork_id, il s'agit du numéro séquentiel dans le journal du point du branchement. Dans les autres cas, cette valeur est NULL.|  
|availability_group_guid|UNIQUEIDENTIFIER|Si une base de données est une base de données Always On, il s’agit du GUID du groupe de disponibilité. Sinon, cette valeur est NULL.|  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (succès) ou 1 (échec).  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert des autorisations **Select** sur cette fonction.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant répertorie toutes les sauvegardes disponibles sauvegardées via [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] pour la base de données « MyDB ».  
  
```  
SELECT *   
FROM msdb.managed_backup.fn_available_backups ('MyDB')  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server de la sauvegarde managée sur Microsoft Azure](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)   
 [Restauration à partir de sauvegardes stockées dans Windows Azure](../../relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure.md)  
  
  
