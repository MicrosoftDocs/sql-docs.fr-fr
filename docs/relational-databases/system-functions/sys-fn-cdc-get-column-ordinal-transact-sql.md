---
description: sys.fn_cdc_get_column_ordinal (Transact-SQL)
title: sys.fn_cdc_get_column_ordinal (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/25/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fn_cdc_get_column_ordinal
- fn_cdc_get_column_ordinal_TSQL
- fn_cdc_get_column_ordinal
- sys.fn_cdc_get_column_ordinal_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_get_column_ordinal
- sys.fn_cdc_get_column_ordinal
ms.assetid: 4bb21a57-2b94-4208-8bdf-6a3e2681d881
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4ebab4e1b02b325c5abb7e7b6fbc62b2b970ae50
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094991"
---
# <a name="sysfn_cdc_get_column_ordinal-transact-sql"></a>sys.fn_cdc_get_column_ordinal (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne l’ordinal de colonne de la colonne spécifiée telle qu’elle apparaît dans la [table de modifications](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) associée à l’instance de capture spécifiée.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.fn_cdc_get_column_ordinal ( 'capture_instance','column_name')  
```  
  
## <a name="arguments"></a>Arguments  
 **'** *capture_instance* **'**  
 Nom de l'instance de capture dans laquelle la colonne spécifiée est identifiée comme une colonne capturée. *capture_instance* est de **type sysname**.  
  
 **'** *column_name* **'**  
 Colonne sur laquelle des rapports doivent être effectués. *column_name* est de **type sysname**.  
  
## <a name="return-type"></a>Type de retour  
 **int**  
  
## <a name="remarks"></a>Notes  
 Cette fonction est utilisée pour identifier la position ordinale d'une colonne capturée dans le masque de mise à jour de capture des données modifiées. Il est principalement utilisé conjointement avec la fonction [sys.fn_cdc_is_bit_set](../../relational-databases/system-functions/sys-fn-cdc-is-bit-set-transact-sql.md) pour extraire des informations du masque de mise à jour lors de l’interrogation des données modifiées.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation SELECT sur toutes les colonnes capturées de la table source. Si un rôle de base de données pour le composant de capture des données modifiées est spécifié pour l'instance de capture, l'appartenance à ce rôle est également requise.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant obtient la position ordinale de la colonne `VacationHours` dans le masque de mise à jour pour l'instance de capture `HumanResources_Employee`. Cette valeur est ensuite utilisée dans l'appel à `sys.fn_cdc_is_bit_set` pour extraire des informations du masque de mise à jour retourné.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @from_lsn binary(10), @to_lsn binary(10),  @VacationHoursOrdinal int;  
SET @from_lsn = sys.fn_cdc_get_min_lsn('HumanResources_Employee');  
SET @to_lsn = sys.fn_cdc_get_max_lsn();  
SET @VacationHoursOrdinal = sys.fn_cdc_get_column_ordinal   
    ( 'HumanResources_Employee','VacationHours');  
SELECT *, sys.fn_cdc_is_bit_set(@VacationHoursOrdinal,  
    __$update_mask) as 'VacationHours'  
FROM cdc.fn_cdc_get_net_changes_HumanResources_Employee  
    ( @from_lsn, @to_lsn, 'all with mask');  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de capture de données modifiées &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-data-capture-functions-transact-sql.md)   
 [À propos de la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)   
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [sys.sp_cdc_get_captured_columns &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-captured-columns-transact-sql.md)   
 [sys.fn_cdc_is_bit_set &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-is-bit-set-transact-sql.md)  
  
  
