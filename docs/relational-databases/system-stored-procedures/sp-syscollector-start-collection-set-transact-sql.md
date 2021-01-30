---
description: sp_syscollector_start_collection_set (Transact-SQL)
title: sp_syscollector_start_collection_set (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_start_collection_set_TSQL
- sp_syscollector_start_collection_set
dev_langs:
- TSQL
helpviewer_keywords:
- data collector [SQL Server], stored procedures
- sp_syscollector_start_collection_set
ms.assetid: d8357180-f51e-4681-99f9-0596fe2d2b53
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 839b9ef5a7d20850d2f8a9658a9c7ba875757ede
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190577"
---
# <a name="sp_syscollector_start_collection_set-transact-sql"></a>sp_syscollector_start_collection_set (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Démarre un jeu d'éléments de collecte si le collecteur est déjà activé et que le jeu d'éléments de collecte n'est pas en cours d'exécution. Si le collecteur n’est pas activé, activez le collecteur en exécutant [sp_syscollector_enable_collector](../../relational-databases/system-stored-procedures/sp-syscollector-enable-collector-transact-sql.md) puis utilisez cette procédure stockée pour démarrer un jeu d’Collections.  

  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syscollector_start_collection_set   
    [ [ @collection_set_id = ] collection_set_id ]  
    , [[ @name = ] 'name' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @collection_set_id = ] collection_set_id` Identificateur local unique pour le jeu d’Collections. *collection_set_id* est de **type int** avec NULL comme valeur par défaut. *collection_set_id* doit avoir une valeur si le *nom* est null.  
  
`[ @name = ] 'name'` Nom du jeu d’entités de collecte. *Name* est de **type sysname** avec NULL comme valeur par défaut. le *nom* doit avoir une valeur si *collection_set_id* a la valeur null.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 sp_syscollector_create_collection_set doit être exécuté dans le contexte de la base de données système msdb et l'Agent SQL Server doit être activé.  
  
 Cette procédure échoue si elle est exécutée sur un jeu d'éléments de collecte sans planification. Si le jeu d’collections n’a pas de planification (car son mode de collecte est défini sur non mis en cache, par exemple), utilisez la procédure stockée [sp_syscollector_run_collection_set](../../relational-databases/system-stored-procedures/sp-syscollector-run-collection-set-transact-sql.md) pour démarrer le jeu d’Collections.  
  
 Cette procédure active les travaux de collecte et de téléchargement pour le jeu d'éléments de collecte spécifié, et démarre immédiatement le travail de l'agent de collecte si le mode de collecte a la valeur 0 (mis en cache). Pour plus d’informations, consultez [sp_syscollector_create_collection_set](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md).  
  
 Si le jeu d'éléments de collecte ne contient aucun élément de collecte, cette opération est sans effet. L'erreur 14685 est retournée en tant qu'avertissement.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe dc_operator pour exécuter cette procédure. Si le jeu d'éléments de collecte n'a pas de compte proxy, l'appartenance au rôle serveur fixe sysadmin est requis.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant démarre un jeu d'éléments de collecte à l'aide de son identificateur.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_start_collection_set @collection_set_id = 1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)   
 [syscollector_collection_sets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collection-sets-transact-sql.md)  
  
  
