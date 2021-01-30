---
description: sp_syscollector_stop_collection_set (Transact-SQL)
title: sp_syscollector_stop_collection_set (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_stop_collection_set_TSQL
- sp_syscollector_stop_collection_set
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_stop_collection_set
- data collector [SQL Server], stored procedures
ms.assetid: 4668cfb7-462f-40d0-948c-8f740a792a4d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 335e4966896ef6e08cdec78612caa2a53b3641b7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159489"
---
# <a name="sp_syscollector_stop_collection_set-transact-sql"></a>sp_syscollector_stop_collection_set (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Arrête un jeu d'éléments de collecte.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syscollector_stop_collection_set   
    [ [ @collection_set_id = ] collection_set_id ]  
    , [ [ @name = ] 'name' ]  
    , [ [ @stop_collection_job = ] stop_collection_job ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @collection_set_id =] *collection_set_id*  
 Identificateur local unique pour le jeu d'éléments de collecte. *collection_set_id* est de **type int** avec NULL comme valeur par défaut. *collection_set_id* doit avoir une valeur si le *nom* est null.  
  
 [ @name =] '*nom*'  
 Nom du jeu d’entités de collecte. *Name* est de **type sysname** avec NULL comme valeur par défaut. le *nom* doit avoir une valeur si *collection_set_id* a la valeur null.  
  
 [ @stop_collection_job =] *stop_collection_job*  
 Spécifie que le travail de collecte pour le jeu d'éléments de collecte doit être arrêté s'il est en cours d'exécution. *stop_collection_job* est de **bits** avec 1 comme valeur par défaut.  
  
 *stop_collection_job* s’applique uniquement aux jeux d’collections avec le mode de collecte défini sur mis en cache. Pour plus d’informations, consultez [sp_syscollector_create_collection_set &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md).  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 sp_syscollector_create_collection_set doit être exécuté dans le contexte de la base de données système msdb.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle de base de données fixe dc_operator (avec autorisation EXECUTE) pour exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant arrête un jeu d'éléments de collecte à l'aide de son identificateur.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_stop_collection_set @collection_set_id = 1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)   
 [Procédures stockées du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)  
  
  
