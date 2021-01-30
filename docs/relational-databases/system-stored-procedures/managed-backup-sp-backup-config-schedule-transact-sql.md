---
description: managed_backup.sp_backup_config_schedule managed_backup (Transact-SQL)
title: managed_backup managed_backup.sp_backup_config_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/20/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_backup_config_schedule_TSQL
- managed_backup.sp_backup_config_schedule
- managed_backup.sp_backup_config_schedule_TSQL
- sp_backup_config_schedule
dev_langs:
- TSQL
helpviewer_keywords:
- managed_backup.sp_backup_config_schedule
- sp_backup_config_schedule
ms.assetid: 82541160-d1df-4061-91a5-6868dd85743a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 46acc8efa858c3d0b3dfcd4913b9bfb4eb91ba30
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203700"
---
# <a name="managed_backupsp_backup_config_schedule-transact-sql"></a>managed_backup.sp_backup_config_schedule managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Configure les options de planification automatisées ou personnalisées pour [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] .  
    
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```vb  
EXEC managed_backup.sp_backup_config_schedule   
    [@database_name = ] 'database_name'
    ,[@scheduling_option = ] {'Custom' | 'System'}  
    ,[@full_backup_freq_type = ] {'Daily' | 'Weekly'}  
    ,[@days_of_week = ] 'days_of_the_week'  
    ,[@backup_begin_time = ] 'begin time of the backup window'  
    ,[@backup_duration = ] 'backup window length'  
    ,[@log_backup_freq = ] 'frequency of log backup'  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Arguments  
 @database_name  
 Nom de la base de données pour activer la sauvegarde managée sur une base de données spécifique. Si la valeur est NULL ou *, cette sauvegarde managée s’applique à toutes les bases de données sur le serveur.  
  
 @scheduling_option  
 Spécifiez « système » pour la planification de sauvegarde contrôlée par le système. Spécifiez « Custom » pour une planification personnalisée définie par l’autre paramètres.  
  
 @full_backup_freq_type  
 Type de fréquence de l’opération de sauvegarde managée, qui peut être définie sur « quotidien » ou « hebdomadaire ».  
  
 @days_of_week  
 Les jours de la semaine pour les sauvegardes lorsque @full_backup_freq_type est défini sur hebdomadaire. Spécifiez des noms de chaîne complets comme « Monday ».  Vous pouvez également spécifier plus d’un nom de jour, séparé par un canal. Par exemple N’Monday | Mercredi | Vendredi».  
  
 @backup_begin_time  
 Heure de début de la fenêtre de sauvegarde. Les sauvegardes ne seront pas démarrées en dehors de la fenêtre de temps, qui est définie par une combinaison de @backup_begin_time et @backup_duration .  
  
 @backup_duration  
 Durée de la fenêtre de temps de sauvegarde. Notez qu’il n’y a aucune garantie que les sauvegardes seront effectuées pendant la fenêtre de temps définie par @backup_begin_time et @backup_duration . Les opérations de sauvegarde démarrées dans cette fenêtre de temps mais qui dépassent la durée de la fenêtre ne sont pas annulées.  
  
 @log_backup_freq  
 Cela détermine la fréquence des sauvegardes du journal des transactions. Ces sauvegardes se produisent à intervalles réguliers plutôt qu’en fonction de la planification spécifiée pour les sauvegardes de base de données. @log_backup_freq peut être en minutes ou en heures et être `0:00` valide, ce qui indique l’absence de sauvegardes du journal. La désactivation des sauvegardes de journaux ne conviendrait que pour les bases de données avec un mode de récupération simple.  
  
> [!NOTE]  
>  Si le mode de récupération passe de simple à Full, vous devez reconfigurer le log_backup_freq de `0:00` à une valeur différente de zéro.  
  
## <a name="return-code-value"></a>Valeur du code de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données **db_backupoperator** , avec les autorisations **ALTER ANY CREDENTIAL** et **Execute** sur **sp_delete_backuphistory** procédure stockée.  
  
## <a name="see-also"></a>Voir aussi  
 [managed_backup.sp_backup_config_basic managed_backup (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-basic-transact-sql.md)   
 [managed_backup.sp_backup_config_advanced &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-advanced-transact-sql.md)  
  
  
