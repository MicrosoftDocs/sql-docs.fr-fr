---
description: sp_syspolicy_set_log_on_success (Transact-SQL)
title: sp_syspolicy_set_log_on_success (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_set_log_on_success_TSQL
- sp_syspolicy_set_log_on_success
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_set_log_on_success
ms.assetid: 6b33383b-5949-488a-a911-59299a270f46
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 268ef1970ba38befb87801cef0098c99f0c9f2bf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161313"
---
# <a name="sp_syspolicy_set_log_on_success-transact-sql"></a>sp_syspolicy_set_log_on_success (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Indique si des évaluations de stratégies réussies sont consignées dans le journal Historique de la stratégie pour la Gestion basée sur des stratégies.  
  
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syspolicy_set_log_on_success [ @value = ] value  
```  
  
## <a name="arguments"></a>Arguments  
`[ @value = ] value` Détermine si les évaluations de stratégie réussies sont journalisées. la *valeur* est **SQLVARIANT** et peut prendre l’une des valeurs suivantes :  
  
-   0 ou 'false' = Les évaluations de stratégies réussies ne sont pas consignées.  
  
-   1 ou 'true' = Des évaluations de stratégies réussies sont consignées.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Vous devez exécuter sp_syspolicy_set_log_on_success dans le contexte de la base de données système msdb.  
  
 Lorsque la *valeur* est définie sur 0 ou sur « false », seules les évaluations de stratégies ayant échoué sont journalisées.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe PolicyAdministratorRole.  
  
> **IMPORTANT** Élévation possible des informations d’identification : les utilisateurs du rôle PolicyAdministratorRole peuvent créer des déclencheurs de serveur et planifier des exécutions de stratégie qui peuvent affecter le fonctionnement de l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Par exemple, les utilisateurs du rôle PolicyAdministratorRole peuvent créer une stratégie qui peut empêcher la création de la plupart des objets dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] . En raison de cette élévation possible des informations d’identification, le rôle PolicyAdministratorRole doit être accordé uniquement aux utilisateurs approuvés par le contrôle de la configuration du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant permet la journalisation des évaluations de stratégies réussies.  
  
```  
EXEC msdb.dbo.sp_syspolicy_set_log_on_success @value = 1;  
  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de gestion basée sur des stratégies &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-configure-transact-sql.md)  
  
  
