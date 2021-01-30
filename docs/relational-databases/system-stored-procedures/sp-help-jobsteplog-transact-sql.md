---
description: sp_help_jobsteplog (Transact-SQL)
title: sp_help_jobsteplog (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobsteplog_TSQL
- sp_help_jobsteplog
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobsteplog
ms.assetid: 1a0be7b1-8f31-4b4c-aadb-586c0e00ed04
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0e5311127f4ff825f862cb3aef2317a2c53578cc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200093"
---
# <a name="sp_help_jobsteplog-transact-sql"></a>sp_help_jobsteplog (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne les métadonnées relatives à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Journal d’étapes de travail de l’agent spécifique. **sp_help_jobsteplog** ne retourne pas le journal réel.  

  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_jobsteplog { [ @job_id = ] 'job_id' | [ @job_name = ] 'job_name' }  
     [ , [ @step_id = ] step_id ]  
     [ , [ @step_name = ] 'step_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_id = ] 'job_id'` Numéro d’identification du travail pour lequel retourner les informations du journal d’étapes de travail. *job_id* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @job_name = ] 'job_name'` Nom du travail. *job_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *Job_id* ou *job_name* doivent être spécifiés, mais ne peuvent pas être spécifiés.  
  
`[ @step_id = ] step_id` Numéro d’identification de l’étape du travail. S'il n'est pas inclus, toutes les étapes du travail sont englobées. *step_id* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @step_name = ] 'step_name'` Nom de l’étape du travail. *step_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|Identificateur unique du travail.|  
|**job_name**|**sysname**|Nom du travail.|  
|**step_id**|**int**|Identificateur de l'étape du travail. Par exemple, si l’étape est la première étape du travail, son *step_id* est 1.|  
|**step_name**|**sysname**|Nom de l’étape du travail.|  
|**step_uid**|**uniqueidentifier**|Identificateur unique de l'étape du travail (généré par le système).|  
|**date_created**|**datetime**|Date de création de l'étape.|  
|**date_modified**|**datetime**|Date de la dernière modification de l'étape.|  
|**log_size**|**float**|Taille du journal d'étapes du travail, en mégaoctets (Mo).|  
|**Sign**|**nvarchar(max)**|Sortie du journal d'étapes du travail.|  
  
## <a name="remarks"></a>Notes  
 **sp_help_jobsteplog** se trouve dans la base de données **msdb** .  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Les membres de **SQLAgentUserRole** peuvent uniquement afficher les métadonnées du journal d’étapes de travail pour les étapes de travail dont ils sont propriétaires.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returns-job-step-log-information-for-all-steps-in-a-specific-job"></a>R. Retourne des informations sur toutes les étapes d'un travail spécifique  
 L'exemple ci-dessous retourne toutes les informations du journal d'étapes du travail `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobsteplog  
    @job_name = N'Weekly Sales Data Backup' ;  
GO  
```  
  
### <a name="b-return-job-step-log-information-about-a-specific-job-step"></a>B. Retourne des informations sur une étape spécifique du travail  
 L'exemple ci-dessous retourne des informations du journal d'étapes concernant la première étape du travail appelé `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobsteplog  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_delete_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_help_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [sp_delete_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_delete_jobsteplog &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobsteplog-transact-sql.md)   
 [SQL Server Agent des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)  
  
  
