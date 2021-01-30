---
description: sp_help_job (Transact-SQL)
title: sp_help_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_job_TSQL
- sp_help_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_job
ms.assetid: 8a8b6104-e0e4-4d07-a2c3-f4243ee0d6fa
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 32ffb85143cf448742831071bee7d4b94a25ec57
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200159"
---
# <a name="sp_help_job-transact-sql"></a>sp_help_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations sur les travaux utilisés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent pour effectuer des opérations automatisées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_job { [ @job_id = ] job_id  
[ @job_name = ] 'job_name' }   
     [ , [ @job_aspect = ] 'job_aspect' ]   
     [ , [ @job_type = ] 'job_type' ]   
     [ , [ @owner_login_name = ] 'login_name' ]   
     [ , [ @subsystem = ] 'subsystem' ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @enabled = ] enabled ]   
     [ , [ @execution_status = ] status ]   
     [ , [ @date_comparator = ] 'date_comparison' ]   
     [ , [ @date_created = ] date_created ]   
     [ , [ @date_last_modified = ] date_modified ]   
     [ , [ @description = ] 'description_pattern' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_id = ] job_id` Numéro d’identification du travail. *job_id* est de type **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
`[ @job_name = ] 'job_name'` Nom du travail. *job_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Pour afficher un travail spécifique, *job_id* ou *job_name* doit être spécifié.  Omettez les deux *job_id* et *job_name* pour renvoyer des informations sur tous les travaux.
  
`[ @job_aspect = ] 'job_aspect'` Attribut de tâche à afficher. *job_aspect* est de type **varchar (9)**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**ALL**|Informations sur l'aspect du travail.|  
|**ATTENTE**|Informations sur le travail.|  
|**PLANIFICATIONS**|Informations sur la planification.|  
|**ÉTAPES**|Informations sur l'étape du travail|  
|**COMPILÉ**|Informations sur la cible|  
  
`[ @job_type = ] 'job_type'` Type des travaux à inclure dans le rapport. *job_type* est de type **varchar (12)**, avec NULL comme valeur par défaut. *job_type* peut être **local** ou **Multi-Server**.  
  
`[ @owner_login_name = ] 'login_name'` Nom de connexion du propriétaire du travail. *login_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @subsystem = ] 'subsystem'` Nom du sous-système. *Subsystem* est de type **nvarchar (40)**, avec NULL comme valeur par défaut.  
  
