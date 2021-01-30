---
description: sp_update_schedule (Transact-SQL)
title: sp_update_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_schedule
- sp_update_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_schedule
ms.assetid: 97b3119b-e43e-447a-bbfb-0b5499e2fefe
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 13359a3d3588e238ef0b78ed5bfe5148fb447dae
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189541"
---
# <a name="sp_update_schedule-transact-sql"></a>sp_update_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie les paramètres d'une planification de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_update_schedule   
    {   [ @schedule_id = ] schedule_id   
      | [ @name = ] 'schedule_name' }  
    [ , [ @new_name = ] new_name ]  
    [ , [ @enabled = ] enabled ]  
    [ , [ @freq_type = ] freq_type ]  
    [ , [ @freq_interval = ] freq_interval ]   
    [ , [ @freq_subday_type = ] freq_subday_type ]   
    [ , [ @freq_subday_interval = ] freq_subday_interval ]   
    [ , [ @freq_relative_interval = ] freq_relative_interval ]   
    [ , [ @freq_recurrence_factor = ] freq_recurrence_factor ]   
    [ , [ @active_start_date = ] active_start_date ]   
    [ , [ @active_end_date = ] active_end_date ]   
    [ , [ @active_start_time = ] active_start_time ]   
    [ , [ @active_end_time = ] active_end_time ]   
    [ , [ @owner_login_name = ] 'owner_login_name' ]  
    [ , [ @automatic_post =] automatic_post ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @schedule_id = ] schedule_id` Identificateur de la planification à modifier. *schedule_id* est de **type int**, sans valeur par défaut. *Schedule_id* ou *schedule_name* doivent être spécifiés.  
  
`[ @name = ] 'schedule_name'` Nom de la planification à modifier. *schedule_name* est de **type sysname**, sans valeur par défaut. *Schedule_id* ou *schedule_name* doivent être spécifiés.  
  
`[ @new_name = ] new_name` Nouveau nom de la planification. *new_name* est de **type sysname**, avec NULL comme valeur par défaut. Lorsque *new_name* a la valeur null, le nom de la planification n’est pas modifié.  
  
`[ @enabled = ] enabled` Indique l’état actuel de la planification. *Enabled* est de **type tinyint**, avec **1** comme valeur par défaut (activé). Si la **valeur est 0**, la planification n’est pas activée. Si la planification n'est pas activée, aucun travail n'est exécuté dans cette dernière.  
  
`[ @freq_type = ] freq_type` Valeur indiquant quand une tâche doit être exécutée. *freq_type* est de **type int**, avec **0** comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**4**|Quotidien|  
|**8**|Hebdomadaire|  
|**16**|Mensuelle|  
|**32**|Tous les mois, par rapport à l' *intervalle FREQ*|  
|**64**|Lancé au démarrage du service SQLServerAgent|  
|**128**|Exécution pendant une période d'inactivité de l'ordinateur.|  
  
`[ @freq_interval = ] freq_interval` Jours d’exécution d’un travail. *freq_interval* est de **type int**, avec **0** comme valeur par défaut, et dépend de la valeur de *freq_type*.  
  
|Valeur de *freq_type*|Effet sur *freq_interval*|  
|---------------------------|--------------------------------|  
|**1** (une fois)|*freq_interval* n’est pas utilisé.|  
|**4** (tous les jours)|Tous les *freq_interval* jours.|  
|**8** (hebdomadaire)|*freq_interval* est une ou plusieurs des valeurs suivantes (combinées avec un opérateur logique **or** ) :<br /><br /> **1** = dimanche<br /><br /> **2** = lundi<br /><br /> **4** = mardi<br /><br /> **8** = mercredi<br /><br /> **16** = jeudi<br /><br /> **32** = vendredi<br /><br /> **64** = samedi|  
|**16** (mensuellement)|Le *freq_interval* jour du mois.|  
|**32** (par rapport à chaque mois)|*freq_interval* est l’un des éléments suivants :<br /><br /> **1** = dimanche<br /><br /> **2** = lundi<br /><br /> **3** = mardi<br /><br /> **4** = mercredi<br /><br /> **5** = jeudi<br /><br /> **6** = vendredi<br /><br /> **7** = samedi<br /><br /> **8** = jour<br /><br /> **9** = jour de semaine<br /><br /> **10** = jour de week-end|  
|**64** (au démarrage du service SQLServerAgent)|*freq_interval* n’est pas utilisé.|  
|**128**|*freq_interval* n’est pas utilisé.|  
  
`[ @freq_subday_type = ] freq_subday_type` Spécifie les unités pour *freq_subday_interval * *.* *freq_subday_type* est de **type int**, avec **0** comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description (unité)|  
|-----------|--------------------------|  
|**0x1**|À une heure spécifiée|  
|**0X2**|Secondes|  
|**0x4**|Minutes|  
|**0x8**|Heures|  
  
`[ @freq_subday_interval = ] freq_subday_interval` Nombre de *freq_subday_type* périodes entre chaque exécution d’un travail. *freq_subday_interval* est de **type int**, avec **0** comme valeur par défaut.  
  
`[ @freq_relative_interval = ] freq_relative_interval` L’occurrence d’un travail de *freq_interval* par mois, si *freq_interval* est de **32** (mensuel relatif). *freq_relative_interval* est de **type int**, avec **0** comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description (unité)|  
|-----------|--------------------------|  
|**1**|Premier|  
|**2**|Seconde|  
|**4**|Third|  
|**8**|Quatrième|  
|**16**|Dernier|  
  
`[ @freq_recurrence_factor = ] freq_recurrence_factor` Nombre de semaines ou de mois entre l’exécution planifiée d’un travail. *freq_recurrence_factor* est utilisé uniquement si *freq_type* est **8**, **16** ou **32**. *freq_recurrence_factor* est de **type int**, avec **0** comme valeur par défaut.  
  
`[ @active_start_date = ] active_start_date` Date à laquelle l’exécution d’un travail peut commencer. *active_start_date* est de **type int**, avec NULL comme valeur par défaut, qui indique la date du jour. La date est au format AAAAMMJJ. Si *active_start_date* n’a pas la valeur null, la date doit être supérieure ou égale à 19900101.  
  
 Après avoir créé la planification, examinez la date de début et assurez-vous qu'elle est correcte. Pour plus d’informations, consultez la section « planification de la date de début » dans [créer et attacher des planifications à des travaux](../../ssms/agent/create-and-attach-schedules-to-jobs.md).  
  
`[ @active_end_date = ] active_end_date` Date à laquelle l’exécution d’un travail peut être arrêtée. *active_end_date* est de **type int**, avec **99991231** comme valeur par défaut, qui indique le 31 décembre 9999. La mise en forme est la suivante : AAAAMMJJ.  
  
`[ @active_start_time = ] active_start_time` Heure à partir de n’importe quel jour entre *active_start_date* et *active_end_date* pour commencer l’exécution d’un travail. *active_start_time* est de **type int**, avec 000000 comme valeur par défaut, qui indique 12:00:00 sur une horloge de 24 heures. Elle doit être au format HHMMSS.  
  
`[ @active_end_time = ] active_end_time` L’heure de n’importe quel jour entre *active_start_date* et *active_end_date* pour terminer l’exécution d’un travail. *active_end_time* est de **type int**, avec **235959** comme valeur par défaut, qui indique 11:59:59 P.M. sur une horloge de 24 heures. Elle doit être au format HHMMSS.  
  
`[ @owner_login_name = ] 'owner_login_name']` Nom du principal de serveur qui possède la planification. *owner_login_name* est de **type sysname**, avec NULL comme valeur par défaut, qui indique que la planification appartient au créateur.  
  
`[ @automatic_post = ] automatic_post` Réservé.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Tous les travaux qui utilisent la planification utilisent immédiatement les nouveaux paramètres. Cependant, la modification d'une planification n'entraîne pas l'arrêt des travaux en cours d'exécution.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Seuls les membres de **sysadmin** peuvent modifier une planification appartenant à un autre utilisateur.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant montre comment changer l'état activé de la planification `NightlyJobs` en `0` et comment définir le propriétaire à `terrid`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_schedule  
    @name = 'NightlyJobs',  
    @enabled = 0,  
    @owner_login_name = 'terrid' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Créer et attacher des planifications à des travaux](../../ssms/agent/create-and-attach-schedules-to-jobs.md)   
 [Planifier un travail](../../ssms/agent/schedule-a-job.md)   
 [Créer une planification](../../ssms/agent/create-a-schedule.md)   
 [SQL Server Agent des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_add_jobschedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md)   
 [sp_delete_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_help_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-schedule-transact-sql.md)   
 [sp_attach_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
  
