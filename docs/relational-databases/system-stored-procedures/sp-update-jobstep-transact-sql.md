---
description: sp_update_jobstep (Transact-SQL)
title: sp_update_jobstep (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_jobstep
- sp_update_jobstep_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_jobstep
ms.assetid: e158802c-c347-4a5d-bf75-c03e5ae56e6b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c102cee625bc81611bd67209306d6bc5f9398660
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183571"
---
# <a name="sp_update_jobstep-transact-sql"></a>sp_update_jobstep (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie la valeur d'une étape d'un travail qui est utilisé pour effectuer des activités automatisées.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_update_jobstep   
     {   [@job_id =] job_id   
       | [@job_name =] 'job_name' } ,  
     [@step_id =] step_id  
     [ , [@step_name =] 'step_name' ]  
     [ , [@subsystem =] 'subsystem' ]   
     [ , [@command =] 'command' ]  
     [ , [@additional_parameters =] 'parameters' ]  
     [ , [@cmdexec_success_code =] success_code ]  
     [ , [@on_success_action =] success_action ]   
     [ , [@on_success_step_id =] success_step_id ]  
     [ , [@on_fail_action =] fail_action ]   
     [ , [@on_fail_step_id =] fail_step_id ]  
     [ , [@server =] 'server' ]   
     [ , [@database_name =] 'database' ]  
     [ , [@database_user_name =] 'user' ]   
     [ , [@retry_attempts =] retry_attempts ]  
     [ , [@retry_interval =] retry_interval ]   
     [ , [@os_run_priority =] run_priority ]  
     [ , [@output_file_name =] 'file_name' ]   
     [ , [@flags =] flags ]  
     [ ,  {   [ @proxy_id = ] proxy_id   
            | [ @proxy_name = ] 'proxy_name' }   
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_id = ] job_id` Numéro d’identification du travail auquel l’étape appartient. *job_id* est de type **uniqueidentifier**, avec NULL comme valeur par défaut. *Job_id* ou *job_name* doivent être spécifiés, mais ils ne peuvent pas être spécifiés.  
  
`[ @job_name = ] 'job_name'` Nom du travail auquel l’étape appartient. *job_name* est de **type sysname**, avec NULL comme valeur par défaut. *Job_id* ou *job_name* doivent être spécifiés, mais ils ne peuvent pas être spécifiés.  
  
`[ @step_id = ] step_id` Numéro d’identification de l’étape de travail à modifier. Il est impossible de modifier ce numéro. *step_id* est de **type int**, sans valeur par défaut.  
  
`[ @step_name = ] 'step_name'` Nouveau nom de l’étape. *step_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @subsystem = ] 'subsystem'` Sous-système utilisé par Microsoft SQL Server Agent pour exécuter la *commande*. *Subsystem* est de type **nvarchar (40)**, avec NULL comme valeur par défaut.  
  
`[ @command = ] 'command'` Commande (s) à exécuter par le biais du *sous-système*. la *commande* est de type **nvarchar (max)**, avec NULL comme valeur par défaut.  
  
`[ @additional_parameters = ] 'parameters'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @cmdexec_success_code = ] success_code` La valeur retournée par une commande de sous-système **CmdExec** pour indiquer que la *commande* a été exécutée avec succès. *success_code* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @on_success_action = ] success_action` Action à exécuter si l’étape se déroule correctement. *success_action* est de **type tinyint**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description (action)|  
|-----------|----------------------------|  
|**1**|Quittez avec succès.|  
|**2**|Sortie avec échec|  
|**3**|Passage à l'étape suivante|  
|**4**|Passez à l’étape *success_step_id.*|  
  
`[ @on_success_step_id = ] success_step_id` Numéro d’identification de l’étape de ce travail à exécuter si l’étape se déroule correctement et que *success_action* a la valeur **4**. *success_step_id* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @on_fail_action = ] fail_action` Action à exécuter si l’étape échoue. *fail_action* est de **type tinyint**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description (action)|  
|-----------|----------------------------|  
|**1**|Quittez avec succès.|  
|**2**|Sortie avec échec|  
|**3**|Passage à l'étape suivante|  
|**4**|Passez à l’étape *fail_step_id * *.*|  
  
