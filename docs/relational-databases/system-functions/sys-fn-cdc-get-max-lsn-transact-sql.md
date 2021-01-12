---
description: sys.fn_cdc_get_max_lsn (Transact-SQL)
title: sys.fn_cdc_get_max_lsn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fn_cdc_get_max_lsn
- fn_cdc_get_max_lsn
- fn_cdc_get_max_lsn_TSQL
- sys.fn_cdc_get_max_lsn_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_get_max_lsn
- sys.fn_cdc_get_max_lsn
ms.assetid: 93f3a4c8-b91f-4ebb-8e96-9397bb3a1c43
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 59613b93de2a086bfd4d0bac7fa0f8a8983d8ce5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094987"
---
# <a name="sysfn_cdc_get_max_lsn-transact-sql"></a>sys.fn_cdc_get_max_lsn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne le numéro séquentiel dans le journal (LSN) maximal à partir de la colonne start_lsn de la table système [CDC.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) . Vous pouvez utiliser cette fonction pour retourner le point de terminaison supérieur de la chronologie de capture de données modifiées pour toute instance de capture.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.fn_cdc_get_max_lsn ()  
```  
  
## <a name="return-types"></a>Types de retour  
 **binary(10)**  
  
## <a name="remarks"></a>Notes  
 Cette fonction retourne le LSN maximal dans la colonne start_lsn de la table [CDC.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md) . En tant que tel, il s'agit du dernier numéro séquentiel dans le journal traité par le processus de capture lorsque les modifications sont propagées aux tables de modification de base de données. Il sert également de point de terminaison supérieur pour toutes les chronologies associées aux instances de capture définies pour la base de données.  
  
 La fonction est généralement utilisée pour obtenir un point de terminaison supérieur approprié pour un intervalle de requête.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données public.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-the-maximum-lsn-value"></a>R. Retour de la valeur LSN maximale  
 L'exemple suivant retourne le numéro séquentiel dans le journal maximal pour toutes les instances de capture dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
SELECT sys.fn_cdc_get_max_lsn()AS max_lsn;  
```  
  
### <a name="b-setting-the-high-endpoint-of-a-query-range"></a>B. Définition du point de terminaison supérieur d'une plage de requêtes  
 L'exemple suivant utilise le numéro séquentiel dans le journal maximal retourné par `sys.fn_cdc_get_max_lsn` pour définir le point de terminaison supérieur d'une plage de requêtes pour l'instance de capture `HumanResources_Employee`.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @from_lsn binary(10), @to_lsn binary(10);  
SET @from_lsn = sys.fn_cdc_get_min_lsn(N'HumanResources_Employee');  
SET @to_lsn = sys.fn_cdc_get_max_lsn();  
SELECT * FROM cdc.fn_cdc_get_all_changes_HumanResources_Employee(@from_lsn, @to_lsn, 'all');  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.fn_cdc_get_min_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-get-min-lsn-transact-sql.md)   
 [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)  
  
  
