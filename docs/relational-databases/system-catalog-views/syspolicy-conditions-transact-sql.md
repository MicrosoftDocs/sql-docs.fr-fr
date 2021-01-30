---
description: syspolicy_conditions (Transact-SQL)
title: syspolicy_conditions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syspolicy_conditions
- syspolicy_conditions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syspolicy_conditions view
ms.assetid: af97d26c-4bd5-4b08-be51-8419e3b2832c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e51c0d002d697898f4ac5d47c32cfded8235db35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199674"
---
# <a name="syspolicy_conditions-transact-sql"></a>syspolicy_conditions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche une ligne pour chaque condition de la gestion basée sur des stratégies dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . syspolicy_conditions appartient au schéma dbo dans la base de données msdb. Le tableau suivant décrit les colonnes dans la vue syspolicy_conditions.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|condition_id|**int**|Identificateur de cette condition. Chaque condition représente une collection d'une ou de plusieurs expressions de condition.|  
|name|**sysname**|Nom de la condition.|  
|date_created|**datetime**|Date et heure de création de la condition.|  
|description|**nvarchar(max)**|Description de la condition. La colonne de description est facultative et peut être NULL.|  
|created_by|**sysname**|Connexion qui a créé la condition.|  
|modified_by|**sysname**|Connexion qui a récemment modifié la condition. Est NULL si jamais modifiée.|  
|date_modified|**datetime**|Date et heure de création de la condition. Est NULL si jamais modifiée.|  
|is_name_condition|**smallint**|Spécifie si la condition est une condition d'affectation de noms.<br /><br /> 0 = l'expression de condition ne contient pas la variable @Name.<br /><br /> 1 = l'expression de condition contient la variable @Name.|  
|facet|**nvarchar(max)**|Nom de la facette sur laquelle est basée la condition.|  
|Expression|**nvarchar(max)**|Expression des états de la facette.|  
|obj_name|**sysname**|Nom d'objet affecté à @Name si l'expression de condition contient cette variable.|  
  
## <a name="remarks"></a>Notes  
 Lorsque vous dépannez la Gestion basée sur des stratégies, interrogez la vue syspolicy_conditions pour déterminer qui a créé ou modifié en dernier la condition.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle PolicyAdministratorRole dans la base de données msdb.  
  
## <a name="see-also"></a>Voir aussi  
 [Administrer des serveurs à l'aide de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)   
 [Vues de la Gestion basée sur des stratégies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)  
  
  
