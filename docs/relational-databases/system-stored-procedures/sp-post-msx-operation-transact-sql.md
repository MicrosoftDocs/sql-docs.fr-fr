---
description: sp_post_msx_operation (Transact-SQL)
title: sp_post_msx_operation (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_post_msx_operation
- sp_post_msx_operation_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_post_msx_operation
ms.assetid: 085deef8-2709-4da9-bb97-9ab32effdacf
author: markingmyname
ms.author: maghan
ms.openlocfilehash: be9ef622f686044800f634837819b4cba92821a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196039"
---
# <a name="sp_post_msx_operation-transact-sql"></a>sp_post_msx_operation (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Insère des opérations (lignes) dans la table système **sysdownloadlist** pour les serveurs cibles à télécharger et exécuter.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_post_msx_operation  
     [ @operation = ] 'operation'  
     [ , [ @object_type = ] 'object' ]   
     { , [ @job_id = ] job_id }   
     [ , [ @specific_target_server = ] 'target_server' ]   
     [ , [ @value = ] value ]  
     [ , [ @schedule_uid = ] schedule_uid ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @operation = ] 'operation'` Type d’opération de l’opération publiée. *operation* est de type **varchar (64)**, sans valeur par défaut. Les opérations valides dépendent de *object_type*.  
  
|Type d'objet|Opération|  
|-----------------|---------------|  
|**ATTENTE**|INSERT<br /><br /> UPDATE<br /><br /> Suppression<br /><br /> START<br /><br /> STOP|  
|**SERVEURS**|RE-ENLIST<br /><br /> DEFECT<br /><br /> SYNC-TIME<br /><br /> SET-POLL|  
|**PROGRAMMATEUR**|INSERT<br /><br /> UPDATE<br /><br /> Suppression|  
  
`[ @object_type = ] 'object'` Type d’objet pour lequel une opération doit être publiée. Les types valides sont **Job**, **Server** et **Schedule**. l' *objet* est de type **varchar (64)**, avec **Job** comme valeur par défaut.  
  
`[ @job_id = ] job_id` Numéro d’identification du travail auquel l’opération s’applique. *job_id* est de type **uniqueidentifier**, sans valeur par défaut. **0x00** indique tous les travaux. Si l' *objet* est **serveur**, *job_id* n’est pas nécessaire.  
  
`[ @specific_target_server = ] 'target_server'` Nom du serveur cible pour lequel l’opération spécifiée s’applique. Si *job_id* est spécifié, mais que *target_server* n’est pas spécifié, les opérations sont publiées pour tous les serveurs de travail du travail. *target_server* est de type **nvarchar (30)**, avec NULL comme valeur par défaut.  
  
`[ @value = ] value` Intervalle d’interrogation, en secondes. *value* est de type **int**, avec NULL comme valeur par défaut. Spécifiez ce paramètre uniquement si l' *opération* est **Set-Poll**.  
  
`[ @schedule_uid = ] schedule_uid` Identificateur unique de la planification à laquelle l’opération s’applique. *schedule_uid* est de type **uniqueidentifier**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 **sp_post_msx_operation** doit être exécuté à partir de la base de données **msdb** .  
  
 les **sp_post_msx_operation** peuvent toujours être appelées en toute sécurité, car elles déterminent d’abord si le serveur actuel est un agent multiserveur Microsoft SQL Server et, si tel est le cas, si l' *objet* est un travail multiserveur.  
  
 Une fois qu’une opération a été publiée, elle apparaît dans la table **sysdownloadlist** . Après qu'un travail a été créé et publié, les modifications ultérieures apportées à ce travail doivent être aussi communiquées aux serveurs cibles (TSX). L'opération est également réalisée à l'aide de la liste de téléchargement. .  
  
 Il est fortement recommandé de gérer la liste de téléchargement à l'aide de SQL Server Management Studio. Pour plus d’informations, consultez [afficher ou modifier des travaux](../../ssms/agent/view-or-modify-jobs.md).  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter cette procédure stockée, les utilisateurs doivent disposer du rôle serveur fixe **sysadmin** .  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_jobserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)   
 [sp_delete_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_delete_jobserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [sp_delete_targetserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-targetserver-transact-sql.md)   
 [sp_resync_targetserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-resync-targetserver-transact-sql.md)   
 [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)   
 [sp_stop_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-stop-job-transact-sql.md)   
 [sp_update_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [sp_update_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-operator-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
