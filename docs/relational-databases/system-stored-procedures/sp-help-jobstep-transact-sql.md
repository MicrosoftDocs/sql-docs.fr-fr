---
description: sp_help_jobstep (Transact-SQL)
title: sp_help_jobstep (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobstep_TSQL
- sp_help_jobstep
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobstep
ms.assetid: 4a13b804-45f2-4f82-987f-42d9a57dd6db
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c6a4a59a69612c37a6b6d9208e121b11ea448a35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200105"
---
# <a name="sp_help_jobstep-transact-sql"></a>sp_help_jobstep (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie des informations sur les étapes d'un travail utilisé par le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent pour procéder à des actions automatisées.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_jobstep { [ @job_id = ] 'job_id' | [ @job_name = ] 'job_name' }  
     [ , [ @step_id = ] step_id ]   
     [ , [ @step_name = ] 'step_name' ]   
     [ , [ @suffix = ] suffix ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_id = ] 'job_id'` Numéro d’identification du travail pour lequel renvoyer des informations sur le travail. *job_id* est de type **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
`[ @job_name = ] 'job_name'` Nom du travail. *job_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *Job_id* ou *job_name* doivent être spécifiés, mais ne peuvent pas être spécifiés.  
  
`[ @step_id = ] step_id` Numéro d’identification de l’étape du travail. S'il n'est pas inclus, toutes les étapes du travail sont englobées. *step_id* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @step_name = ] 'step_name'` Nom de l’étape du travail. *step_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @suffix = ] suffix` Indicateur spécifiant si une description de texte est ajoutée à la colonne **Flags** dans la sortie. le *suffixe* est de **bit**, avec **0** comme valeur par défaut. Si le *suffixe* est **1**, une description est ajoutée.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**step_id**|**int**|Identificateur unique de l'étape.|  
|**step_name**|**sysname**|Nom de l’étape du travail.|  
|**sous-système**|**nvarchar(40)**|Sous-système dans lequel la commande d'étape doit être exécutée.|  
|**command**|**nvarchar(max)**|Commande exécutée dans l'étape.|  
|**flags**|**int**|Masque de bits des valeurs qui contrôle le comportement de l'étape.|  
|**cmdexec_success_code**|**int**|Pour une étape **CmdExec** , il s’agit du code de sortie du processus d’une commande réussie.|  
|**on_success_action**|**tinyint**|Action à effectuer si l'étape est exécutée correctement :<br /><br /> **1** = quitter le travail signalant la réussite.<br /><br /> **2** = quitter le travail signalant une défaillance.<br /><br /> **3** = passer à l’étape suivante.<br /><br /> **4** = passer à l’étape.|  
|**on_success_step_id**|**int**|Si **on_success_action** a la valeur 4, cela indique l’étape suivante à exécuter.|  
|**on_fail_action**|**tinyint**|Action à exécuter si l'étape échoue. Les valeurs sont identiques à celles de **on_success_action**.|  
|**on_fail_step_id**|**int**|Si **on_fail_action** a la valeur 4, cela indique l’étape suivante à exécuter.|  
|**server**|**sysname**|Réservé.|  
|**database_name**|**sysname**|Pour une étape [!INCLUDE[tsql](../../includes/tsql-md.md)], c'est la base de données dans laquelle la commande est exécutée.|  
|**database_user_name**|**sysname**|Pour une étape [!INCLUDE[tsql](../../includes/tsql-md.md)], c'est le contexte de l'utilisateur de la base de données dans lequel la commande est exécutée.|  
|**retry_attempts**|**int**|Nombre maximum de tentatives de la commande (en cas d'échecs).|  
|**retry_interval**|**int**|Intervalle (en minutes) entre les tentatives.|  
|**os_run_priority**|**int**|Réservé.|  
|**output_file_name**|**nvarchar(200)**|Fichier dans lequel la sortie de la commande doit être écrite ( [!INCLUDE[tsql](../../includes/tsql-md.md)] , **CmdExec** et les étapes **PowerShell** uniquement).|  
|**last_run_outcome**|**int**|Résultat de l'étape lors de sa dernière exécution.<br /><br /> **0** = échec<br /><br /> **1** = réussite<br /><br /> **2** = nouvelle tentative<br /><br /> **3** = annulé<br /><br /> **5** = inconnu|  
|**last_run_duration**|**int**|Durée (hhmmss) de l'étape lors de sa dernière exécution.|  
|**last_run_retries**|**int**|Nombre de tentatives de la commande lors de la dernière exécution de l'étape.|  
|**last_run_date**|**int**|Date de début de la dernière exécution de l'étape.|  
|**last_run_time**|**int**|Heure de début de la dernière exécution de l'étape.|  
|**proxy_id**|**int**|Proxy pour les étapes du travail.|  
  
## <a name="remarks"></a>Notes  
 **sp_help_jobstep** se trouve dans la base de données **msdb** .  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Les membres de **SQLAgentUserRole** peuvent uniquement afficher les étapes de travail des travaux dont ils sont propriétaires.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-return-information-for-all-steps-in-a-specific-job"></a>R. Renvoi d'informations sur toutes les étapes d'un travail spécifique  
 L'exemple suivant renvoie toutes les étapes du travail intitulé `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobstep  
    @job_name = N'Weekly Sales Data Backup' ;  
GO  
```  
  
### <a name="b-return-information-about-a-specific-job-step"></a>B. Renvoi d'informations sur une étape spécifique d'un travail  
 Cet exemple renvoie des informations sur la première étape du travail `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobstep  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_delete_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_help_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_update_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
