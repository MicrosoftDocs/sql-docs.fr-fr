---
description: sp_syspolicy_delete_policy_execution_history (Transact-SQL)
title: sp_syspolicy_delete_policy_execution_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_delete_policy_execution_history
- sp_syspolicy_delete_policy_execution_history_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_delete_policy_execution_history
ms.assetid: fe651af9-267e-45ec-b4e7-4b0698fb1be3
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 221a295d5c1bcf3b5f8890bca991edc6675e67af
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201213"
---
# <a name="sp_syspolicy_delete_policy_execution_history-transact-sql"></a>sp_syspolicy_delete_policy_execution_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime l'historique d'exécution pour les stratégies dans la Gestion basée sur des stratégies. Vous pouvez utiliser cette procédure stockée pour supprimer l'historique d'exécution pour une stratégie particulière ou pour toutes les stratégies, et pour supprimer l'historique d'exécution avant une date spécifique.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syspolicy_delete_policy_execution_history [ @policy_id = ] policy_id ]  
    [ , [ @oldest_date = ] 'oldest_date' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @policy_id = ] policy_id` Identificateur de la stratégie pour laquelle vous voulez supprimer l’historique d’exécution. *policy_id* est de **type int** et est obligatoire. Sa valeur peut être NULL.  
  
`[ @oldest_date = ] 'oldest_date'` Date la plus ancienne pour laquelle vous souhaitez conserver l’historique d’exécution de la stratégie. Tout historique d'exécution antérieur à cette date est supprimé. *oldest_date* est de **type DateTime** et est obligatoire. Sa valeur peut être NULL.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Vous devez exécuter sp_syspolicy_delete_policy_execution_history dans le contexte de la base de données système msdb.  
  
 Pour obtenir des valeurs pour *policy_id*, et pour afficher les dates de l’historique d’exécution, vous pouvez utiliser la requête suivante :  
  
```  
SELECT a.name AS N'policy_name', b.policy_id, b.start_date, b.end_date  
FROM msdb.dbo.syspolicy_policies AS a   
INNER JOIN msdb.dbo.syspolicy_policy_execution_history AS b  
ON a.policy_id = b.policy_id  
```  
  
 Le comportement suivant s'applique si vous spécifiez Null pour l'une des deux valeurs suivantes, ou les deux :  
  
-   Pour supprimer tous les historiques d’exécution de stratégie, spécifiez NULL pour les *policy_id* et pour *oldest_date*.  
  
-   Pour supprimer l’historique d’exécution de la stratégie pour une stratégie spécifique, spécifiez un identificateur de stratégie pour *policy_id* et spécifiez NULL comme *oldest_date*.  
  
-   Pour supprimer l’historique d’exécution de la stratégie pour toutes les stratégies avant une date spécifique, spécifiez NULL pour *policy_id* et spécifiez une date pour *oldest_date*.  
  
 Pour archiver l'historique d'exécution de la stratégie, vous pouvez ouvrir le journal Historique de la stratégie dans l'Explorateur d'objets et exporter l'historique d'exécution dans un fichier. Pour accéder au Journal de l’historique des stratégies, développez **gestion**, cliquez avec le bouton droit sur **gestion des stratégies**, puis cliquez sur **afficher l’historique**.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Élévation possible des informations d’identification : les utilisateurs du rôle PolicyAdministratorRole peuvent créer des déclencheurs de serveur et planifier des exécutions de stratégie qui peuvent affecter le fonctionnement de l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Par exemple, les utilisateurs du rôle PolicyAdministratorRole peuvent créer une stratégie qui peut empêcher la création de la plupart des objets dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] . En raison de cette élévation possible des informations d’identification, le rôle PolicyAdministratorRole doit être accordé uniquement aux utilisateurs approuvés par le contrôle de la configuration du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime l'historique d'exécution de la stratégie avant une date spécifique pour une stratégie ayant un ID égal à 7.  
  
```  
EXEC msdb.dbo.sp_syspolicy_delete_policy_execution_history @policy_id = 7  
, @oldest_date = '2009-02-16 16:00:00.000';  
  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de gestion basée sur des stratégies &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_set_config_history_retention &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-set-config-history-retention-transact-sql.md)   
 [sp_syspolicy_purge_history &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-purge-history-transact-sql.md)  
  
  
