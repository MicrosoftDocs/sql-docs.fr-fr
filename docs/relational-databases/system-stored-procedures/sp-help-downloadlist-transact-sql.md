---
description: sp_help_downloadlist (Transact-SQL)
title: sp_help_downloadlist (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_downloadlist_TSQL
- sp_help_downloadlist
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_downloadlist
ms.assetid: 745b265b-86e8-4399-b928-c6969ca1a2c8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6f60b3da9c05d38b60573aa272c33a7cb6915116
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208917"
---
# <a name="sp_help_downloadlist-transact-sql"></a>sp_help_downloadlist (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Répertorie toutes les lignes de la table système **sysdownloadlist** pour le travail fourni, ou toutes les lignes si aucun travail n’est spécifié.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_downloadlist { [ @job_id = ] job_id | [ @job_name = ] 'job_name' }   
     [ , [ @operation = ] 'operation' ]   
     [ , [ @object_type = ] 'object_type' ]   
     [ , [ @object_name = ] 'object_name' ]   
     [ , [ @target_server = ] 'target_server' ]   
     [ , [ @has_error = ] has_error ]   
     [ , [ @status = ] status ]   
     [ , [ @date_posted = ] date_posted ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_id = ] job_id` Numéro d’identification du travail pour lequel des informations doivent être retournées. *job_id* est de type **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
`[ @job_name = ] 'job_name'` Nom du travail. *job_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *Job_id* ou *job_name* doivent être spécifiés, mais ne peuvent pas être spécifiés.  
  
`[ @operation = ] 'operation'` Opération valide pour le travail spécifié. *operation* est de type **varchar (64)**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Vice**|Opération de serveur qui demande au serveur cible de se désinscrire du service **SQLServerAgent** maître.|  
|**DELETE**|Opération qui supprime intégralement un travail.|  
|**INSERT**|Opération qui insère un travail ou actualise un travail existant. Cette opération comporte toutes les étapes et planifications du travail, le cas échéant.|  
|**RE-ENLIST**|Opération serveur qui fait renvoyer les informations d'inscription par le serveur cible, y compris la fréquence d'interrogation et le fuseau horaire, au domaine multiserveur. Le serveur cible télécharge également les détails de l' **opérateur MSXOperator** .|  
|**SET-POLL**|Opération de serveur qui définit l'intervalle, en secondes, que doivent respecter les serveurs cibles pour l'interrogation du domaine multiserveur. S’il est spécifié, la *valeur* est interprétée comme la valeur de l’intervalle requis et peut être comprise entre **10** et **28 800**.|  
|**ACTIVER**|Opération de travail qui requiert le début de l'exécution d'un travail.|  
|**ERREUR**|Opération de travail qui nécessite l'interruption de l'exécution d'un travail.|  
|**SYNC-TIME**|Opération de serveur qui commande au serveur cible de synchroniser son horloge système avec le domaine multiserveur. Cette opération étant coûteuse, il est préférable de l'exécuter le plus rarement possible.|  
|**UPDATE**|Opération de travail qui met à jour uniquement les informations **sysjobs** pour un travail, pas les étapes de travail ou les planifications. Est appelé automatiquement par **sp_update_job**.|  
  
`[ @object_type = ] 'object_type'` Type de l’objet pour le travail spécifié. *object_type* est de type **varchar (64)**, avec NULL comme valeur par défaut. *object_type* peut être un travail ou un serveur. Pour plus d’informations sur les valeurs de *object_type* valides, consultez [sp_add_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-category-transact-sql.md).  
  
`[ @object_name = ] 'object_name'` Nom de l’objet. *object_name* est de **type sysname**, avec NULL comme valeur par défaut. Si *object_type* est job, *object_name* est le nom du travail. Si *object_type* est serveur, *object_name* est le nom du serveur.  
  
`[ @target_server = ] 'target_server'` Nom du serveur cible. *target_server* est de type **nvarchar (128)**, avec NULL comme valeur par défaut.  
  
`[ @has_error = ] has_error` Indique si le travail doit accuser réception d’erreurs. *has_error* est de **type tinyint**, avec NULL comme valeur par défaut, ce qui indique qu’aucune erreur ne doit être confirmée. **1** indique que toutes les erreurs doivent être confirmées.  
  
`[ @status = ] status` État du travail. *Status* est de **type tinyint**, avec NULL comme valeur par défaut.  
  
`[ @date_posted = ] date_posted` La date et l’heure auxquelles toutes les entrées effectuées à la date et à l’heure spécifiées doivent être incluses dans le jeu de résultats. *date_posted* est de **type DateTime**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|Numéro d'identification entier unique de l'instruction.|  
|**source_server**|**nvarchar(30)**|Nom de l'ordinateur du serveur qui émet l'instruction. Dans la [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version 7,0, il s’agit toujours du nom d’ordinateur du serveur maître (MSX).|  
|**operation_code**|**nvarchar(4000)**|Code d'opération pour l'instruction.|  
|**object_name**|**sysname**|Objet affecté par l'instruction.|  
|**object_id**|**uniqueidentifier**|Numéro d’identification de l’objet affecté par l’instruction (**job_id** pour un objet de traitement, ou 0x00 pour un objet serveur) ou une valeur de données spécifique à l' **operation_code**.|  
|**target_server**|**nvarchar(30)**|Serveur cible devant télécharger cette instruction.|  
|**error_message**|**nvarchar(1024)**|Message d'erreur (le cas échéant) émis par le serveur cible s'il y a eu un problème lors du traitement de l'instruction.<br /><br /> Remarque : tout message d’erreur bloque tous les téléchargements supplémentaires par le serveur cible.|  
|**date_posted**|**datetime**|Date à laquelle l'instruction a été envoyée à la table.|  
|**date_downloaded**|**datetime**|Date à laquelle l'instruction a été téléchargée par le serveur cible.|  
|**statut**|**tinyint**|État du travail :<br /><br /> **0** = pas encore téléchargé<br /><br /> **1** = téléchargé avec succès.|  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d'exécution de cette procédure sont accordées par défaut aux membres du rôle de serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant répertorie des lignes dans `sysdownloadlist` pour le travail `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_downloadlist  
    @job_name = N'NightlyBackups',  
    @operation = N'UPDATE',   
    @object_type = N'JOB',   
    @object_name = N'NightlyBackups',  
    @target_server = N'SEATTLE2',   
    @has_error = 1,   
    @status = NULL,   
    @date_posted = NULL ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
