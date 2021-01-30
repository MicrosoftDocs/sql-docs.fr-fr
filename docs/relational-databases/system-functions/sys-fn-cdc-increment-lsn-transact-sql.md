---
description: sys.fn_cdc_increment_lsn (Transact-SQL)
title: sys.fn_cdc_increment_lsn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_cdc_increment_lsn_TSQL
- sys.fn_cdc_increment_lsn_TSQL
- sys.fn_cdc_increment_lsn
- fn_cdc_increment_lsn
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_increment_lsn
- sys.fn_cdc_increment_lsn
ms.assetid: e53b6703-358b-4c9a-912a-8f7c7331069b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2e20b1d3146ee9e8f08cafdac3d2a4b1771a2081
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194400"
---
# <a name="sysfn_cdc_increment_lsn-transact-sql"></a>sys.fn_cdc_increment_lsn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne le numéro séquentiel dans le journal suivant dans la séquence basée sur le numéro séquentiel dans le journal spécifié.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.fn_cdc_increment_lsn ( lsn_value )  
```  
  
## <a name="arguments"></a>Arguments  
 *lsn_value*  
 Valeur LSN. *lsn_value* est **de type binaire (10)**.  
  
## <a name="return-type"></a>Type de retour  
 **binary(10)**  
  
## <a name="remarks"></a>Notes  
 La valeur LSN retournée par la fonction est toujours supérieure à la valeur spécifiée, et aucune valeur LSN n'existe entre les deux valeurs.  
  
 Pour interroger systématiquement un flux de données de modification dans le temps, vous pouvez répéter périodiquement l'appel de fonction de requête, en spécifiant chaque fois un nouvel intervalle de requête pour délimiter les modifications retournées dans la requête. Pour aider à s'assurer de ne perdre aucune donnée, la limite supérieure de la requête précédente est souvent utilisée pour générer la limite inférieure de la requête suivante. Étant donné que l'intervalle de requête est un intervalle fermé, la nouvelle limite inférieure doit être plus grande que la limite supérieure précédente, mais assez petite pour garantir qu'aucune modification ne comporte de valeur LSN qui se trouve entre cette valeur et l'ancienne limite supérieure. La fonction sys.fn_cdc_increment_lsn est utilisée pour obtenir cette valeur.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données public.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise `sys.fn_cdc_increment_lsn` pour générer une nouvelle valeur de limite inférieure pour une requête de capture des données modifiées basée sur la limite supérieure enregistrée dans la variable `@save_to_lsn` à partir d'une requête précédente.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @from_lsn binary(10), @to_lsn binary(10), @save_to_lsn binary(10);  
SET @save_to_lsn = <previous_upper_bound_value>;  
SET @from_lsn = sys.fn_cdc_increment_lsn(@save_to_lsn);  
SET @to_lsn = sys.fn_cdc_get_max_lsn();  
SELECT * from cdc.fn_cdc_get_all_changes_HumanResources_Employee( @from_lsn, @to_lsn, 'all' );  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.fn_cdc_decrement_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-decrement-lsn-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [À propos de la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
