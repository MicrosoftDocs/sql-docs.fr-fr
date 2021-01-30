---
description: sp_help_jobs_in_schedule (Transact-SQL)
title: sp_help_jobs_in_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobs_in_schedule_TSQL
- sp_help_jobs_in_schedule
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobs_in_schedule
ms.assetid: 1168aa2c-136b-4ba3-b18e-9070d95a26fa
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1890fc4b8aa3757b55dfc5ad7e33070126300897
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200149"
---
# <a name="sp_help_jobs_in_schedule-transact-sql"></a>sp_help_jobs_in_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations sur les travaux auxquels une planification particulière est attachée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_jobs_in_schedule   
     [ @schedule_name = ] 'schedule_name' ,  
     [ @schedule_id = ] schedule_id   
```  
  
## <a name="arguments"></a>Arguments  
`[ @schedule_id = ] schedule_id` Identificateur de la planification pour laquelle répertorier les informations. *schedule_id* est de **type int**, sans valeur par défaut. *Schedule_id* ou *schedule_name* peuvent être spécifiés.  
  
`[ @schedule_name = ] 'schedule_name'` Nom de la planification pour laquelle répertorier les informations. *schedule_name* est de **type sysname**, sans valeur par défaut. *Schedule_id* ou *schedule_name* peuvent être spécifiés.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Retourne le jeu de résultats suivant :  
  
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
|**notify_level_eventlog**|**int**|Masque binaire indiquant les circonstances entraînant la consignation d'une notification d'événement dans le journal des applications Microsoft Windows. Peut prendre l'une des valeurs suivantes :<br /><br /> **0** = jamais<br /><br /> **1** = lors de la tentative d’exécution d’un travail<br /><br /> **2** = en cas d’échec du travail<br /><br /> **3** = à chaque achèvement du travail (quel que soit le résultat du travail)|  
|**notify_level_email**|**int**|Masque de bits indiquant les conditions d'envoi d'un message électronique en fin de travail. Les valeurs possibles sont les mêmes que pour **notify_level_eventlog**.|  
|**notify_level_netsend**|**int**|Masque de bits précisant les conditions d'envoi d'un message réseau en fin de travail. Les valeurs possibles sont les mêmes que pour **notify_level_eventlog**.|  
|**notify_level_page**|**int**|Masque de bits indiquant les conditions d'envoi d'une page en fin de travail. Les valeurs possibles sont les mêmes que pour **notify_level_eventlog**.|  
|**notify_email_operator**|**sysname**|Nom d'adresse électronique de l'opérateur à avertir.|  
|**notify_netsend_operator**|**sysname**|Nom de l'utilisateur ou de l'ordinateur utilisé pour envoyer les messages sur le réseau.|  
|**notify_page_operator**|**sysname**|Nom de l'utilisateur ou de l'ordinateur utilisé pour envoyer une page.|  
|**delete_level**|**int**|Masque de bits indiquant les conditions de suppression du travail en fin de travail. Les valeurs possibles sont les mêmes que pour **notify_level_eventlog**.|  
|**date_created**|**datetime**|Date de création du travail.|  
|**date_modified**|**datetime**|Date de dernière modification du travail.|  
|**version_number**|**int**|Version du travail (mise à jour automatique à chaque modification).|  
|**last_run_date**|**int**|Date du début de la dernière exécution du travail.|  
|**last_run_time**|**int**|Heure du début de la dernière exécution du travail.|  
|**last_run_outcome**|**int**|Résultat du travail lors de sa dernière exécution :<br /><br /> **0** = échec<br /><br /> **1** = réussite<br /><br /> **3** = annulé<br /><br /> **5** = inconnu|  
|**next_run_date**|**int**|Date prévue pour la prochaine exécution du travail.|  
|**next_run_time**|**int**|Heure prévue pour la prochaine exécution du travail.|  
|**next_run_schedule_id**|**int**|Numéro d'identification de la prochaine exécution planifiée.|  
|**current_execution_status**|**int**|État de l’exécution actuelle.|  
|**current_execution_step**|**sysname**|Étape d'exécution du travail en cours.|  
|**current_retry_attempt**|**int**|Si le travail est en cours d'exécution et que l'étape est effectuée plusieurs fois, ce paramètre correspond à la tentative en cours.|  
|**has_step**|**int**|Nombre d'étapes du travail.|  
|**has_schedule**|**int**|Nombre de planifications d'un travail.|  
|**has_target**|**int**|Nombre de serveurs cibles d'un travail.|  
|**type**|**int**|Type du travail :<br /><br /> **1** = travail local.<br /><br /> **2** = travail multiserveur.<br /><br /> **0** = le travail n’a pas de serveurs cibles.|  
  
## <a name="remarks"></a>Notes  
 Cette procédure affiche des informations sur les travaux attachés à la planification spécifiée.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Les membres de **SQLAgentUserRole** peuvent uniquement afficher l’état des travaux dont ils sont propriétaires.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant affiche les travaux attachés à la planification `NightlyJobs`.  
  
```  
USE msdb ;  
GO  
  
EXEC sp_help_jobs_in_schedule  
    @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Agent des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_attach_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_detach_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-detach-schedule-transact-sql.md)  
  
  
