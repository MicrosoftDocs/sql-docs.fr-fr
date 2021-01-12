---
description: CHANGE_TRACKING_IS_COLUMN_IN_MASK (Transact-SQL)
title: CHANGE_TRACKING_IS_COLUMN_IN_MASK (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- CHANGE_TRACKING_IS_COLUMN_IN_MASK_TSQL
- CHANGE_TRACKING_IS_COLUMN_IN_MASK
dev_langs:
- TSQL
helpviewer_keywords:
- change tracking [SQL Server], CHANGE_TRACKING_IS_COLUMN_IN_MASK
- CHANGE_TRACKING_IS_COLUMN_IN_MASK
ms.assetid: 649b370b-da54-4915-919d-1b597a39d505
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a9d5ee6c44e0b0ca9544ee4a32b7cbd455497fe6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095009"
---
# <a name="change_tracking_is_column_in_mask-transact-sql"></a>CHANGE_TRACKING_IS_COLUMN_IN_MASK (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Interprète la valeur SYS_CHANGE_COLUMNS retournée par la fonction CHANGETABLE (CHANGEs...). Cela permet à une application de déterminer si la colonne spécifiée est incluse dans les valeurs retournées pour SYS_CHANGE_COLUMNS.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CHANGE_TRACKING_IS_COLUMN_IN_MASK ( column_id , change_columns )  
```  
  
## <a name="arguments"></a>Arguments  
 *column_id*  
 ID de la colonne en cours de vérification. L’ID de colonne peut être obtenu à l’aide de la fonction [COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md) .  
  
 *change_columns*  
 Données binaires de la colonne SYS_CHANGE_COLUMNS des données [CHANGETABLE](../../relational-databases/system-functions/changetable-transact-sql.md) .  
  
## <a name="return-type"></a>Type de retour  
 **bit**  
  
## <a name="return-values"></a>Valeurs de retour  
 CHANGE_TRACKING_IS_COLUMN_IN_MASK retourne les valeurs suivantes.  
  
|Valeur retournée|Description|  
|------------------|-----------------|  
|0|La colonne spécifiée ne figure pas dans la liste *change_columns* .|  
|1|La colonne spécifiée se trouve dans la liste *change_columns* .|  
  
## <a name="remarks"></a>Notes  
 CHANGE_TRACKING_IS_COLUMN_IN_MASK n’effectue aucune vérification pour valider la valeur de *column_id* ou si le paramètre *change_columns* a été obtenu à partir de la table à partir de laquelle le *column_id* a été obtenu.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant détermine si la colonne `Salary` de la table `Employees` a été mise à jour. La `COLUMNPROPERTY` fonction retourne l’ID de colonne de la `Salary` colonne. La variable locale `@change_columns` doit être définie en fonction des résultats d'une requête en utilisant CHANGETABLE comme source de données.  
  
```sql  
SET @SalaryChanged = CHANGE_TRACKING_IS_COLUMN_IN_MASK  
    (COLUMNPROPERTY(OBJECT_ID('Employees'), 'Salary', 'ColumnId')  
    ,@change_columns);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de suivi des modifications &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [CHANGETABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/changetable-transact-sql.md)   
 [Suivi des modifications de données &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)  
  
  
