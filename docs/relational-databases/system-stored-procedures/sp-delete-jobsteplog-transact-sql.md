---
description: sp_delete_jobsteplog (Transact-SQL)
title: sp_delete_jobsteplog (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_jobsteplog
- sp_delete_jobsteplog_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_jobsteplog
ms.assetid: e9ef4c99-abde-4038-b6a3-a25dcbaf0958
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 17cdcb1ba86e214ce707ff084a0d921b3817cbe0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211146"
---
# <a name="sp_delete_jobsteplog-transact-sql"></a>sp_delete_jobsteplog (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime tous les journaux d'étapes de travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui sont spécifiés via les arguments. Utilisez cette procédure stockée pour tenir à jour la table **sysjobstepslogs** dans la base de données **msdb** .  
  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_delete_jobsteplog { [ @job_id = ] 'job_id' | [ @job_name = ] 'job_name' }  
       [ , [ @step_id = ] step_id | [ @step_name = ] 'step_name' ]  
       [ , [ @older_than = ] 'date' ]  
       [ , [ @larger_than = ] 'size_in_bytes' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_id = ] 'job_id'` Numéro d’identification du travail qui contient le journal d’étapes de travail à supprimer. *job_id* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @job_name = ] 'job_name'` Nom du travail. *job_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> **Remarque :** *Job_id* ou *job_name* doivent être spécifiés, mais ne peuvent pas être spécifiés.  
  
`[ @step_id = ] step_id` Numéro d’identification de l’étape du travail pour laquelle le journal d’étapes de travail doit être supprimé. Si ce n’est pas le cas, tous les journaux d’étapes de travail du travail sont supprimés, sauf si **\@ older_than** ou **\@ larger_than** sont spécifiés. *step_id* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @step_name = ] 'step_name'` Nom de l’étape du travail pour laquelle le journal d’étapes de travail doit être supprimé. *step_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> **Remarque :** *Step_id* ou *step_name* peuvent être spécifiés, mais ils ne peuvent pas être spécifiés.  
  
`[ @older_than = ] 'date'` Date et heure du journal d’étapes de travail le plus ancien que vous souhaitez conserver. Tous les journaux d'étapes de travail antérieurs à cette date/heure sont supprimés. *Date* est de **type DateTime**, avec NULL comme valeur par défaut. **\@ Older_than** et **\@ larger_than** peuvent être spécifiés.  
  
`[ @larger_than = ] 'size_in_bytes'` Taille en octets du journal d’étapes de travail le plus volumineux que vous souhaitez conserver. Tous les journaux d'étapes de travail dont la taille est supérieure à celle spécifiée sont supprimés. **\@ Larger_than** et **\@ older_than** peuvent être spécifiés.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 **sp_delete_jobsteplog** se trouve dans la base de données **msdb** .  
  
 Si aucun argument n’est spécifié, à l’exception de **\@ job_id** ou **\@ job_name** , tous les journaux d’étapes de travail du travail spécifié sont supprimés.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Seuls les membres de **sysadmin** peuvent supprimer un journal d’étapes de travail appartenant à un autre utilisateur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-removing-all-job-step-logs-from-a-job"></a>R. Suppression de tous les journaux d'étapes d'un travail particulier  
 L'exemple suivant montre la suppression de tous les journaux d'étapes du travail `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobsteplog  
    @job_name = N'Weekly Sales Data Backup';  
GO  
```  
  
### <a name="b-removing-the-job-step-log-for-a-particular-job-step"></a>B. Suppression du journal d'étapes de travail pour une étape particulière  
 L'exemple suivant montre la suppression du journal d'étapes de travail pour l'étape 2 du travail `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobsteplog  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 2;  
GO  
```  
  
### <a name="c-removing-all-job-step-logs-based-on-age-and-size"></a>C. Suppression de tous les journaux d'étapes de travail en fonction de l'ancienneté et de la taille.  
 L'exemple suivant montre la suppression de tous les journaux d'étapes de travail créés avant le 25 octobre 2005 à midi et dont la taille est supérieure à 100 mégaoctets (Mo) pour le travail `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobsteplog  
    @job_name = N'Weekly Sales Data Backup',  
    @older_than = '10/25/2005 12:00:00',  
    @larger_than = 104857600;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_help_jobsteplog &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobsteplog-transact-sql.md)   
 [SQL Server Agent des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)  
  
  
