---
description: sp_help_maintenance_plan (Transact-SQL)
title: sp_help_maintenance_plan (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_maintenance_plan_TSQL
- sp_help_maintenance_plan
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_maintenance_plan
ms.assetid: e972a510-960e-41d6-93c5-c71cd581a585
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4d996179f089b95a3270c9045778afaaf5c13cd9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200020"
---
# <a name="sp_help_maintenance_plan-transact-sql"></a>sp_help_maintenance_plan (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie des informations sur le plan de maintenance spécifié. Si aucun plan n'est spécifié, cette procédure stockée renvoie des informations sur tous les plans de maintenance.  
  
> **Remarque :** Cette procédure stockée est utilisée avec des plans de maintenance de base de données. Cette fonctionnalité a été remplacée par des plans de maintenance qui n'utilisent pas cette procédure stockée. Utilisez cette procédure pour gérer des plans de maintenance de base de données sur des installations qui ont été mises à niveau à partir d'une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_maintenance_plan [ [ @plan_id = ] 'plan_id' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @plan_id = ] 'plan\_id'` Spécifie l’ID de plan du plan de maintenance. *plan_id* est de type **uniqueidentifier**. La valeur par défaut est NULL.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si *plan_id* est spécifié, **sp_help_maintenance_plan** renverra trois tables : plan, base de données et travail.  
  
### <a name="plan-table"></a>Table Plan  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**plan_id**|**uniqueidentifier**|Identificateur du plan de maintenance.|  
|**plan_name**|**sysname**|Nom du plan de maintenance.|  
|**date_created**|**datetime**|Date de création du plan de maintenance.|  
|**propriétaire**|**sysname**|Propriétaire du plan de maintenance.|  
|**max_history_rows**|**int**|Nombre maximal de lignes allouées pour l'enregistrement de l'historique du plan de maintenance dans la table système.|  
|**remote_history_server**|**int**|Nom du serveur distant sur lequel le rapport d’historique a pu être écrit.|  
|**max_remote_history_rows**|**int**|Nombre maximal de lignes allouées dans la table système d'un serveur distant sur lequel le rapport de l'historique peut être écrit.|  
|**user_defined_1**|**int**|La valeur par défaut est NULL.|  
|**user_defined_2**|**nvarchar(100**|La valeur par défaut est NULL.|  
|**user_defined_3**|**datetime**|La valeur par défaut est NULL.|  
|**user_defined_4**|**uniqueidentifier**|La valeur par défaut est NULL.|  
  
### <a name="database-table"></a>Table Database  
  
|Nom de la colonne|Description|  
|-----------------|-----------------|  
|**database_name**|Nom de toutes les bases de données associées au plan de maintenance. *database_name* est de type **sysname**.|  
  
### <a name="job-table"></a>Table Job  
  
|Nom de la colonne|Description|  
|-----------------|-----------------|  
|**job_id**|Identificateur de tous les travaux associés au plan de maintenance. *job_id* est de type **uniqueidentifier**.|  
  
## <a name="remarks"></a>Notes  
 **sp_help_maintenance_plan** se trouve dans la base de données **msdb** .  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_help_maintenance_plan**.  
  
## <a name="examples"></a>Exemples  
 Cet exemple fournit des informations détaillées sur le plan de maintenance FAD6F2AB-3571-11D3-9D4A-00C04FB925FC.  
  
```  
EXECUTE   sp_help_maintenance_plan   
   N'FAD6F2AB-3571-11D3-9D4A-00C04FB925FC';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Plans de maintenance](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [Procédures stockées de plan de maintenance de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-maintenance-plan-stored-procedures-transact-sql.md)  
  
  
