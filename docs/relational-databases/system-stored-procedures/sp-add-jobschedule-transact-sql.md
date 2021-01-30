---
description: sp_add_jobschedule (Transact-SQL)
title: sp_add_jobschedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/28/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_jobschedule
- sp_add_jobschedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_jobschedule
ms.assetid: ffce19d9-d1d6-45b4-89fd-ad0f60822ba0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6c44f473d71f67718e9444ab2994fcfc17258411
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206463"
---
# <a name="sp_add_jobschedule-transact-sql"></a>sp_add_jobschedule (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crée une planification pour un travail de l’agent SQL.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
  > [!IMPORTANT]  
  > Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_jobschedule [ @job_id = ] job_id, | [ @job_name = ] 'job_name', [ @name = ] 'name'  
     [ , [ @enabled = ] enabled_flag ]  
     [ , [ @freq_type = ] frequency_type ]  
     [ , [ @freq_interval = ] frequency_interval ]  
     [ , [ @freq_subday_type = ] frequency_subday_type ]  
     [ , [ @freq_subday_interval = ] frequency_subday_interval ]  
     [ , [ @freq_relative_interval = ] frequency_relative_interval ]  
     [ , [ @freq_recurrence_factor = ] frequency_recurrence_factor ]  
     [ , [ @active_start_date = ] active_start_date ]  
     [ , [ @active_end_date = ] active_end_date ]  
     [ , [ @active_start_time = ] active_start_time ]  
     [ , [ @active_end_time = ] active_end_time ]  
     [ , [ @schedule_id = ] schedule_id OUTPUT ]
     [ , [ @schedule_uid = ] _schedule_uid OUTPUT ]
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_id = ] job_id` Numéro d’identification du travail auquel la planification est ajoutée. *job_id* est de type **uniqueidentifier**, sans valeur par défaut.  
  
`[ @job_name = ] 'job_name'` Nom du travail auquel la planification est ajoutée. *job_name* est de type **nvarchar (128)**, sans valeur par défaut.  
  
> [!NOTE]  
>  *Job_id* ou *job_name* doivent être spécifiés, mais ne peuvent pas être spécifiés.  
  
`[ @name = ] 'name'` Nom de la planification. *Name* est de type **nvarchar (128)**, sans valeur par défaut.  
  
`[ @enabled = ] enabled_flag` Indique l’état actuel de la planification. *enabled_flag* est de **type tinyint**, avec **1** comme valeur par défaut (activé). Si la **valeur est 0**, la planification n’est pas activée. Lorsque la planification n'est pas activée, le travail n'est pas exécuté.  
  
`[ @freq_type = ] frequency_type` Valeur qui indique le moment où la tâche doit être exécutée. *frequency_type* est de **type int**, avec **0** comme valeur par défaut et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**4**|Quotidien|  
|**8**|Hebdomadaire|  
|**16**|Mensuelle|  
|**32**|Mensuellement, par rapport à *frequency_interval.*|  
|**64**|Lancer lorsque le service de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] démarre.|  
|**128**|Lancer lorsque l'ordinateur est inactif.|  
  
`[ @freq_interval = ] frequency_interval` Jour d’exécution du travail. *frequency_interval* est de **type int**, avec 0 comme valeur par défaut, et dépend de la valeur de *frequency_type* , comme indiqué dans le tableau suivant :  
  
|Valeur|Résultat|  
|-----------|------------|  
|**1** (une fois)|*frequency_interval* n’est pas utilisé.|  
|**4** (tous les jours)|Tous les *frequency_interval* jours.|  
|**8** (hebdomadaire)|*frequency_interval* est une ou plusieurs des valeurs suivantes (combinées avec un opérateur logique or) :<br /><br /> 1 = Dimanche<br /><br /> 2 = Lundi<br /><br /> 4 = mardi<br /><br /> 8 = mercredi<br /><br /> 16 = jeudi<br /><br /> 32 = vendredi<br /><br /> 64 = samedi|  
|**16** (mensuellement)|Le *frequency_interval* jour du mois.|  
|**32** (par rapport à chaque mois)|*frequency_interval* est l’un des éléments suivants :<br /><br /> 1 = Dimanche<br /><br /> 2 = Lundi<br /><br /> 3 = Mardi<br /><br /> 4 = Mercredi<br /><br /> 5 = Jeudi<br /><br /> 6 = Vendredi<br /><br /> 7 = Samedi<br /><br /> 8 = Jour<br /><br /> 9 = Jour de semaine<br /><br /> 10 = Jour de week-end|  
|**64** (au [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] démarrage du service Agent)|*frequency_interval* n’est pas utilisé.|  
|**128**|*frequency_interval* n’est pas utilisé.|  
  
`[ @freq_subday_type = ] frequency_subday_type` Spécifie les unités pour *frequency_subday_interval*. *frequency_subday_type* est de **type int**, sans valeur par défaut, et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description (unité)|  
|-----------|--------------------------|  
|**0x1**|À une heure spécifiée|  
|**0X2**|Secondes|  
|**0x4**|Minutes|  
|**0x8**|Heures|  
  
`[ @freq_subday_interval = ] frequency_subday_interval` Nombre de *frequency_subday_type* périodes entre chaque exécution du travail. *frequency_subday_interval* est de **type int**, avec 0 comme valeur par défaut.  
  
`[ @freq_relative_interval = ] frequency_relative_interval` Définit davantage le *frequency_interval* lorsque *frequency_type* a la valeur **32** (mensuelle relative).  
  
 *frequency_relative_interval* est de **type int**, sans valeur par défaut, et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description (unité)|  
|-----------|--------------------------|  
|**1**|Premier|  
|**2**|Seconde|  
|**4**|Third|  
|**8**|Quatrième|  
|**16**|Dernier|  
  
 *frequency_relative_interval* indique l’occurrence de l’intervalle. Par exemple, si *frequency_relative_interval* a la valeur **2**, *frequency_type* a la valeur **32** et *frequency_interval* la valeur **3**, la tâche planifiée se produit le deuxième mardi de chaque mois.  
  
`[ @freq_recurrence_factor = ] frequency_recurrence_factor` Nombre de semaines ou de mois entre l’exécution planifiée du travail. *frequency_recurrence_factor* est utilisé uniquement si *frequency_type* a la valeur **8**, **16** ou **32**. *frequency_recurrence_factor* est de **type int**, avec 0 comme valeur par défaut.  
  
`[ @active_start_date = ] active_start_date` Date à laquelle l’exécution du travail peut commencer. *active_start_date* est de **type int**, sans valeur par défaut. La date est au format AAAAMMJJ. Si *active_start_date* est défini, la date doit être supérieure ou égale à 19900101.  
  
 Après avoir créé la planification, examinez la date de début et assurez-vous qu'elle est correcte. Pour plus d’informations, consultez la section « planification de la date de début » dans [créer et attacher des planifications à des travaux](../../ssms/agent/create-and-attach-schedules-to-jobs.md).  
  
`[ @active_end_date = ] active_end_date` Date à laquelle l’exécution du travail peut être arrêtée. *active_end_date* est de **type int**, sans valeur par défaut. La date est au format AAAAMMJJ.  
  
`[ @active_start_time = ] active_start_time` Heure à partir de n’importe quel jour entre *active_start_date* et *active_end_date* pour commencer l’exécution du travail. *active_start_time* est de **type int**, sans valeur par défaut. L'heure est au format HHMMSS et est exprimée sur 24 heures.  
  
`[ @active_end_time = active_end_time_` Heure de n’importe quel jour entre *active_start_date* et *active_end_date* pour terminer l’exécution du travail. *active_end_time* est de **type int**, sans valeur par défaut. L'heure est au format HHMMSS et est exprimée sur 24 heures.  
  
`[ @schedule_id = schedule_idOUTPUT` Numéro d’identification de planification affecté à la planification si elle est créée avec succès. *schedule_id* est une variable de sortie de type **int**, sans valeur par défaut.  
  
`[ @schedule_uid = ] _schedule_uidOUTPUT` Identificateur unique de la planification. *schedule_uid* est une variable de type **uniqueidentifier**.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Il est désormais possible de gérer la planification des travaux indépendamment des travaux. Pour ajouter une planification à un travail, utilisez **sp_add_schedule** pour créer la planification et **sp_attach_schedule** pour joindre la planification à un travail.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
 
 ## <a name="example"></a>Exemple
 L’exemple suivant affecte une planification de travail `SaturdayReports` qui s’exécute tous les samedis à 2:00 AM.
```sql  
EXEC msdb.dbo.sp_add_jobschedule 
        @job_name = N'SaturdayReports', -- Job name
        @name = N'Weekly_Sat_2AM',  -- Schedule name
        @freq_type = 8, -- Weekly
        @freq_interval = 64, -- Saturday
        @freq_recurrence_factor = 1, -- every week
        @active_start_time = 20000 -- 2:00 AM
```
  
## <a name="see-also"></a>Voir aussi  
 [Créer et attacher des planifications à des travaux](../../ssms/agent/create-and-attach-schedules-to-jobs.md)   
 [Planifier un travail](../../ssms/agent/schedule-a-job.md)   
 [Créer une planification](../../ssms/agent/create-a-schedule.md)   
 [SQL Server Agent des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_update_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_help_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-schedule-transact-sql.md)   
 [sp_attach_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
