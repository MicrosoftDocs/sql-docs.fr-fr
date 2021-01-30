---
description: sp_syscollector_delete_collection_item (Transact-SQL)
title: sp_syscollector_delete_collection_item (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_delete_collection_item
- sp_syscollector_delete_collection_item_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_delete_collecton_item
- data collector [SQL Server], stored procedures
ms.assetid: 9c2b0990-1d3d-4a59-94a0-3cca6fef4681
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ae900632edd8ccce0e31c87bd7814c41f02da291
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204328"
---
# <a name="sp_syscollector_delete_collection_item-transact-sql"></a>sp_syscollector_delete_collection_item (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime un élément de collecte d’un jeu d’éléments de collecte.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syscollector_delete_collection_item [[ @collection_item_id = ] collection_item_id ]  
    , [[ @name = ] 'name' ]   
```  
  
## <a name="arguments"></a>Arguments  
 [ @collection_item_id =] *collection_item_id*  
 Identificateur unique de l'élément de collecte. *collection_item_id* est de **type int** avec NULL comme valeur par défaut. *collection_item_id* doit avoir une valeur si le *nom* est null.  
  
 [ @name =] '*nom*'  
 Nom de l'élément de collecte. *Name* est de **type sysname** avec NULL comme valeur par défaut. le *nom* doit avoir une valeur si *collection_item_id* a la valeur null.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 sp_syscollector_delete_collection_item doit être exécuté dans le contexte de la base de données système msdb. Il est impossible de supprimer des éléments de collecte de jeux d'éléments de collecte système.  
  
 Le jeu d'éléments de collecte qui contient l'élément de collecte est arrêté puis redémarré au cours de cette opération.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle de base de données fixe dc_admin (avec autorisation EXECUTE) pour exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime un élément de collecte appelé `MyCollectionItem1`.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_delete_collection_item @name = 'MyCollectionItem1';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)   
 [sp_syscollector_create_collection_item &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-item-transact-sql.md)   
 [Procédures stockées du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [syscollector_collection_items &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collection-items-transact-sql.md)  
  
  
