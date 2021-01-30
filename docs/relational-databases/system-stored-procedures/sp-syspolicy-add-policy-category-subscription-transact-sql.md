---
description: sp_syspolicy_add_policy_category_subscription (Transact-SQL)
title: sp_syspolicy_add_policy_category_subscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_add_policy_category_subscription
- sp_syspolicy_add_policy_category_subscription_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_add_policy_category_subscription
ms.assetid: 4284f550-9a3f-4726-8181-15e407fbf08f
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 279cec943ce0e93556442c3ec29c2139764e078c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195971"
---
# <a name="sp_syspolicy_add_policy_category_subscription-transact-sql"></a>sp_syspolicy_add_policy_category_subscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute un abonnement aux catégories de stratégies à la base de données spécifiée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syspolicy_add_policy_category_subscription [ @target_type = ] 'target_type'  
    , [ @target_object = ] 'target_object'  
    , [ @policy_category = ] 'policy_category'  
    [ , [ @policy_category_subscription_id = ] policy_category_subscription_id OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @target_type = ] 'target_type'` Type de cible de l’abonnement aux catégories. *target_type* est de **type sysname**, est obligatoire et doit avoir la valeur’Database'.  
  
`[ @target_object = ] 'target_object'` Nom de la base de données qui s’abonnera à la catégorie. *target_object* est de **type sysname** et est obligatoire.  
  
`[ @policy_category = ] 'policy_category'` Nom de la catégorie de stratégie à laquelle s’abonner. *policy_category* est de **type sysname** et est obligatoire.  
  
 Pour obtenir des valeurs pour *policy_category*, interrogez la vue système msdb.dbo.syspolicy_policy_categories.  
  
`[ @policy_category_subscription_id = ] policy_category_subscription_id` Identificateur de l’abonnement aux catégories. *policy_category_subscription_id* est de **type int** et est retourné en tant que output.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Vous devez exécuter sp_syspolicy_add_policy_category_subscription dans le contexte de la base de données système msdb.  
  
 Si vous spécifiez une catégorie de stratégie qui n'existe pas, une nouvelle catégorie de stratégie est créée et l'abonnement est autorisé pour toutes les bases de données lorsque vous exécutez la procédure stockée. Si vous supprimez l’abonnement autorisé pour la nouvelle catégorie, il ne s’appliquera qu’à la base de données que vous avez spécifiée en tant que *target_object*. Pour plus d’informations sur la modification d’un paramètre d’abonnement autorisé, consultez [sp_syspolicy_update_policy_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-update-policy-category-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée est exécutée dans le contexte du propriétaire actuel de la procédure stockée.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant configure la base de données spécifiée pour qu'elle s'abonne à une catégorie de stratégie nommée « Table Naming Policies ».  
  
```  
EXEC msdb.dbo.sp_syspolicy_add_policy_category_subscription @target_type = N'DATABASE'  
, @target_object = N'AdventureWorks2012'  
, @policy_category = N'Table Naming Policies';  
  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de gestion basée sur des stratégies &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_update_policy_category_subscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-update-policy-category-subscription-transact-sql.md)   
 [sp_syspolicy_unsubscribe_from_policy_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-unsubscribe-from-policy-category-transact-sql.md)  
  
  