`[ @on_fail_step_id = ] fail_step_id` Numéro d’identification de l’étape à exécuter dans ce travail si l’étape échoue et si *fail_action* a la valeur **4**. *fail_step_id* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @server = ] 'server'`[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] *Server* est de type **nvarchar (128)**, avec NULL comme valeur par défaut.  
  
`[ @database_name = ] 'database'` Nom de la base de données dans laquelle exécuter une [!INCLUDE[tsql](../../includes/tsql-md.md)] étape. *Database est de* **type sysname**. Les noms placés entre crochets ([ ]) ne sont pas autorisés. La valeur par défaut est NULL.  
  
`[ @database_user_name = ] 'user'` Nom du compte d’utilisateur à utiliser lors de l’exécution d’une [!INCLUDE[tsql](../../includes/tsql-md.md)] étape. *User* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @retry_attempts = ] retry_attempts` Nombre de nouvelles tentatives à utiliser en cas d’échec de cette étape. *retry_attempts* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @retry_interval = ] retry_interval` Durée, en minutes, entre chaque tentative. *retry_interval* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @os_run_priority = ] run_priority` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @output_file_name = ] 'file_name'` Nom du fichier dans lequel la sortie de cette étape est enregistrée. *file_name* est de type **nvarchar (200)**, avec NULL comme valeur par défaut. Ce paramètre est valide uniquement avec les commandes fonctionnant dans les sous-systèmes [!INCLUDE[tsql](../../includes/tsql-md.md)] ou CmdExec.  
  
 Pour affecter la valeur NULL à output_file_name, vous devez affecter à *output_file_name* une chaîne vide (' ') ou à une chaîne de caractères vides, mais vous ne pouvez pas utiliser la fonction **char (32)** . Vous pouvez par exemple définir cet argument avec une chaîne vide comme suit :  
  
 **@output_file_name = ' '**  
  
`[ @flags = ] flags` Option qui contrôle le comportement. *Flags* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0** (valeur par défaut)|Écrasement du fichier de sortie.|  
|**2**|Ajout au fichier de sortie|  
|**4**|Écriture de la sortie de l'étape d'un travail Transact-SQL dans l'historique des étapes.|  
|**8**|Écriture du journal dans la table (remplace l'historique existant)|  
|**16**|Écriture du journal dans la table (s'ajoute à l'historique existant)|  
  
`[ @proxy_id = ] proxy_id` Numéro d’identification du proxy sous lequel l’étape de travail s’exécute. *proxy_id* est de type **int**, avec NULL comme valeur par défaut. Si aucun *proxy_id* n’est spécifié, aucun *proxy_name* n’est spécifié et aucun *user_name* n’est spécifié, l’étape de travail s’exécute en tant que compte de service pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’agent.  
  
`[ @proxy_name = ] 'proxy_name'` Nom du proxy sous lequel l’étape de travail s’exécute. *proxy_name* est de type **sysname**, avec NULL comme valeur par défaut. Si aucun *proxy_id* n’est spécifié, aucun *proxy_name* n’est spécifié et aucun *user_name* n’est spécifié, l’étape de travail s’exécute en tant que compte de service pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’agent.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_update_jobstep** doit être exécuté à partir de la base de données **msdb** .  
  
 La mise à jour de l'étape d'un travail incrémente le numéro de version de ce travail.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Seuls les membres de **sysadmin** peuvent mettre à jour une étape de travail appartenant à un autre utilisateur.  
  
 Si l'étape d'un travail nécessite un accès à un proxy, le créateur de l'étape doit avoir accès au proxy pour l'étape du travail. Tous les sous-systèmes, excepté Transact-SQL, requièrent un compte proxy. Les membres de **sysadmin** ont accès à tous les proxies et peuvent utiliser le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte de service agent pour le proxy.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant modifie le nombre de tentatives de reprises pour la première étape du travail `Weekly Sales Data Backup`. À la fin de l'exemple, ce nombre s'élève à `10`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_jobstep  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1,  
    @retry_attempts = 10 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher ou modifier les travaux](../../ssms/agent/view-or-modify-jobs.md)   
 [sp_delete_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_help_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
