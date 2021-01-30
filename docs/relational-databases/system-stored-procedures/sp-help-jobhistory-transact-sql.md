---
description: sp_help_jobhistory (Transact-SQL)
title: sp_help_jobhistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobhistory_TSQL
- sp_help_jobhistory
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobhistory
ms.assetid: a944d44e-411b-4735-8ce4-73888d4262d7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2cfef1ba5f28b498ab360daf67cc1bf3c79f0d2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200141"
---
# <a name="sp_help_jobhistory-transact-sql"></a>sp_help_jobhistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fournit des informations sur les travaux pour les serveurs dans le domaine d'administration multiserveur.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_jobhistory [ [ @job_id = ] job_id ]   
     [ , [ @job_name = ] 'job_name' ]   
     [ , [ @step_id = ] step_id ]   
     [ , [ @sql_message_id = ] sql_message_id ]   
     [ , [ @sql_severity = ] sql_severity ]   
     [ , [ @start_run_date = ] start_run_date ]   
     [ , [ @end_run_date = ] end_run_date ]   
     [ , [ @start_run_time = ] start_run_time ]   
     [ , [ @end_run_time = ] end_run_time ]   
     [ , [ @minimum_run_duration = ] minimum_run_duration ]   
     [ , [ @run_status = ] run_status ]   
     [ , [ @minimum_retries = ] minimum_retries ]   
     [ , [ @oldest_first = ] oldest_first ]   
     [ , [ @server = ] 'server' ]   
     [ , [ @mode = ] 'mode' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_id = ] job_id` Numéro d’identification du travail. *job_id* est de type **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
`[ @job_name = ] 'job_name'` Nom du travail. *job_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @step_id = ] step_id` Numéro d’identification de l’étape. *step_id* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @sql_message_id = ] sql_message_id` Numéro d’identification du message d’erreur retourné par Microsoft SQL Server lors de l’exécution du travail. *sql_message_id* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @sql_severity = ] sql_severity` Niveau de gravité du message d’erreur retourné par SQL Server lors de l’exécution du travail. *sql_severity* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @start_run_date = ] start_run_date` Date à laquelle le travail a été démarré. *start_run_date* est de **type int**, avec NULL comme valeur par défaut. *start_run_date* doit être entré au format AAAAMMJJ, où yyyy est une année à quatre caractères, mm est un nom de mois à deux caractères et JJ est un nom de jour à deux caractères.  
  
`[ @end_run_date = ] end_run_date` Date à laquelle le travail a été effectué. *end_run_date* est de **type int**, avec NULL comme valeur par défaut. *end_run_date* doit être entré au format AAAAMMJJ, où yyyy est une année à quatre chiffres, mm est un nom de mois à deux caractères et JJ est un nom de jour à deux caractères.  
  
`[ @start_run_time = ] start_run_time` Heure à laquelle le travail a été démarré. *start_run_time* est de **type int**, avec NULL comme valeur par défaut. *start_run_time* doit être entré au format HHMMSS, où hh représente une heure sur deux caractères du jour, mm est une minute du jour à deux caractères et SS est une seconde du jour à deux caractères.  
  
`[ @end_run_time = ] end_run_time` Heure à laquelle le travail a terminé son exécution. *end_run_time* est de **type int**, avec NULL comme valeur par défaut. *end_run_time* doit être entré au format HHMMSS, où hh représente une heure sur deux caractères du jour, mm est une minute du jour à deux caractères et SS est une seconde du jour à deux caractères.  
  
`[ @minimum_run_duration = ] minimum_run_duration` Durée minimale pour la fin du travail. *minimum_run_duration* est de **type int**, avec NULL comme valeur par défaut. *minimum_run_duration* doit être entré au format HHMMSS, où hh représente une heure sur deux caractères du jour, mm est une minute du jour à deux caractères et SS est une seconde du jour à deux caractères.  
  
`[ @run_status = ] run_status` État d’exécution du travail. *run_status* est de **type int**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Échec|  
|**1**|Opération réussie|  
|**2**|Reprise (étape uniquement)|  
|**3**|Opération annulée|  
|**4**|Message en cours|  
|**5**|Unknown|  
  
`[ @minimum_retries = ] minimum_retries` Nombre minimal de tentatives d’exécution d’un travail. *minimum_retries* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @oldest_first = ] oldest_first` Indique s’il faut d’abord présenter la sortie avec les travaux les plus anciens. *oldest_first* est de **type int**, avec **0** comme valeur par défaut, qui présente les travaux les plus récents en premier. **1** présente en premier les travaux les plus anciens.  
  
