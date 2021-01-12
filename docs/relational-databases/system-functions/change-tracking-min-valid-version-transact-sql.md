---
description: CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL)
title: CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- CHANGE_TRACKING_CLEANUP_VERSION
- CHANGE_TRACKING_CLEANUP_VERSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CHANGE_TRACKING_MIN_VALID_VERSION
- change tracking [SQL Server], CHANGE_TRACKING_MIN_VALID_VERSION
ms.assetid: 5a43d23f-adcf-4c0b-95ad-07cee03c1f9d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 81e386ef391283f3a3f3095b7dd321055a676332
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093897"
---
# <a name="change_tracking_min_valid_version-transact-sql"></a>CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne la version minimale sur le client qui peut être utilisée pour obtenir des informations de suivi des modifications à partir de la table spécifiée, lorsque vous utilisez la fonction [CHANGETABLE](../../relational-databases/system-functions/changetable-transact-sql.md) .  
    
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CHANGE_TRACKING_MIN_VALID_VERSION ( table_object_id )  
```  
  
## <a name="arguments"></a>Arguments  
 *table_object_id*  
 ID d’objet de la table. *table_object_id* est un **entier**.  
  
## <a name="return-type"></a>Type de retour  
 **bigint**  
  
## <a name="remarks"></a>Notes  
 Utilisez cette fonction pour valider la valeur du paramètre *last_sync_version* pour CHANGETABLE. Si *last_sync_version* est inférieure à la valeur signalée par cette fonction, les résultats retournés à partir d’un appel ultérieur à CHANGETABLE peuvent ne pas être valides.  
  
 CHANGE_TRACKING_MIN_VALID_VERSION utilise les informations suivantes pour déterminer la valeur de retour :  
  
-   Lorsque la table a été activée pour le suivi des modifications.  
  
-   Lorsque la tâche de nettoyage en arrière-plan s'est exécutée pour supprimer des informations de suivi des modifications antérieures à la période de rétention spécifiée pour la base de données.  
  
-   Si la table a été tronquée. Cette instruction supprime toutes les informations de suivi des modifications associées à la table.  
  
 La fonction retourne la valeur NULL si l'une des conditions suivantes est remplie :  
  
-   Le suivi des modifications n'est pas activé pour la base de données.  
  
-   L'ID d'objet de table spécifié n'est pas valide pour la base de données active.  
  
-   Autorisation insuffisante pour la table spécifiée par l'ID d'objet.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant détermine si la version spécifiée est valide. L'exemple obtient la version valide minimale de la table `dbo.Employees`, puis la compare à la valeur de la variable `@last_sync_version`. Si la valeur de `@last_sync_version` est inférieure à la valeur de `@min_valid_version`, la liste des lignes modifiées n'est pas valide.  
  
> [!NOTE]  
>  Généralement, vous pouvez obtenir la valeur à partir d'une table ou d'un autre emplacement dans lequel vous avez stocké le dernier numéro de version utilisé pour synchroniser les données.  
  
```  
-- The tracked change is tagged with the specified context   
DECLARE @min_valid_version bigint, @last_sync_version bigint;  
  
SET @min_valid_version =   
CHANGE_TRACKING_MIN_VALID_VERSION(OBJECT_ID('dbo.Employees'));  
  
SET @last_sync_version = 11  
IF (@last_sync_version < @min_valid_version)  
-- Error � do not obtain changes  
ELSE  
-- Obtain changes using CHANGETABLE(CHANGES ...)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de suivi des modifications &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [sys.change_tracking_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md)  
  
  
