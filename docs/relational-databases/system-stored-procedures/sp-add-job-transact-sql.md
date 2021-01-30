---
description: sp_add_job (Transact-SQL)
title: sp_add_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_job_TSQL
- sp_add_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_job
ms.assetid: 6ca8fe2c-7b1c-4b59-b4c7-e3b7485df274
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 39f083d083a38e4a581c507354efaf6548471e7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206796"
---
# <a name="sp_add_job-transact-sql"></a>sp_add_job (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Ajoute une nouvelle tâche exécutée par le service SQL Agent.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
 
 > [!IMPORTANT]  
 > Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).
 
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_job [ @job_name = ] 'job_name'  
     [ , [ @enabled = ] enabled ]   
     [ , [ @description = ] 'description' ]   
     [ , [ @start_step_id = ] step_id ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @category_id = ] category_id ]   
     [ , [ @owner_login_name = ] 'login' ]   
     [ , [ @notify_level_eventlog = ] eventlog_level ]   
     [ , [ @notify_level_email = ] email_level ]   
     [ , [ @notify_level_netsend = ] netsend_level ]   
     [ , [ @notify_level_page = ] page_level ]   
     [ , [ @notify_email_operator_name = ] 'email_name' ]   
          [ , [ @notify_netsend_operator_name = ] 'netsend_name' ]   
     [ , [ @notify_page_operator_name = ] 'page_name' ]   
     [ , [ @delete_level = ] delete_level ]   
     [ , [ @job_id = ] job_id OUTPUT ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_name = ] 'job_name'` Nom du travail. Le nom doit être unique et ne peut pas contenir le caractère de pourcentage ( **%** ). *job_name* est de type **nvarchar (128)**, sans valeur par défaut.  
  
`[ @enabled = ] enabled` Indique l’état de la tâche ajoutée. *Enabled* est de **type tinyint**, avec 1 comme valeur par défaut (activé). Si la **valeur est 0**, le travail n’est pas activé et ne s’exécute pas en fonction de sa planification. Toutefois, elle peut être exécutée manuellement.  
  
`[ @description = ] 'description'` Description du travail. *Description* est de type **nvarchar (512)**, avec NULL comme valeur par défaut. Si *Description* est omis, « aucune description n’est disponible » est utilisé.  
  
`[ @start_step_id = ] step_id` Numéro d’identification de la première étape à exécuter pour le travail. *step_id* est de **type int**, avec 1 comme valeur par défaut.  
  
`[ @category_name = ] 'category'` Catégorie du travail. *Category* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @category_id = ] category_id` Mécanisme indépendant du langage permettant de spécifier une catégorie de travaux. *category_id* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @owner_login_name = ] 'login'` Nom de la connexion propriétaire du travail. *login* est de **type sysname**, avec NULL comme valeur par défaut, qui est interprété comme le nom de connexion actuel. Seuls les membres du rôle serveur fixe **sysadmin** peuvent définir ou modifier la valeur de **\@ owner_login_name**. Si les utilisateurs qui ne sont pas membres du rôle **sysadmin** définissent ou modifient la valeur de **\@ owner_login_name**, l’exécution de cette procédure stockée échoue et une erreur est retournée.  
  
`[ @notify_level_eventlog = ] eventlog_level` Valeur indiquant le moment auquel une entrée doit être placée dans le journal des applications Microsoft Windows pour ce travail. *eventlog_level* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Jamais|  
|**1**|Si succès|  
|**2** (par défaut)|Si échec|  
|**3**|Toujours|  
  
`[ @notify_level_email = ] email_level` Valeur qui indique quand envoyer un message électronique à la fin de ce travail. *email_level* est de **type int**, avec **0** comme valeur par défaut, qui indique Never. *email_level* utilise les mêmes valeurs que *eventlog_level*.  
  
`[ @notify_level_netsend = ] netsend_level` Valeur qui indique à quel moment envoyer un message réseau à la fin de ce travail. *netsend_level* est de **type int**, avec **0** comme valeur par défaut, qui indique Never. *netsend_level* utilise les mêmes valeurs que *eventlog_level*.  
  
`[ @notify_level_page = ] page_level` Valeur qui indique à quel moment envoyer une page à la fin de ce travail. *page_level* est de **type int**, avec **0** comme valeur par défaut, qui indique Never. *page_level* utilise les mêmes valeurs que *eventlog_level*.  
  
`[ @notify_email_operator_name = ] 'email_name'` Nom de messagerie électronique de la personne à laquelle envoyer un message électronique lorsque *email_level* est atteint. *email_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @notify_netsend_operator_name = ] 'netsend_name'` Nom de l’opérateur auquel le message réseau est envoyé à l’achèvement de ce travail. *netsend_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @notify_page_operator_name = ] 'page_name'` Nom de la personne à paginer à la fin de ce travail. *page_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @delete_level = ] delete_level` Valeur qui indique à quel moment supprimer le travail. *delete_value* est de **type int**, avec 0 comme valeur par défaut, ce qui signifie Never. *delete_level* utilise les mêmes valeurs que *eventlog_level*.  
  
