---
description: sp_stop_job (Transact-SQL)
title: sp_stop_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_stop_job_TSQL
- sp_stop_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_stop_job
ms.assetid: 64b4cc75-99a0-421e-b418-94e37595bbb0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b7f4c7010ae6b00f083e54bcd3b6ec152d5d66ab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159783"
---
# <a name="sp_stop_job-transact-sql"></a>sp_stop_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Demande à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent d'arrêter l'exécution d'un travail.  

  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_stop_job   
      [@job_name =] 'job_name'  
    | [@job_id =] job_id   
    | [@originating_server =] 'master_server'  
    | [@server_name =] 'target_server'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_name = ] 'job_name'` Nom du travail à arrêter. *job_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @job_id = ] job_id` Numéro d’identification du travail à arrêter. *job_id* est de type **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
`[ @originating_server = ] 'master_server'` Nom du serveur maître. Si ce nom est spécifié, tous les travaux multiserveurs sont arrêtés. *master_server* est de type **nvarchar (128)**, avec NULL comme valeur par défaut. Spécifiez ce paramètre uniquement lors de l’appel de **sp_stop_job** sur un serveur cible.  
  
> [!NOTE]  
>  Seul un des trois premiers paramètres peut être précisé.  
  
`[ @server_name = ] 'target_server'` Nom du serveur cible spécifique sur lequel arrêter un travail multiserveur. *target_server* est de type **nvarchar (128)**, avec NULL comme valeur par défaut. Spécifiez ce paramètre uniquement lors de l’appel de **sp_stop_job** sur un serveur maître pour un travail multiserveur.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 **sp_stop_job** envoie un signal d’arrêt à la base de données. Certains processus peuvent être arrêtés immédiatement et certains doivent atteindre un point stable (ou un point d’entrée vers le chemin de code) avant de pouvoir s’arrêter. Certaines instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] de longue durée telles que BACKUP, RESTORE et certaines commandes DBCC peuvent prendre beaucoup de temps. Lorsqu’ils sont en cours d’exécution, la tâche peut prendre un certain temps avant l’annulation du travail. L'arrêt d'un travail provoque l'enregistrement d'une entrée « Travail annulé » dans l'historique des travaux.  
  
 Si un travail exécute actuellement une étape de type **CmdExec** ou **PowerShell**, le processus en cours d’exécution (par exemple, MyProgram.exe) est forcé à se terminer prématurément. La fin prématurée peut aboutir à un comportement imprévisible ; les fichiers que le processus utilise risquant, par exemple, d'être bloqués alors qu'ils sont ouverts. Par conséquent, **sp_stop_job** doit être utilisé uniquement dans des circonstances extrêmes si le travail contient des étapes de type **CmdExec** ou **PowerShell**.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Les membres de **SQLAgentUserRole** et **SQLAgentReaderRole** peuvent uniquement arrêter les travaux dont ils sont propriétaires. Les membres de **SQLAgentOperatorRole** peuvent arrêter tous les travaux locaux, y compris ceux qui appartiennent à d’autres utilisateurs. Les membres de **sysadmin** peuvent arrêter tous les travaux locaux et multiserveurs.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant arrête un travail nommé `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_stop_job  
    N'Weekly Sales Data Backup' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_delete_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_help_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)   
 [sp_update_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
