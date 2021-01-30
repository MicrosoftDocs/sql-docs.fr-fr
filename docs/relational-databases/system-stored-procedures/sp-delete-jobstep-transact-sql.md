---
description: sp_delete_jobstep (Transact-SQL)
title: sp_delete_jobstep (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_jobstep
- sp_delete_jobstep_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_jobstep
ms.assetid: 421ede8e-ad57-474a-9fb9-92f70a3e77e3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9b795b6526941518e476390dba158f56e18040da
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186330"
---
# <a name="sp_delete_jobstep-transact-sql"></a>sp_delete_jobstep (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime une étape d'un travail.  
  
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_delete_jobstep { [ @job_id = ] job_id | [ @job_name = ] 'job_name' } ,   
     [ @step_id = ] step_id   
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_id = ] job_id` Numéro d’identification du travail à partir duquel l’étape sera supprimée. *job_id* est de type **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
`[ @job_name = ] 'job_name'` Nom du travail à partir duquel l’étape sera supprimée. *job_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> **Remarque :** *Job_id* ou *job_name* doit être spécifié ; les deux ne peuvent pas être spécifiés.  
  
`[ @step_id = ] step_id` Numéro d’identification de l’étape en cours de suppression. *step_id* est de **type int**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Si vous supprimez une étape de travail, les autres étapes contenant une référence à l'étape supprimée sont automatiquement mises à jour.  
  
 Pour plus d’informations sur les étapes associées à un travail particulier, exécutez **sp_help_jobstep**.  
  
> **Remarque :** L’appel de **sp_delete_jobstep** avec une valeur *step_id* égale à zéro supprime toutes les étapes de travail du travail.  
  
 Microsoft SQL Server Management Studio est un outil simple, basé sur une interface graphique, qui permet de gérer les travaux. Son utilisation est recommandée pour créer et gérer l'infrastructure des travaux.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Seuls les membres de **sysadmin** peuvent supprimer une étape de travail appartenant à un autre utilisateur.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime l'étape `1` du travail `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobstep  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher ou modifier les travaux](../../ssms/agent/view-or-modify-jobs.md)   
 [sp_add_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_update_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md)   
 [sp_help_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