`[ @server = ] 'server'` Nom du serveur sur lequel le travail a été effectué. *Server* est de type **nvarchar (30)**, avec NULL comme valeur par défaut.  
  
`[ @mode = ] 'mode'` Indique si SQL Server imprime toutes les colonnes dans le jeu de résultats (**Full**) ou un résumé des colonnes. *mode* est de type **varchar (7)**, avec **Summary** comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 La liste de colonnes réelle dépend de la valeur du *mode*. L’ensemble de colonnes le plus complet est indiqué ci-dessous et est retourné lorsque le *mode* est plein.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|Numéro d'identification de l'entrée d'historique.|  
|**job_id**|**uniqueidentifier**|Numéro d’identification du travail.|  
|**job_name**|**sysname**|Nom du travail.|  
|**step_id**|**int**|Numéro d’identification de l’étape ( **0** pour un historique des travaux).|  
|**step_name**|**sysname**|Nom de l'étape (NULL pour un historique des travaux).|  
|**sql_message_id**|**int**|Pour une étape [!INCLUDE[tsql](../../includes/tsql-md.md)], représente le numéro d'erreur [!INCLUDE[tsql](../../includes/tsql-md.md)] le plus récent affiché pendant l'exécution de la commande.|  
|**sql_severity**|**int**|Pour une étape [!INCLUDE[tsql](../../includes/tsql-md.md)], représente le degré de gravité [!INCLUDE[tsql](../../includes/tsql-md.md)] le plus élevé obtenu pendant l'exécution de la commande.|  
|**message**|**nvarchar(1024)**|Message d'historique d'étape ou de travail.|  
|**run_status**|**int**|Résultat du travail ou de l'étape.|  
|**run_date**|**int**|Date de début d'exécution du travail ou de l'étape.|  
|**run_time**|**int**|Heure de début d'exécution du travail ou de l'étape.|  
|**run_duration**|**int**|Durée, au format HHMMSS, de l'exécution du travail ou de l'étape.|  
|**operator_emailed**|**nvarchar(20**|Opérateur qui a reçu un courrier électronique concernant ce travail (NULL pour un historique d'étape).|  
|**operator_netsent**|**nvarchar(20**|Opérateur qui a reçu un message réseau concernant ce travail (NULL pour un historique d'étape).|  
|**operator_paged**|**nvarchar(20**|Opérateur qui a reçu un message par radiomessagerie concernant ce travail (NULL pour un historique d'étape).|  
|**retries_attempted**|**int**|Nombre de reprises de l'étape (0 pour un historique d'étape).|  
|**server**|**nvarchar(30)**|Serveur sur lequel est exécutée l'étape ou le travail. Est toujours (**local**).|  
  
## <a name="remarks"></a>Notes  
 **sp_help_jobhistory** retourne un rapport avec l’historique des tâches planifiées spécifiées. Si aucun paramètre n'est précisé, le rapport contient l'historique de tous les travaux planifiés.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Les membres du rôle de base de données **SQLAgentUserRole** peuvent uniquement afficher l’historique des travaux dont ils sont propriétaires.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-all-job-information-for-a-job"></a>R. Affichage de toutes les informations d'un travail  
 L'exemple ci-dessous répertorie toutes les informations du travail `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobhistory   
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
### <a name="b-listing-information-for-jobs-that-match-certain-conditions"></a>B. Affichage des informations sur les travaux répondant à certaines conditions  
 L'exemple ci-dessous imprime toutes les colonnes et toutes les informations de tous les travaux et toutes les étapes de travail qui ont échoué avec un message d'erreur `50100` (message d'erreur personnalisé) et un degré de gravité égal à `20`.  
  
```  
USE msdb  
GO  
  
EXEC dbo.sp_help_jobhistory  
    @sql_message_id = 50100,  
    @sql_severity = 20,  
    @run_status = 0,  
    @mode = N'FULL' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_purge_jobhistory &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-purge-jobhistory-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
