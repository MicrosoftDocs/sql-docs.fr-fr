---
description: sp_update_job (Transact-SQL)
title: sp_update_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_job
- sp_update_job_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_job
ms.assetid: cbdfea38-9e42-47f3-8fc8-5978b82e2623
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e559c50589681f7d00b1bd6048b1e23579213bee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184707"
---
# <a name="sp_update_job-transact-sql"></a>sp_update_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie les attributs d'un travail.  
  

  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_update_job [ @job_id =] job_id | [@job_name =] 'job_name'  
     [, [@new_name =] 'new_name' ]   
     [, [@enabled =] enabled ]  
     [, [@description =] 'description' ]   
     [, [@start_step_id =] step_id ]  
     [, [@category_name =] 'category' ]   
     [, [@owner_login_name =] 'login' ]  
     [, [@notify_level_eventlog =] eventlog_level ]  
     [, [@notify_level_email =] email_level ]  
     [, [@notify_level_netsend =] netsend_level ]  
     [, [@notify_level_page =] page_level ]  
     [, [@notify_email_operator_name =] 'operator_name' ]  
     [, [@notify_netsend_operator_name =] 'netsend_operator' ]  
     [, [@notify_page_operator_name =] 'page_operator' ]  
     [, [@delete_level =] delete_level ]   
     [, [@automatic_post =] automatic_post ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_id = ] job_id` Numéro d’identification du travail à mettre à jour. *job_id* est de type **uniqueidentifier**.  
  
`[ @job_name = ] 'job_name'` Nom du travail. *job_name* est **de type nvarchar (128)**.  
  
> **Remarque :** *Job_id* ou *job_name* doivent être spécifiés, mais ils ne peuvent pas être spécifiés.  
  
`[ @new_name = ] 'new_name'` Nouveau nom du travail. *new_name* est **de type nvarchar (128)**.  
  
`[ @enabled = ] enabled` Spécifie si le travail est activé (**1**) ou désactivé (**0**). *Enabled* est de **type tinyint**.  
  
`[ @description = ] 'description'` Description du travail. *Description* est **de type nvarchar (512)**.  
  
`[ @start_step_id = ] step_id` Numéro d’identification de la première étape à exécuter pour le travail. *step_id* est de **type int**.  
  
`[ @category_name = ] 'category'` Catégorie du travail. la *catégorie* est **nvarchar (128)**.  
  
`[ @owner_login_name = ] 'login'` Nom de la connexion propriétaire du travail. la *connexion* est **nvarchar (128)** seuls les membres du rôle serveur fixe **sysadmin** peuvent modifier la propriété du travail.  
  
`[ @notify_level_eventlog = ] eventlog_level` Spécifie à quel moment placer une entrée dans le journal des applications Microsoft Windows pour ce travail. *eventlog_level* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description (action)|  
|-----------|----------------------------|  
|**0**|Jamais|  
|**1**|Si succès|  
|**2**|Si échec|  
|**3**|Toujours|  
  
`[ @notify_level_email = ] email_level` Spécifie à quel moment envoyer un message électronique à la fin de ce travail. *email_level* est de **type int**. *email_level* utilise les mêmes valeurs que *eventlog_level*.  
  
`[ @notify_level_netsend = ] netsend_level` Spécifie à quel moment envoyer un message réseau à la fin de ce travail. *netsend_level* est de **type int**. *netsend_level* utilise les mêmes valeurs que *eventlog_level*.  
  
`[ @notify_level_page = ] page_level` Spécifie à quel moment envoyer une page à la fin de ce travail. *page_level* est de **type int**. *page_level* utilise les mêmes valeurs que *eventlog_level*.  
  
`[ @notify_email_operator_name = ] 'operator_name'` Nom de l’opérateur auquel le message électronique est envoyé lorsque *email_level* est atteint. *email_name* est **de type nvarchar (128)**.  
  
`[ @notify_netsend_operator_name = ] 'netsend_operator'` Nom de l’opérateur auquel le message réseau est envoyé. *netsend_operator* est **de type nvarchar (128)**.  
  
`[ @notify_page_operator_name = ] 'page_operator'` Nom de l’opérateur auquel une page est envoyée. *page_operator* est **de type nvarchar (128)**.  
  
`[ @delete_level = ] delete_level` Spécifie à quel moment supprimer le travail. *delete_value* est de **type int**. *delete_level* utilise les mêmes valeurs que *eventlog_level*.  
  
`[ @automatic_post = ] automatic_post` Réservé.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_update_job** doit être exécuté à partir de la base de données **msdb** .  
  
 **sp_update_job** modifie uniquement les paramètres pour lesquels des valeurs de paramètre sont fournies. Si un paramètre est manquant, la valeur actuelle est retenue.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Seuls les membres de **sysadmin** peuvent utiliser cette procédure stockée pour modifier les attributs des tâches appartenant à d’autres utilisateurs.  
  
## <a name="examples"></a>Exemples  
 Cet exemple modifie le nom, la description et l'état (activé ou non) du travail `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_job  
    @job_name = N'NightlyBackups',  
    @new_name = N'NightlyBackups -- Disabled',  
    @description = N'Nightly backups disabled during server migration.',  
    @enabled = 0 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)   
 [sp_delete_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_help_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
