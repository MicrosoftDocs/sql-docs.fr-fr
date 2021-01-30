---
description: sp_help_jobactivity (Transact-SQL)
title: sp_help_jobactivity (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobactivity_TSQL
- sp_help_jobactivity
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobactivity
ms.assetid: d344864f-b4d3-46b1-8933-b81dec71f511
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b399b08120b8153af01d0faab42633f5a8325142
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199648"
---
# <a name="sp_help_jobactivity-transact-sql"></a>sp_help_jobactivity (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Répertorie les informations concernant l'état d'exécution des travaux de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_jobactivity { [ @job_id = ] job_id | [ @job_name = ] 'job_name' }  
     [ , [ @session_id = ] session_id ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_id = ] job_id` Numéro d’identification du travail. *job_id* est de type **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
`[ @job_name = ] 'job_name'` Nom du travail. *job_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *Job_id* ou *job_name* doivent être spécifiés, mais ne peuvent pas être spécifiés.  
  
`[ @session_id = ] session_id` ID de session pour lequel des informations doivent être signalées. *session_id* est de **type int**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Retourne le jeu de résultats suivant :  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|Numéro d'identification de la session de l'Agent.|  
|**job_id**|**uniqueidentifier**|Identificateur du travail.|  
|**job_name**|**sysname**|Nom du travail.|  
|**run_requested_date**|**datetime**|Moment auquel le travail devait s'exécuter.|  
|**run_requested_source**|**sysname**|Source de la requête pour exécuter le travail. Valeurs possibles :<br /><br /> **1** = exécuter selon une planification<br /><br /> **2** = exécution en réponse à une alerte<br /><br /> **3** = exécution au démarrage<br /><br /> **4** = exécution par l’utilisateur<br /><br /> **6** = exécution sur la planification d’inactivité de l’UC|  
|**queued_date**|**datetime**|Moment où la requête a intégré une file d'attente. NULL si le travail a été exécuté directement.|  
|**start_execution_date**|**datetime**|Moment où le travail a été attribué à un thread exécutable.|  
|**last_executed_step_id**|**int**|ID de l'étape du travail exécutée en dernier lieu.|  
|**last_exectued_step_date**|**datetime**|Heure à laquelle l'étape du travail exécutée en dernier lieu a démarré son exécution.|  
|**stop_execution_date**|**datetime**|Heure à laquelle l'exécution du travail s'est terminée.|  
|**next_scheduled_run_date**|**datetime**|Date et heure prévues pour la prochaine exécution du travail.|  
|**job_history_id**|**int**|Identificateur de l'historique des travaux dans la table d'historique des travaux.|  
|**message**|**nvarchar(1024)**|Message produit lors de la dernière exécution du travail.|  
|**run_status**|**int**|État retourné de la dernière exécution du travail :<br /><br /> **0** = échec de l’erreur<br /><br /> **1** = réussite<br /><br /> **3** = annulé<br /><br /> **5** = état inconnu|  
|**operator_id_emailed**|**int**|Numéro d'identification de l'opérateur notifié par courrier électronique en fin de travail.|  
|**operator_id_netsent**|**int**|Numéro d’identification de l’opérateur notifié via **net send** à la fin du travail.|  
|**operator_id_paged**|**int**|Numéro d'identification de l'opérateur notifié par radiomessagerie en fin de travail.|  
  
## <a name="remarks"></a>Notes  
 Cette procédure fournit un instantané de l'état actuel des travaux. Les résultats renvoyés représentent des informations correspondant au moment du traitement de la requête.  
  
 L'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée un ID de session chaque fois que le service SQL Agent démarre. L’ID de session est stocké dans la table **msdb.dbo.syssessions**.  
  
 Quand aucun *session_id* n’est fourni, répertorie des informations sur la session la plus récente.  
  
 Quand aucun *job_name* ou *job_id* n’est fourni, répertorie des informations pour tous les travaux.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Seuls les membres de **sysadmin** peuvent afficher l’activité des travaux appartenant à d’autres utilisateurs.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant répertorie les activités de tous les travaux que l'utilisateur actuel a l'autorisation d'afficher.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobactivity ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Agent des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)  
  
  
