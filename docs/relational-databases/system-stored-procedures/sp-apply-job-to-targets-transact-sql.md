---
description: sp_apply_job_to_targets (Transact-SQL)
title: sp_apply_job_to_targets (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_apply_job_to_targets
- sp_apply_job_to_targets_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_apply_job_to_targets
ms.assetid: 4a3e9173-7e3c-4100-a9ac-2f5d2c60a8b0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f9fdc4cffdbe21d1c6c502aa813db55e0444d696
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203233"
---
# <a name="sp_apply_job_to_targets-transact-sql"></a>sp_apply_job_to_targets (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Applique un travail à un ou plusieurs serveurs cibles ou aux serveurs cibles d'un ou de plusieurs groupes de serveurs cibles.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_apply_job_to_targets { [ @job_id = ] job_id | [ @job_name = ] 'job_name' }  
     [ , [ @target_server_groups = ] 'target_server_groups' ]   
     [ , [ @target_servers = ] 'target_servers' ]   
     [ , [ @operation = ] 'operation' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_id = ] job_id` Numéro d’identification du travail à appliquer aux serveurs cibles ou aux groupes de serveurs cibles spécifiés. *job_id* est de type **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
`[ @job_name = ] 'job_name'` Nom du travail à appliquer aux serveurs cibles ou aux groupes de serveurs cibles spécifiés. *job_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *Job_id* ou *job_name* doivent être spécifiés, mais ne peuvent pas être spécifiés.  
  
`[ @target_server_groups = ] 'target_server_groups'` Liste séparée par des virgules des groupes de serveurs cibles auxquels le travail spécifié doit être appliqué. *target_server_groups* est de type **nvarchar (2048)**, avec NULL comme valeur par défaut.  
  
`[ @target_servers = ] 'target_servers'` Liste séparée par des virgules des serveurs cibles auxquels le travail spécifié doit être appliqué. *target_servers* est de type **nvarchar (2048)**, avec NULL comme valeur par défaut.  
  
`[ @operation = ] 'operation'` Indique si le travail spécifié doit être appliqué ou supprimé des serveurs cibles ou des groupes de serveurs cibles spécifiés. *operation* est de type **varchar (7)**, avec Apply comme valeur par défaut. Les opérations valides sont **apply** et **Remove**.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_apply_job_to_targets** offre un moyen facile d’appliquer (ou de supprimer) un travail à partir de plusieurs serveurs cibles et constitue une alternative à l’appel de **sp_add_jobserver** (ou **sp_delete_jobserver**) une fois pour chaque serveur cible requis.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant applique le travail `Backup Customer Information` créé précédemment à tous les serveurs cibles du groupe `Servers Maintaining Customer Information`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_apply_job_to_targets  
    @job_name = N'Backup Customer Information',  
    @target_server_groups = N'Servers Maintaining Customer Information',   
    @operation = N'APPLY' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_jobserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)   
 [sp_delete_jobserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [sp_remove_job_from_targets &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-remove-job-from-targets-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
