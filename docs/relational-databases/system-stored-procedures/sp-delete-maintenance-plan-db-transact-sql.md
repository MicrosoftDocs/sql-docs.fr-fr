---
description: sp_delete_maintenance_plan_db (Transact-SQL)
title: sp_delete_maintenance_plan_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_maintenance_plan_db_TSQL
- sp_delete_maintenance_plan_db
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_maintenance_plan_db
- maintenance plans [SQL Server], deleting
- removing maintenance plan
- disassociating maintenance plan
ms.assetid: d1e8afb5-12ee-492b-a770-ba708ed7c8a4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cc76fc7d139da1440f7bf22ca8ab452ff98bc3bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193758"
---
# <a name="sp_delete_maintenance_plan_db-transact-sql"></a>sp_delete_maintenance_plan_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Annule l'association entre le plan de maintenance spécifié et la base de données spécifiée.  
  
> [!NOTE]  
>  Cette procédure stockée s'utilise avec des plans de maintenance de base de données. Cette fonctionnalité a été remplacée par des plans de maintenance qui n'utilisent pas cette procédure stockée. Utilisez cette procédure pour gérer des plans de maintenance de base de données sur des installations qui ont été mises à niveau à partir d'une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_delete_maintenance_plan_db [ @plan_id = ] 'plan_id' ,   
     [ @db_name = ] 'database_name'   
```  
  
## <a name="arguments"></a>Arguments  
`[ @plan_id = ] 'plan\_id'` Spécifie l’ID du plan de maintenance. *plan_id* est de type **uniqueidentifier**.  
  
`[ @db_name = ] 'database\_name'` Spécifie le nom de la base de données à supprimer du plan de maintenance. *database_name* est de type **sysname**.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_delete_maintenance_plan_db** doit être exécuté à partir de la base de données **msdb** .  
  
 La procédure stockée **sp_delete_maintenance_plan_db** supprime l’association entre le plan de maintenance et la base de données spécifiée ; elle ne supprime ni ne détruit la base de données.  
  
 Lorsque **sp_delete_maintenance_plan_db** supprime la dernière base de données du plan de maintenance, la procédure stockée supprime également le plan de maintenance.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_delete_maintenance_plan_db**.  
  
## <a name="examples"></a>Exemples  
 Supprime le plan de maintenance de la base de données **AdventureWorks2012** , précédemment ajouté à l’aide de **sp_add_maintenance_plan_db**.  
  
```  
EXECUTE   sp_delete_maintenance_plan_db N'FAD6F2AB-3571-11D3-9D4A-00C04FB925FC', N'AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Plans de maintenance](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [Procédures stockées de plan de maintenance de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-maintenance-plan-stored-procedures-transact-sql.md)  
  
  