`[ @category_name = ] 'category'` Nom de la catégorie. *Category* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @enabled = ] enabled` Nombre indiquant si des informations sont affichées pour les travaux activés ou les travaux désactivés. *Enabled* est de **type tinyint**, avec NULL comme valeur par défaut. **1** indique que les travaux sont activés et **0** indique les travaux désactivés.  
  
`[ @execution_status = ] status` État d’exécution des travaux. *Status* est de **type int**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Renvoie uniquement les travaux qui ne sont ni inactifs ni suspendus.|  
|**1**|En cours d'exécution.|  
|**2**|Attente du thread.|  
|**3**|Entre deux reprises.|  
|**4**|Inactif.|  
|**5**|Suspendu.|  
|**7**|Effectue le travail.|  
  
`[ @date_comparator = ] 'date_comparison'` Opérateur de comparaison à utiliser dans les comparaisons de *Date_Created* et *Date_Modified*. *date_comparison* est de **type char (1)** et peut être =, \<, or > .  
  
`[ @date_created = ] date_created` Date à laquelle le travail a été créé. *Date_Created* est de **type DateTime**, avec NULL comme valeur par défaut.  
  
`[ @date_last_modified = ] date_modified` Date de la dernière modification du travail. *Date_Modified* est de **type DateTime**, avec NULL comme valeur par défaut.  
  
`[ @description = ] 'description_pattern'` Description du travail. *description_pattern* est de type **nvarchar (512)**, avec NULL comme valeur par défaut. *description_pattern* pouvez inclure les caractères génériques SQL Server pour les critères spéciaux.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si aucun argument n’est spécifié, **sp_help_job** retourne ce jeu de résultats.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|ID unique du travail.|  
|**originating_server**|**nvarchar(30)**|Nom du serveur d'origine du travail|  
|**name**|**sysname**|Nom du travail.|  
|**activé**|**tinyint**|Indique si le travail est activé pour être exécuté.|  
|**description**|**nvarchar(512)**|Description du travail.|  
|**start_step_id**|**int**|Identificateur de l'étape du travail à partir de laquelle l'exécution doit débuter.|  
|**category**|**sysname**|Catégorie de travail.|  
|**propriétaire**|**sysname**|Propriétaire du travail.|  
|**notify_level_eventlog**|**int**|**Masque binaire** indiquant les circonstances entraînant la consignation d’un événement de notification dans le journal des applications Microsoft Windows. Peut prendre l'une des valeurs suivantes :<br /><br /> **0** = jamais<br /><br /> **1** = lors de la tentative d’exécution d’un travail<br /><br /> **2** = en cas d’échec du travail<br /><br /> **3** = à chaque achèvement du travail (quel que soit le résultat du travail)|  
|**notify_level_email**|**int**|**Masque binaire** indiquant les circonstances entraînant l’envoi d’un message électronique de notification lorsqu’un travail est terminé. Les valeurs possibles sont les mêmes que pour **notify_level_eventlog**.|  
|**notify_level_netsend**|**int**|**Masque binaire** indiquant les circonstances entraînant l’envoi d’un message réseau lorsqu’un travail est terminé. Les valeurs possibles sont les mêmes que pour **notify_level_eventlog**.|  
|**notify_level_page**|**int**|**Masque binaire** indiquant les circonstances auxquelles une page doit être envoyée lorsqu’un travail est terminé. Les valeurs possibles sont les mêmes que pour **notify_level_eventlog**.|  
|**notify_email_operator**|**sysname**|Nom d'adresse électronique de l'opérateur à avertir.|  
|**notify_netsend_operator**|**sysname**|Nom de l'utilisateur ou de l'ordinateur utilisé pour envoyer les messages sur le réseau.|  
|**notify_page_operator**|**sysname**|Nom de l'utilisateur ou de l'ordinateur utilisé pour envoyer une page.|  
|**delete_level**|**int**|**Masque binaire** indiquant les circonstances dans lesquelles le travail doit être supprimé lorsqu’un travail se termine. Les valeurs possibles sont les mêmes que pour **notify_level_eventlog**.|  
|**date_created**|**datetime**|Date de création du travail.|  
|**date_modified**|**datetime**|Date de dernière modification du travail.|  
|**version_number**|**int**|Version du travail (mise à jour automatique à chaque modification).|  
|**last_run_date**|**int**|Date du début de la dernière exécution du travail.|  
|**last_run_time**|**int**|Heure du début de la dernière exécution du travail.|  
|**last_run_outcome**|**int**|Résultat du travail lors de sa dernière exécution :<br /><br /> **0** = échec<br /><br /> **1** = réussite<br /><br /> **3** = annulé<br /><br /> **5** = inconnu|  
|**next_run_date**|**int**|Date prévue pour la prochaine exécution du travail.|  
|**next_run_time**|**int**|Heure prévue pour la prochaine exécution du travail.|  
|**next_run_schedule_id**|**int**|Numéro d'identification de la prochaine exécution planifiée.|  
|**current_execution_status**|**int**|État de l’exécution actuelle :<br /><br /> **1** = en cours d’exécution<br /><br /> **2** = en attente de thread<br /><br /> **3** = entre les tentatives<br /><br /> **4** = inactif<br /><br /> **5** = suspendu<br /><br /> **6** = obsolète<br /><br /> **7** = PerformingCompletionActions|  
|**current_execution_step**|**sysname**|Étape d'exécution du travail en cours.|  
|**current_retry_attempt**|**int**|Si le travail est en cours d'exécution et que l'étape est effectuée plusieurs fois, ce paramètre correspond à la tentative en cours.|  
|**has_step**|**int**|Nombre d'étapes du travail.|  
|**has_schedule**|**int**|Nombre de planifications d'un travail.|  
|**has_target**|**int**|Nombre de serveurs cibles d'un travail.|  
|**type**|**int**|Type du travail.<br /><br /> 1 = Travail local.<br /><br /> **2** = travail multiserveur.<br /><br /> **0** = le travail n’a pas de serveurs cibles.|  
  
 Si *job_id* ou *job_name* est spécifié, **sp_help_job** retourne ces jeux de résultats supplémentaires pour les étapes de travail, les planifications de travaux et les serveurs cibles du travail.  
  
 Voici le jeu de résultats des étapes de travail :  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**step_id**|**int**|Identificateur unique de cette étape (pour ce travail).|  
|**step_name**|**sysname**|Nom de l'étape|  
|**sous-système**|**nvarchar(40)**|Sous-système dans lequel la commande d'étape doit être exécutée.|  
|**command**|**nvarchar (3200)**|Commande à exécuter.|  
|**flags**|**nvarchar(4000)**|**Masque** de temps des valeurs qui contrôlent le comportement de l’étape.|  
|**cmdexec_success_code**|**int**|Pour une étape **CmdExec** , il s’agit du code de sortie du processus d’une commande réussie.|  
|**on_success_action**|**nvarchar(4000)**|Que faire si l'étape est exécutée correctement :<br /><br /> **1** = quitter avec succès.<br /><br /> **2** = quitter avec échec.<br /><br /> **3** = passer à l’étape suivante.<br /><br /> **4** = passer à l’étape.|  
|**on_success_step_id**|**int**|Si **on_success_action** a la valeur **4**, cela indique l’étape suivante à exécuter.|  
|**on_fail_action**|**nvarchar(4000)**|Action à exécuter si l'exécution de l'étape échoue. Les valeurs sont les mêmes que pour **on_success_action**.|  
|**on_fail_step_id**|**int**|Si **on_fail_action** a la valeur **4**, cela indique l’étape suivante à exécuter.|  
|**server**|**sysname**|Réservé.|  
|**database_name**|**sysname**|Pour une étape [!INCLUDE[tsql](../../includes/tsql-md.md)], c'est la base de données dans laquelle la commande sera exécutée.|  
|**database_user_name**|**sysname**|Pour une étape [!INCLUDE[tsql](../../includes/tsql-md.md)], c'est le contexte de l'utilisateur de la base de données dans lequel la commande est exécutée.|  
|**retry_attempts**|**int**|Nombre de tentatives maximum de la commande (si elle échoue) avant que l'étape soit considérée comme un échec.|  
|**retry_interval**|**int**|Intervalle (en minutes) entre chaque tentative.|  
|**os_run_priority**|**varchar (4000)**|Réservé.|  
|**output_file_name**|**varchar (200)**|Fichier dans lequel la sortie de la commande doit être écrite ( [!INCLUDE[tsql](../../includes/tsql-md.md)] et les étapes **CmdExec** uniquement).|  
|**last_run_outcome**|**int**|Résultat de l'étape lors de sa dernière exécution.<br /><br /> **0** = échec<br /><br /> **1** = réussite<br /><br /> **3** = annulé<br /><br /> **5** = inconnu|  
|**last_run_duration**|**int**|Durée (en secondes) de l'étape lors de sa dernière exécution.|  
|**last_run_retries**|**int**|Nombre de tentatives de la commande lors de la dernière exécution de l'étape.|  
|**last_run_date**|**int**|Date de début de la dernière exécution de l'étape.|  
|**last_run_time**|**int**|Heure de début de la dernière exécution de l'étape.|  
|**proxy_id**|**int**|Proxy pour les étapes du travail.|  
  
 Voici le jeu de résultats des planifications de travail.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|Identificateur de la planification (unique pour tous les travaux).|  
|**schedule_name**|**sysname**|Nom de la planification (unique pour ce travail).|  
|**activé**|**int**|Indique si la planification est active (**1**) ou non (**0**).|  
|**freq_type**|**int**|Valeur indiquant la fréquence d'exécution du travail.<br /><br /> **1** = une fois<br /><br /> **4** = tous les jours<br /><br /> **8** = hebdomadaire<br /><br /> **16** = mensuelle<br /><br /> **32** = mensuelle, par rapport au **freq_interval**<br /><br /> **64** = exécution au démarrage du service **SQLServerAgent** .|  
|**freq_interval**|**int**|Jours d’exécution du travail. La valeur dépend de la valeur de **freq_type**. Pour plus d’informations, consultez [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)|  
|**freq_subday_type**|**Int**|Unités pour **freq_subday_interval**. Pour plus d’informations, consultez [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)|  
|**freq_subday_interval**|**int**|Nombre de **freq_subday_type** périodes entre chaque exécution du travail. Pour plus d’informations, consultez [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)|  
|**freq_relative_interval**|**int**|Occurrence de la tâche planifiée de l' **freq_interval** chaque mois. Pour plus d’informations, consultez [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)|  
|**freq_recurrence_factor**|**int**|Nombre de mois devant s'écouler entre les exécutions planifiées du travail.|  
|**active_start_date**|**int**|Date de démarrage de l'exécution du travail.|  
|**active_end_date**|**int**|Date de fin d’exécution du travail.|  
|**active_start_time**|**int**|Heure de début de l’exécution du travail sur **active_start_date.**|  
|**active_end_time**|**int**|Heure de fin de l’exécution du travail sur **active_end_date**.|  
|**date_created**|**datetime**|Date de création de la planification.|  
|**schedule_description**|**nvarchar(4000)**|Description en anglais de la planification (sur demande).|  
|**next_run_date**|**int**|Date à laquelle la planification va lancer l'exécution du travail.|  
|**next_run_time**|**int**|Heure à laquelle la planification va lancer l'exécution du travail.|  
|**schedule_uid**|**uniqueidentifier**|Identificateur de la planification.|  
|**job_count**|**int**|Retourne le nombre de travaux qui référencent cette planification.|  
  
 Jeu de résultats pour les serveurs cibles de travaux.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|Identificateur du serveur cible.|  
|**server_name**|**nvarchar(30)**|Nom de l'ordinateur du serveur cible.|  
|**enlist_date**|**datetime**|Date d'inscription du serveur cible sur le serveur maître.|  
|**last_poll_date**|**datetime**|Date à laquelle le serveur cible a interrogé pour la dernière fois le serveur maître.|  
|**last_run_date**|**int**|Date du début de la dernière exécution du travail sur ce serveur cible.|  
|**last_run_time**|**int**|Heure du début de la dernière exécution du travail sur ce serveur cible.|  
|**last_run_duration**|**int**|Durée du travail lors de sa dernière exécution sur ce serveur cible.|  
|**last_run_outcome**|**tinyint**|Résultat du travail à l'issue de sa dernière exécution sur ce serveur.<br /><br /> **0** = échec<br /><br /> **1** = réussite<br /><br /> **3** = annulé<br /><br /> **5** = inconnu|  
|**last_outcome_message**|**nvarchar(1024)**|Message indiquant le résultat du travail lors de sa dernière exécution sur ce serveur cible.|  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Les membres de **SQLAgentUserRole** peuvent uniquement afficher les travaux dont ils sont propriétaires. Les membres de **sysadmin**, **SQLAgentReaderRole** et **SQLAgentOperatorRole** peuvent afficher tous les travaux locaux et multiserveurs.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-list-information-for-all-jobs"></a>R. Création de la liste des informations sur tous les travaux  
 L'exemple suivant exécute la procédure `sp_help_job` sans aucun paramètre afin que des informations sur tous les travaux définis dans la base de données `msdb` soient retournées.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_job ;  
GO  
```  
  
### <a name="b-listing-information-for-jobs-matching-a-specific-criteria"></a>B. Création de la liste des informations pour les travaux correspondant à un critère spécifique  
 L'exemple suivant répertorie les informations sur les travaux multiserveurs détenus par `françoisa` lorsque les travaux sont activés et exécutés.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_job   
   @job_type = N'MULTI-SERVER',  
   @owner_login_name = N'françoisa',  
   @enabled = 1,  
   @execution_status = 1 ;  
GO  
```  
  
### <a name="c-listing-all-aspects-of-information-for-a-job"></a>C. Création de la liste de tous les aspects des informations pour un travail  
 L'exemple suivant répertorie tous les aspects des informations pour le travail `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_job  
    @job_name = N'NightlyBackups',  
    @job_aspect = N'ALL' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)   
 [sp_delete_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_update_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
