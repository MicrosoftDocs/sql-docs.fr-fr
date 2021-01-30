---
description: sp_syspolicy_add_policy_category (Transact-SQL)
title: sp_syspolicy_add_policy_category (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_add_policy_category
- sp_syspolicy_add_policy_category_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_add_policy_category
ms.assetid: b682fac4-23c6-4662-8d05-c38f3b45507e
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 4f2c1cd70576342c14028c3bb313a7492bdd0391
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201253"
---
# <a name="sp_syspolicy_add_policy_category-transact-sql"></a>sp_syspolicy_add_policy_category (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute une catégorie de stratégie qui peut être utilisée avec la Gestion basée sur des stratégies. Les catégories de stratégie vous permettent d'organiser des stratégies et de définir leur étendue.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syspolicy_add_policy_category [ @name = ] 'name'  
    [ , [ @mandate_database_subscriptions = ] mandate_database_subscriptions ]  
    , [ @policy_category_id = ] policy_category_id OUTPUT  
```  
  
## <a name="arguments"></a>Arguments  
`[ @name = ] 'name'` Nom de la catégorie de stratégie. *Name* est de **type sysname** et est obligatoire. le *nom* ne peut pas être null ou une chaîne vide.  
  
`[ @mandate_database_subscriptions = ] mandate_database_subscriptions` Détermine si l’abonnement à la base de données est autorisé pour la catégorie de stratégie. *mandate_database_subscriptions* est une valeur de **bit** , avec 1 comme valeur par défaut (activé).  
  
`[ @policy_category_id = ] policy_category_id` Identificateur de la catégorie de stratégie. *policy_category_id* est de **type int** et est retourné en tant que output.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Vous devez exécuter sp_syspolicy_add_policy_category dans le contexte de la base de données système msdb.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Élévation possible des informations d’identification : les utilisateurs du rôle PolicyAdministratorRole peuvent créer des déclencheurs de serveur et planifier des exécutions de stratégie qui peuvent affecter le fonctionnement de l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Par exemple, les utilisateurs du rôle PolicyAdministratorRole peuvent créer une stratégie qui peut empêcher la création de la plupart des objets dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] . En raison de cette élévation possible des informations d’identification, le rôle PolicyAdministratorRole doit être accordé uniquement aux utilisateurs approuvés par le contrôle de la configuration du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée une catégorie de stratégie dans laquelle l'abonnement à la catégorie n'est pas autorisé. Cela signifie que des bases de données individuelles peuvent être configurées pour s'abonner aux stratégies de la catégorie ou annuler l'abonnement.  
  
```  
DECLARE @policy_category_id int;  
  
EXEC msdb.dbo.sp_syspolicy_add_policy_category  
  @name = N'Table Naming Policies'  
, @mandate_database_subscriptions = 0  
, @policy_category_id = @policy_category_id OUTPUT;  
  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de gestion basée sur des stratégies &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_add_policy_category_subscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-add-policy-category-subscription-transact-sql.md)   
 [sp_syspolicy_delete_policy_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-delete-policy-category-transact-sql.md)  
  
  
