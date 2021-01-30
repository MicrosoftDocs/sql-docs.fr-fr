---
description: syspolicy_policies (Transact-SQL)
title: syspolicy_policies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syspolicy_policies_TSQL
- syspolicy_policies
dev_langs:
- TSQL
helpviewer_keywords:
- syspolicy_policies view
ms.assetid: aecf35bb-187e-4f80-870f-48081b88974e
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8547fa15c966e6835c0b100e628217bc0281991e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192991"
---
# <a name="syspolicy_policies-transact-sql"></a>syspolicy_policies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche une ligne pour chaque stratégie de la gestion basée sur des stratégies dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . syspolicy_policies appartient au schéma dbo dans la base de données msdb. Le tableau suivant décrit les colonnes dans la vue syspolicy_policies.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|policy_id|**int**|Identificateur de la stratégie.|  
|name|**sysname**|Nom de la stratégie.|  
|condition_id|**int**|ID de la condition appliquée ou testée par cette stratégie.|  
|root_condition_id|**int**|À usage interne uniquement.|  
|date_created|**datetime**|Date et heure de création de la stratégie.|  
|execution_mode|**int**|Mode d'évaluation de la stratégie. Les valeurs possibles sont les suivantes :<br /><br /> 0 = À la demande<br /><br /> Ce mode évalue la stratégie lorsqu'elle est spécifiée directement par l'utilisateur.<br /><br /> 1 = Sur modification - Empêcher<br /><br /> Ce mode automatisé utilise des déclencheurs DDL pour empêcher les violations de stratégie.<br /><br /> 2 = Sur modification - Journal uniquement<br /><br /> Ce mode automatisé utilise la notification d'événements pour évaluer une stratégie lorsqu'une modification pertinente se produit et il enregistre les violations de stratégie dans un journal.<br /><br /> 4 = Selon la planification<br /><br /> Ce mode automatisé utilise un travail de l'agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour évaluer périodiquement une stratégie. Il enregistre les violations de stratégie dans un journal.<br /><br /> Remarque : la valeur 3 n’est pas une valeur possible.|  
|policy_category|**int**|ID de la catégorie de la stratégie de la Gestion basée sur des stratégies auquel appartient cette stratégie. Est NULL s'il s'agit du groupe de stratégie par défaut.|  
|schedule_uid|**uniqueidentifier**|Lorsque execution_mode a la valeur Selon la planification, contient l'ID de la planification ; sinon, NULL.|  
|description|**nvarchar(max)**|Description de la stratégie. La colonne de description est facultative et peut être NULL.|  
|help_text|**nvarchar(4000)**|Texte de lien hypertexte qui appartient à help_link.|  
|help_link|**nvarchar (2083)**|Lien hypertexte d'aide supplémentaire attribué à la stratégie par le créateur de stratégie.|  
|object_set_id|**int**|ID du jeu d'objets évalué par la stratégie.|  
|is_enabled|**bit**|Indique si la stratégie est actuellement activée (1) ou désactivée (0).|  
|job_id|**uniqueidentifier**|Lorsque execution_mode a la valeur Selon la planification, contient l'ID du travail de l'agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui exécute la stratégie.|  
|created_by|**sysname**|Connexion qui a créé la stratégie.|  
|modified_by|**sysname**|Connexion qui a récemment modifié la stratégie. Est NULL si jamais modifiée.|  
|date_modified|**datetime**|Date et heure de création de la stratégie. Est NULL si jamais modifiée.|  
  
## <a name="remarks"></a>Notes  
 Lorsque vous dépannez la gestion basée sur des stratégies, interrogez la vue de [syspolicy_conditions](../../relational-databases/system-catalog-views/syspolicy-conditions-transact-sql.md) pour déterminer si la stratégie est activée. Cette vue affiche également qui a créé ou modifié en dernier la stratégie.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle PolicyAdministratorRole dans la base de données msdb.  
  
## <a name="see-also"></a>Voir aussi  
 [Administrer des serveurs à l'aide de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)   
 [Vues de la Gestion basée sur des stratégies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)  
  
  
