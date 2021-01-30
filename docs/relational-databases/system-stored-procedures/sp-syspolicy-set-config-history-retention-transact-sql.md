---
description: sp_syspolicy_set_config_history_retention (Transact-SQL)
title: sp_syspolicy_set_config_history_retention (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_set_config_history_retention_TSQL
- sp_syspolicy_set_config_history_retention
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_set_config_history_retention
ms.assetid: 2574898a-e724-4447-b96c-ff778471339d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a7f5df1b06ee2f0ecacc4f90e732f86f1ef58f08
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203174"
---
# <a name="sp_syspolicy_set_config_history_retention-transact-sql"></a>sp_syspolicy_set_config_history_retention (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Spécifie le nombre de jours pendant lesquels l'historique des évaluations de stratégies doit être conservé pour la Gestion basée sur des stratégies.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syspolicy_set_config_history_retention [ @value = ] value  
```  
  
## <a name="arguments"></a>Arguments  
`[ @value = ] value` Nombre de jours de conservation de l’historique de la gestion basée sur des stratégies. la *valeur* est **SQLVARIANT**.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Vous devez exécuter sp_syspolicy_set_config_history_retention dans le contexte de la base de données système msdb.  
  
 Si la *valeur* est définie sur 0, l’historique n’est pas supprimé automatiquement.  
  
 Pour afficher la valeur actuelle de la rétention de l'historique, exécutez la requête suivante :  
  
```  
SELECT current_value FROM msdb.dbo.syspolicy_configuration  
WHERE name = 'HistoryRetentionInDays'  
```  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Élévation possible des informations d’identification : les utilisateurs du rôle PolicyAdministratorRole peuvent créer des déclencheurs de serveur et planifier des exécutions de stratégie qui peuvent affecter le fonctionnement de l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Par exemple, les utilisateurs du rôle PolicyAdministratorRole peuvent créer une stratégie qui peut empêcher la création de la plupart des objets dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] . En raison de cette élévation possible des informations d’identification, le rôle PolicyAdministratorRole doit être accordé uniquement aux utilisateurs approuvés par le contrôle de la configuration du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant définit la rétention de l'historique des évaluations de stratégies à 28 jours.  
  
```  
EXEC msdb.dbo.sp_syspolicy_set_config_history_retention @value = 28;  
  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de gestion basée sur des stratégies &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-configure-transact-sql.md)  
  
  
