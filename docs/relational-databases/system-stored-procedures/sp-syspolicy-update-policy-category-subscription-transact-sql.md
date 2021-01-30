---
description: sp_syspolicy_update_policy_category_subscription (Transact-SQL)
title: sp_syspolicy_update_policy_category_subscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_update_policy_category_subscription_TSQL
- sp_syspolicy_update_policy_category_subscription
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_update_policy_category_subscription
ms.assetid: d0769566-8f5c-4c8a-84d3-ee17ea6e0cb4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 493da853bc06eb1c56986a99a0b2a2aac9c2e5c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174398"
---
# <a name="sp_syspolicy_update_policy_category_subscription-transact-sql"></a>sp_syspolicy_update_policy_category_subscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Met à jour un abonnement aux catégories de stratégies pour une base de données spécifiée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syspolicy_update_policy_category_subscription [ @policy_category_subscription_id = ] policy_category_subscription_id  
    [ , [ @target_type = ] 'target_type' ]  
    [ , [ @target_object = ] 'target_object' ]  
    , [ @policy_category = ] 'policy_category'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @policy_category_subscription_id = ] policy_category_subscription_id` Est l’identificateur de l’abonnement à la catégorie de stratégie que vous souhaitez mettre à jour. *policy_category_subscription_id* est de **type int** et est obligatoire.  
  
`[ @target_type = ] 'target_type'` Type de cible de l’abonnement aux catégories. *target_type* est de **type sysname**, avec NULL comme valeur par défaut.  
  
 Si vous spécifiez *target_type*, la valeur doit être définie sur « Database ».  
  
`[ @target_object = ] 'target_object'` Nom de la base de données qui s’abonnera à la catégorie de stratégie. *target_object* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @policy_category = ] 'policy_category'` Nom de la catégorie de stratégie à laquelle vous souhaitez que la base de données s’abonne. *policy_category* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Vous devez exécuter sp_syspolicy_update_policy_category_subscription dans le contexte de la base de données système msdb.  
  
 Pour obtenir des valeurs pour *policy_category_subscription_id* et pour *policy_category*, vous pouvez utiliser la requête suivante :  
  
```  
SELECT a.policy_category_subscription_id, a.target_type, a.target_object  
    , b.name AS policy_category  
FROM msdb.dbo.syspolicy_policy_category_subscriptions AS a  
INNER JOIN msdb.dbo.syspolicy_policy_categories AS b  
ON a.policy_category_id = b.policy_category_id;  
```  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Élévation possible des informations d’identification : les utilisateurs du rôle PolicyAdministratorRole peuvent créer des déclencheurs de serveur et planifier des exécutions de stratégie qui peuvent affecter le fonctionnement de l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Par exemple, les utilisateurs du rôle PolicyAdministratorRole peuvent créer une stratégie qui peut empêcher la création de la plupart des objets dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] . En raison de cette élévation possible des informations d’identification, le rôle PolicyAdministratorRole doit être accordé uniquement aux utilisateurs approuvés par le contrôle de la configuration du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant met à jour un abonnement aux catégories de stratégies existant de sorte que la base de données AdventureWorks2012 s'abonne à la catégorie de stratégie « Finance ».  
  
```  
EXEC msdb.dbo.sp_syspolicy_update_policy_category_subscription @policy_category_subscription_id = 1  
, @target_object = 'AdventureWorks2012'  
, @policy_category = 'Finance';  
  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de gestion basée sur des stratégies &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_add_policy_category_subscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-add-policy-category-subscription-transact-sql.md)   
 [sp_syspolicy_delete_policy_category_subscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-delete-policy-category-subscription-transact-sql.md)  
  
  
