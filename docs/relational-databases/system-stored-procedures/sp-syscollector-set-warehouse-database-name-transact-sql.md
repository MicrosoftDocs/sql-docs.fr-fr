---
description: sp_syscollector_set_warehouse_database_name (Transact-SQL)
title: sp_syscollector_set_warehouse_database_name (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_set_warehouse_database_name
- sp_syscollector_set_warehouse_database_name_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_set_warehouse_database_name
- data collector [SQL Server], stored procedures
ms.assetid: a85aca1b-8135-4c81-9a05-da5aec76f1ed
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 928e72b551a976c3b51d9260248e02a4963f4052
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159529"
---
# <a name="sp_syscollector_set_warehouse_database_name-transact-sql"></a>sp_syscollector_set_warehouse_database_name (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Spécifie le nom de la base de données défini dans la chaîne de connexion utilisée pour la connexion à l'entrepôt de données de gestion.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syscollector_set_warehouse_database_name [ @database_name = ] 'database_name'  
```  
  
## <a name="arguments"></a>Arguments  
 [ @database_name =] '*database_name*'  
 Nom de l'entrepôt de données de gestion. *database_name* est de **type sysname** avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Vous devez désactiver le collecteur de données avant de modifier sa configuration. Cette procédure échoue si le collecteur de données est activé.  
  
 Pour afficher le nom de la base de données actuelle, interrogez la vue système [syscollector_config_store](../../relational-databases/system-catalog-views/syscollector-config-store-transact-sql.md) .  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle de base de données fixe dc_admin (avec autorisation EXECUTE) pour exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant définit le nom de l'entrepôt de données de gestion sur `RemoteMDW`.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_set_warehouse_database_name N'RemoteMDW';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