> [!NOTE]  
>  Lorsque *delete_level* est **3**, le travail n’est exécuté qu’une seule fois, quelle que soit la planification définie pour le travail. De plus, si un travail est supprimé, son historique est également supprimé.  
  
`[ @job_id = ] _job_idOUTPUT` Numéro d’identification du travail affecté au travail, si celui-ci a été créé avec succès. *job_id* est une variable de sortie de type **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 **\@ originating_server** existe dans **sp_add_job,** mais n’est pas listé sous arguments. **\@ originating_server** est réservé à un usage interne.  
  
 Une fois que **sp_add_job** a été exécuté pour ajouter un travail, **sp_add_jobstep** peut être utilisé pour ajouter des étapes qui exécutent les activités du travail. **sp_add_jobschedule** peut être utilisé pour créer la planification que le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] service Agent utilise pour exécuter le travail. Utilisez **sp_add_jobserver** pour définir l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance où le travail s’exécute, et **sp_delete_jobserver** pour supprimer le travail de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance.  
  
 Si le travail s’exécute sur un ou plusieurs serveurs cibles dans un environnement multiserveur, utilisez **sp_apply_job_to_targets** pour définir les serveurs cibles ou les groupes de serveurs cibles pour le travail. Pour supprimer des tâches des serveurs cibles ou des groupes de serveurs cibles, utilisez **sp_remove_job_from_targets**.  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est un outil dont l'interface graphique permet de gérer facilement les travaux. Son utilisation est recommandée pour créer et gérer l'infrastructure des travaux.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter cette procédure stockée, les utilisateurs doivent être membres du rôle serveur fixe **sysadmin** ou disposer de l’un des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rôles de base de données fixes de l’agent suivants, qui résident dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour plus d’informations sur les autorisations spécifiques associées à chacun de ces rôles de base de données fixes, consultez [SQL Server Agent des rôles de base de données fixes](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent définir ou modifier la valeur de **\@ owner_login_name**. Si les utilisateurs qui ne sont pas membres du rôle **sysadmin** définissent ou modifient la valeur de **\@ owner_login_name**, l’exécution de cette procédure stockée échoue et une erreur est retournée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-adding-a-job"></a>R. Ajout d'un travail  
 Cet exemple ajoute un nouveau travail appelé `NightlyBackups` (sauvegardes de nuit).  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_job  
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
### <a name="b-adding-a-job-with-pager-e-mail-and-net-send-information"></a>B. Ajout d'un travail contenant des informations de radiomessagerie, de messagerie électronique et d'envoi réseau  
 Dans l'exemple ci-dessous, un travail appelé `Ad hoc Sales Data Backup` avertit `François Ajenstat` (par radiomessagerie, messagerie électronique ou par message réseau) lorsqu'un travail échoue, et est supprimé une fois exécuté.  
  
> [!NOTE]  
>  Dans cet exemple, nous considérons qu'un opérateur nommé `François Ajenstat` avec une connexion appelée `françoisa` existe déjà.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_job  
    @job_name = N'Ad hoc Sales Data Backup',   
    @enabled = 1,  
    @description = N'Ad hoc backup of sales data',  
    @owner_login_name = N'françoisa',  
    @notify_level_eventlog = 2,  
    @notify_level_email = 2,  
    @notify_level_netsend = 2,  
    @notify_level_page = 2,  
    @notify_email_operator_name = N'François Ajenstat',  
    @notify_netsend_operator_name = N'François Ajenstat',   
    @notify_page_operator_name = N'François Ajenstat',  
    @delete_level = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_add_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_add_jobserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)   
 [sp_apply_job_to_targets &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-apply-job-to-targets-transact-sql.md)   
 [sp_delete_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_delete_jobserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [sp_remove_job_from_targets &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-remove-job-from-targets-transact-sql.md)   
 [sp_help_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_help_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [sp_update_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
