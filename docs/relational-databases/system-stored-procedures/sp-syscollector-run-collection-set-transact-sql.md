---
description: sp_syscollector_run_collection_set (Transact-SQL)
title: sp_syscollector_run_collection_set (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_run_collection_set_TSQL
- sp_syscollector_run_collection_set
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_run_collection_set
- data collector [SQL Server], stored procedures
ms.assetid: 7bbaee48-dfc7-45c0-b11f-c636b6a7e720
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dba48d2ba3e3945a137df381e4b403ebf1e6dfb8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185263"
---
# <a name="sp_syscollector_run_collection_set-transact-sql"></a>sp_syscollector_run_collection_set (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Démarre un jeu d'éléments de collecte si le collecteur est déjà activé et si le jeu d'éléments de collecte est configuré en mode de collecte sans mise en cache.  
  
> [!NOTE]  
>  Cette procédure échouera si elle est exécutée sur un jeu d'éléments de collecte configuré en mode de collecte mise en cache.  
  
 sp_syscollector_run_collection_set permet à un utilisateur de prendre des instantanés de données à la demande.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syscollector_run_collection_set [[ @collection_set_id = ] collection_set_id ]  
          , [[ @name = ] 'name' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @collection_set_id = ] collection_set_id` Identificateur local unique pour le jeu d’Collections. *collection_set_id* est de **type int** et doit avoir une valeur si *Name* est null.  
  
`[ @name = ] 'name'` Nom du jeu d’entités de collecte. *Name* est de **type sysname** et doit avoir une valeur si *collection_set_id* a la valeur null.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 *Collection_set_id* ou le *nom* doit avoir une valeur, les deux ne peuvent pas être null.  
  
 Cette procédure démarre les travaux de collecte et de téléchargement pour le jeu d’entités de collecte spécifié, et démarre immédiatement le travail de l’agent de collecte si son **\@ collection_mode** défini sur non mis en cache (1). Pour plus d’informations, consultez [sp_syscollector_create_collection_set &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md).  
  
 sp_sycollector_run_collection_set peut également être utilisé pour exécuter un jeu d'éléments de collection sans planification.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données fixe **dc_operator** (avec autorisation EXECUTE) pour exécuter cette procédure.  
  
## <a name="example"></a>Exemple  
 Démarrez un jeu d'éléments de collecte à l'aide de son identificateur.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_run_collection_set @collection_set_id = 1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)  
  
  
