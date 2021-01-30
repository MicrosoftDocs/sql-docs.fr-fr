---
description: sp_purge_jobhistory (Transact-SQL)
title: sp_purge_jobhistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_purge_jobhistory_TSQL
- sp_purge_jobhistory
dev_langs:
- TSQL
helpviewer_keywords:
- sp_purge_jobhistory
ms.assetid: 237f9bad-636d-4262-9bfb-66c034a43e88
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 25858da6a748f16071ee9d9693ada310c2ce6170
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189299"
---
# <a name="sp_purge_jobhistory-transact-sql"></a>sp_purge_jobhistory (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Supprime les enregistrements d'historique d'un travail.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_purge_jobhistory   
   {   [ @job_name = ] 'job_name' |   
     | [ @job_id = ] job_id }  
   [ , [ @oldest_date = ] oldest_date ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_name = ] 'job_name'` Nom du travail pour lequel supprimer les enregistrements d’historique. *job_name* est de **type sysname**, avec NULL comme valeur par défaut. *Job_id* ou *job_name* doivent être spécifiés, mais ne peuvent pas être spécifiés.  
  
> [!NOTE]  
>  Les membres du rôle serveur fixe **sysadmin** ou des membres du rôle de base de données fixe **SQLAgentOperatorRole** peuvent exécuter **sp_purge_jobhistory** sans spécifier de *job_name* ou de *job_id*. Lorsque les utilisateurs **sysadmin** ne spécifient pas ces arguments, l’historique des travaux pour tous les travaux locaux et multiserveurs est supprimé dans le délai spécifié par *oldest_date*. Lorsque les utilisateurs **SQLAgentOperatorRole** ne spécifient pas ces arguments, l’historique des travaux de tous les travaux locaux est supprimé dans le délai spécifié par *oldest_date*.  
  
`[ @job_id = ] job_id` Numéro d’identification du travail pour les enregistrements à supprimer. *job_id* est de type **uniqueidentifier**, avec NULL comme valeur par défaut. *Job_id* ou *job_name* doivent être spécifiés, mais ne peuvent pas être spécifiés. Consultez la remarque dans la description de **\@ job_name** pour plus d’informations sur la façon dont les utilisateurs **sysadmin** ou **SQLAgentOperatorRole** peuvent utiliser cet argument.  
  
`[ @oldest_date = ] oldest_date` Enregistrement le plus ancien à conserver dans l’historique. *oldest_date* est de **type DateTime**, avec NULL comme valeur par défaut. Lorsque *oldest_date* est spécifié, **sp_purge_jobhistory** supprime uniquement les enregistrements antérieurs à la valeur spécifiée.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Lorsque **sp_purge_jobhistory** se termine correctement, un message est retourné.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **SQLAgentOperatorRole** peuvent exécuter cette procédure stockée. Les membres de **sysadmin** peuvent purger l’historique des travaux pour tous les travaux locaux et multiserveurs. Les membres de **SQLAgentOperatorRole** peuvent purger l’historique des travaux pour tous les travaux locaux uniquement.  
  
 Les autres utilisateurs, y compris les membres de **SQLAgentUserRole** et des membres de **SQLAgentReaderRole**, doivent disposer explicitement de l’autorisation EXECUTE sur **sp_purge_jobhistory**. Une fois l'autorisation accordée sur cette procédure stockée, ces utilisateurs peuvent uniquement supprimer l'historique des travaux dont ils sont propriétaires.  
  
 Les rôles de base de données fixes **SQLAgentUserRole**, **SQLAgentReaderRole** et **SQLAgentOperatorRole** se trouvent dans la base de données **msdb** . Pour plus d’informations sur leurs autorisations, consultez [SQL Server Agent des rôles de base de données fixes](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-remove-history-for-a-specific-job"></a>R. Suppression de l'historique d'un travail spécifique  
 L'exemple suivant supprime l'historique du travail `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_purge_jobhistory  
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
### <a name="b-remove-history-for-all-jobs"></a>B. Suppression de l'historique de tous les travaux  
  
> [!NOTE]  
>  Seuls les membres du rôle serveur fixe **sysadmin** et les membres du **SQLAgentOperatorRole** peuvent supprimer l’historique de tous les travaux. Lorsque les utilisateurs **sysadmin** exécutent cette procédure stockée sans paramètres, l’historique des travaux de tous les travaux locaux et multiserveurs est purgé. Lorsque les utilisateurs **SQLAgentOperatorRole** exécutent cette procédure stockée sans aucun paramètre, seul l’historique des travaux de tous les travaux locaux est purgé.  
  
 Dans cet exemple, la procédure est exécutée sans paramètres pour supprimer tous les enregistrements d'historique.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_purge_jobhistory ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_help_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_help_jobhistory &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobhistory-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [GRANT – Octroyer des autorisations sur un objet &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)  
  
  
