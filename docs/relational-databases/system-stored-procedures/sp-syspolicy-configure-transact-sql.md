---
description: sp_syspolicy_configure (Transact-SQL)
title: sp_syspolicy_configure (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_configure
- sp_syspolicy_configure_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_configure
ms.assetid: 70c10922-9345-4190-ba69-808a43f760da
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: abf343b9230e8eeef0ce95ca8e2667c20c714cb8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201248"
---
# <a name="sp_syspolicy_configure-transact-sql"></a>sp_syspolicy_configure (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Configure des paramètres pour la Gestion basée sur des stratégies, tels que l'activation, ou non, de cette dernière.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syspolicy_configure [ @name = ] 'name'  
    , [ @value = ] value  
```  
  
## <a name="arguments"></a>Arguments  
`[ @name = ] 'name'` Est le nom du paramètre que vous souhaitez configurer. *Name* est de **type sysname**, est obligatoire et ne peut pas avoir la valeur null ou être une chaîne vide.  
  
 le *nom* peut être l’une des valeurs suivantes :  
  
-   'Enabled' - Détermine si la Gestion basée sur des stratégies est activée.  
  
-   'HistoryRetentionInDays' - Spécifie le nombre de jours pendant lesquels l'historique des évaluations de stratégies doit être conservé. Si la valeur est 0, l'historique n'est pas supprimé automatiquement.  
  
-   'LogOnSuccess' - Spécifie si la Gestion basée sur des stratégies consigne les évaluations de stratégies réussies.  
  
`[ @value = ] value` Valeur associée à la valeur spécifiée pour le *nom*. la *valeur* est **sql_variant** et est obligatoire.  
  
-   Si vous spécifiez « Enabled » comme *nom*, vous pouvez utiliser l’une des valeurs suivantes :  
  
    -   0 = Désactive la Gestion basée sur des stratégies.  
  
    -   1 = Active la Gestion basée sur des stratégies.  
  
-   Si vous spécifiez « HistoryRententionInDays » comme *nom*, spécifiez le nombre de jours sous la forme d’une valeur entière.  
  
-   Si vous spécifiez « LogOnSuccess » comme *nom*, vous pouvez utiliser l’une des valeurs suivantes :  
  
    -   0 = Consigne uniquement les évaluations de stratégies ayant échoué.  
  
    -   1 = Consigne aussi bien les évaluations de stratégies réussies que celles ayant échoué.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Vous devez exécuter sp_syspolicy_configure dans le contexte de la base de données système msdb.  
  
 Pour afficher les valeurs actuelles de ces paramètres, interrogez la vue système msdb.dbo.syspolicy_configuration.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Élévation possible des informations d’identification : les utilisateurs du rôle PolicyAdministratorRole peuvent créer des déclencheurs de serveur et planifier des exécutions de stratégie qui peuvent affecter le fonctionnement de l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Par exemple, les utilisateurs du rôle PolicyAdministratorRole peuvent créer une stratégie qui peut empêcher la création de la plupart des objets dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] . En raison de cette élévation possible des informations d’identification, le rôle PolicyAdministratorRole doit être accordé uniquement aux utilisateurs approuvés par le contrôle de la configuration du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant active la Gestion basée sur des stratégies.  
  
```  
EXEC msdb.dbo.sp_syspolicy_configure @name = N'Enabled'  
, @value = 1;  
  
GO  
```  
  
 L'exemple suivant définit la rétention de l'historique de la stratégie à 14 jours.  
  
```  
EXEC msdb.dbo.sp_syspolicy_configure @name = N'HistoryRetentionInDays'  
, @value = 14;  
  
GO  
```  
  
 L'exemple suivant configure la Gestion basée sur des stratégies pour qu'aussi bien les évaluations de stratégies réussies que celles ayant échoué soient consignées.  
  
```  
EXEC msdb.dbo.sp_syspolicy_configure @name = N'LogOnSuccess'  
, @value = 1;  
  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de gestion basée sur des stratégies &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_set_config_enabled &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-set-config-enabled-transact-sql.md)   
 [sp_syspolicy_set_config_history_retention &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-set-config-history-retention-transact-sql.md)   
 [sp_syspolicy_set_log_on_success &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-set-log-on-success-transact-sql.md)  
  
  
