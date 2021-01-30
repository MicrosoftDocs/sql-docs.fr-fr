---
description: sp_delete_job (Transact-SQL)
title: sp_delete_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_job
- sp_delete_job_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_job
ms.assetid: b85db6e4-623c-41f1-9643-07e5ea38db09
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ef2fdfc32a0b8b076037b3d75d2c88df9a5883aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186345"
---
# <a name="sp_delete_job-transact-sql"></a>sp_delete_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime un travail.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_delete_job { [ @job_id = ] job_id | [ @job_name = ] 'job_name' } ,  
     [ , [ @originating_server = ] 'server' ]   
     [ , [ @delete_history = ] delete_history ]  
     [ , [ @delete_unused_schedule = ] delete_unused_schedule ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_id = ] job_id` Numéro d’identification du travail à supprimer. *job_id* est de type **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
`[ @job_name = ] 'job_name'` Nom du travail à supprimer. *job_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *Job_id* ou *job_name* doit être spécifié ; les deux ne peuvent pas être spécifiés.  
  
`[ @originating_server = ] 'server'` Pour une utilisation interne.  
  
`[ @delete_history = ] delete_history` Spécifie s’il faut supprimer l’historique du travail. *delete_history* est de **bits**, avec **1** comme valeur par défaut. Lorsque *delete_history* est **1**, l’historique des travaux du travail est supprimé. Lorsque *delete_history* a la **valeur 0**, l’historique des travaux n’est pas supprimé.  
  
 Notez que lorsqu’un travail est supprimé et que l’historique n’est pas supprimé, les informations d’historique du travail ne s’affichent pas dans l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] historique des travaux de l’interface graphique de l’agent, mais les informations se trouvent toujours dans la table **sysjobhistory** de la base de données **msdb** .  
  
`[ @delete_unused_schedule = ] delete_unused_schedule` Spécifie s’il faut supprimer les planifications attachées à ce travail s’ils ne sont attachés à aucun autre travail. *delete_unused_schedule* est de **bits**, avec **1** comme valeur par défaut. Lorsque *delete_unused_schedule* a la valeur **1**, les planifications associées à ce travail sont supprimées si aucune autre tâche ne fait référence à la planification. Lorsque *delete_unused_schedule* a la **valeur 0**, les planifications ne sont pas supprimées.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 L’argument **\@ originating_server** est réservé à un usage interne.  
  
 L’argument **\@ delete_unused_schedule** fournit une compatibilité descendante avec les versions précédentes de SQL Server en supprimant automatiquement les planifications qui ne sont associées à aucun travail. Notez que, par défaut, ce paramètre permet la compatibilité amont. Pour conserver les planifications qui ne sont pas attachées à un travail, vous devez fournir la valeur **0** comme argument **\@ delete_unused_schedule** .  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est un outil dont l'interface graphique permet de gérer facilement les travaux. Son utilisation est recommandée pour créer et gérer l'infrastructure des travaux.  
  
 Cette procédure stockée ne peut pas supprimer les plans de maintenance ni les travaux relevant de plans de maintenance. Pour supprimer les plans de maintenance, vous devez utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_delete_job** pour supprimer un travail. Un utilisateur qui n'est pas membre du rôle serveur fixe **sysadmin** n'a le droit de supprimer que les travaux dont il est propriétaire.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime le travail `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC sp_delete_job  
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)   
 [sp_help_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_update_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
