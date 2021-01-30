---
description: sp_help_jobschedule (Transact-SQL)
title: sp_help_jobschedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobschedule
- sp_help_jobschedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobschedule
ms.assetid: 2cded902-9272-4667-ac4b-a4f95a9f008e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9765feaa9e3237f1ccd8d3a6dcc203c88758aefa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200130"
---
# <a name="sp_help_jobschedule-transact-sql"></a>sp_help_jobschedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations sur la planification des travaux utilisés par [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour effectuer des opérations automatisées.  
 
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_jobschedule { [ @job_id = ] job_id | [ @job_name = ] 'job_name' }  
     [ , [ @schedule_name = ] 'schedule_name' ]  
     [ , [ @schedule_id = ] schedule_id ]  
     [ , [ @include_description = ] include_description ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_id = ] job_id` Numéro d’identification du travail. *job_id* est de type **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
`[ @job_name = ] 'job_name'` Nom du travail. *job_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]
> *Job_id* ou *job_name* doivent être spécifiés, mais ne peuvent pas être spécifiés.

`[ @schedule_name = ] 'schedule_name'` Nom de l’élément de planification pour le travail. *schedule_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @schedule_id = ] schedule_id` Numéro d’identification de l’élément de planification pour le travail. *schedule_id* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @include_description = ] include_description` Spécifie s’il faut inclure la description de la planification dans le jeu de résultats. *include_description* est de **bit**, avec **0** comme valeur par défaut. Lorsque *include_description* a la **valeur 0**, la description de la planification n’est pas incluse dans le jeu de résultats. Lorsque *include_description* a la valeur **1**, la description de la planification est incluse dans le jeu de résultats.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|Numéro d'identificateur de la planification.|  
|**schedule_name**|**sysname**|Nom de la planification.|  
|**activé**|**int**|Indique si la planification est activée (**1**) ou désactivée (**0**).|  
|**freq_type**|**int**|Valeur indiquant quand le travail doit être exécuté.<br /><br /> **1** = une fois<br /><br /> **4** = tous les jours<br /><br /> **8** = hebdomadaire<br /><br /> **16** = mensuelle<br /><br /> **32** = mensuelle, par rapport au **freq_interval**<br /><br /> **64** = exécution au démarrage du service **SQLServerAgent** .|  
|**freq_interval**|**int**|Jours d’exécution du travail. La valeur dépend de la valeur de **freq_type**. Pour plus d’informations, consultez [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md).|  
|**freq_subday_type**|**int**|Unités pour **freq_subday_interval**. Pour plus d’informations, consultez [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md).|  
|**freq_subday_interval**|**int**|Nombre de **freq_subday_type** périodes entre chaque exécution du travail. Pour plus d’informations, consultez [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md).|  
|**freq_relative_interval**|**int**|Occurrence de la tâche planifiée de l' **freq_interval** chaque mois. Pour plus d’informations, consultez [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md).|  
|**freq_recurrence_factor**|**int**|Nombre de mois devant s'écouler entre les exécutions planifiées du travail.|  
|**active_start_date**|**int**|Date d'activation de la planification.|  
|**active_end_date**|**int**|Date de fin de la planification.|  
|**active_start_time**|**int**|Heure de début de la planification.|  
|**active_end_time**|**int**|Heure de fin de la planification.|  
|**date_created**|**datetime**|Date de création de la planification.|  
|**schedule_description**|**nvarchar(4000)**|Description en anglais de la planification qui est dérivée des valeurs dans les **planifications demsdb.dbo.sys**. Lorsque *include_description* a la **valeur 0**, cette colonne contient du texte indiquant que la description n’a pas été demandée.|  
|**next_run_date**|**int**|Date à laquelle la planification va lancer l'exécution du travail.|  
|**next_run_time**|**int**|Heure à laquelle la planification va lancer l'exécution du travail.|  
|**schedule_uid**|**uniqueidentifier**|Identificateur de la planification.|  
|**job_count**|**int**|Nombre de travaux retournés.|  
  
> **Remarque : sp_help_jobschedule** retourne des valeurs à partir des tables système **dbo.sysJobSchedules** et **dbo.sysplanifis** dans **msdb**. **sysjobschedules** est mis à jour toutes les 20 minutes. Cela peut affecter les valeurs retournées par cette procédure stockée.  
  
## <a name="remarks"></a>Notes  
 Les paramètres de **sp_help_jobschedule** peuvent être utilisés uniquement dans certaines combinaisons. Si *schedule_id* est spécifié, ni *job_id* ni *job_name* ne peuvent être spécifiés. Dans le cas contraire, les paramètres *job_id* ou *job_name* peuvent être utilisés avec *schedule_name*.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** . Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Les membres de **SQLAgentUserRole** peuvent uniquement afficher les propriétés des planifications de travaux dont ils sont propriétaires.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-the-job-schedule-for-a-specific-job"></a>R. Retour de la planification d'un travail spécifique  
 Cet exemple retourne les informations de planification du travail `BackupDatabase`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobschedule  
    @job_name = N'BackupDatabase' ;  
GO  
```  
  
### <a name="b-returning-the-job-schedule-for-a-specific-schedule"></a>B. Retour de la planification d'un travail pour une planification spécifique  
 Cet exemple retourne les informations de planification `NightlyJobs` et du travail `RunReports`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobschedule   
    @job_name = N'RunReports',  
    @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
### <a name="c-returning-the-job-schedule-and-schedule-description-for-a-specific-schedule"></a>C. Retour de la planification de travail et de la description d'une planification spécifique  
 Cet exemple retourne les informations de planification `NightlyJobs` et du travail `RunReports`. L'ensemble de résultats retourné comporte une description de la planification.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobschedule  
    @job_name = N'RunReports',  
    @schedule_name = N'NightlyJobs',  
    @include_description = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_update_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
