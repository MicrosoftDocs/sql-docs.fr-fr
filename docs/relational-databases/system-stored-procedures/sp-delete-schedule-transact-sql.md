---
description: sp_delete_schedule (Transact-SQL)
title: sp_delete_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_schedule
- sp_delete_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_schedule
ms.assetid: 18b2c985-47b8-49c8-82d1-8a4af3d7d33a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b152260dca3ab9791124b9f7f1ac13e311f6e514
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199161"
---
# <a name="sp_delete_schedule-transact-sql"></a>sp_delete_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime une planification.  
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_delete_schedule { [ @schedule_id = ] schedule_id | [ @schedule_name = ] 'schedule_name' } ,  
     [ @force_delete = ] force_delete  
```  
  
## <a name="arguments"></a>Arguments  
`[ @schedule_id = ] schedule_id` Numéro d’identification de la planification à supprimer. *schedule_id* est de **type int**, avec NULL comme valeur par défaut.  
  
> **Remarque :** *Schedule_id* ou *schedule_name* doivent être spécifiés, mais ne peuvent pas être spécifiés.  
  
`[ @schedule_name = ] 'schedule_name'` Nom de la planification à supprimer. *schedule_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> **Remarque :** *Schedule_id* ou *schedule_name* doivent être spécifiés, mais ne peuvent pas être spécifiés.  
  
`[ @force_delete = ] force_delete` Spécifie si la procédure doit échouer si la planification est attachée à un travail. *Force_delete* est de bit, avec **0** comme valeur par défaut. Lorsque *force_delete* a la **valeur 0**, la procédure stockée échoue si la planification est attachée à un travail. Lorsque *force_delete* a la valeur **1**, la planification est supprimée, que la planification soit jointe ou non à un travail.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Par défaut, il est impossible de supprimer une planification si elle est attachée à un travail. Pour supprimer une planification attachée à un travail, spécifiez la valeur **1** pour *force_delete*. La suppression d'une planification n'arrête pas les travaux en cours d'exécution.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Notez que le propriétaire du travail peut joindre un travail à une planification et détacher un travail d'une planification sans être le propriétaire de la planification. Toutefois, une planification ne peut pas être supprimée si le détachement le laisse sans travaux, sauf si l’appelant est le propriétaire de la planification.  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Seuls les membres du rôle **sysadmin** peuvent supprimer une planification de travail appartenant à un autre utilisateur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-deleting-a-schedule"></a>R. Suppression d'une planification  
 L'exemple suivant supprime la planification `NightlyJobs`. Si la planification est attachée à un travail, l'exemple ne la supprime pas.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_schedule  
    @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
### <a name="b-deleting-a-schedule-attached-to-a-job"></a>B. Suppression d'une planification attachée à un travail  
 L'exemple suivant supprime la planification `RunOnce`, qu'elle soit ou non attachée à un travail.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_schedule  
    @schedule_name = 'RunOnce',  
    @force_delete = 1;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Implémenter des travaux](../../ssms/agent/implement-jobs.md)   
 [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)  
  
  
