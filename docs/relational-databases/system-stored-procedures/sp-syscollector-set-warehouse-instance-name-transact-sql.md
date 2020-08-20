---
description: sp_syscollector_set_warehouse_instance_name (Transact-SQL)
title: sp_syscollector_set_warehouse_instance_name (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_syscollector_set_warehouse_instance_name_TSQL
- sp_syscollector_set_warehouse_instance_name
dev_langs:
- TSQL
helpviewer_keywords:
- data collector [SQL Server], stored procedures
- sp_syscollector_set_warehouse_instance_name
ms.assetid: 5320fcd4-bed1-468f-b784-a5e10fcfaeb6
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 86816adb5b3683aa6cbcd5c869b233960cf1a68f
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88492989"
---
# <a name="sp_syscollector_set_warehouse_instance_name-transact-sql"></a>sp_syscollector_set_warehouse_instance_name (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Spécifie le nom de l'instance pour la chaîne de connexion utilisée pour la connexion à l'entrepôt de données de gestion.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syscollector_set_warehouse_instance_name [ @instance_name = ] 'instance_name'  
```  
  
## <a name="arguments"></a>Arguments  
 [ @instance_name =] '*instance_name*'  
 Est le nom de l’instance. *instance_name* est de **type sysname** et sa valeur par défaut est l’instance locale si la valeur est null.  
  
> **Remarque :**_instance_name_ doit être le nom complet de l’instance, qui se compose du nom de l’ordinateur et du nom de l’instance, sous la forme *ComputerName* \\ *nom_instance*.    
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Vous devez désactiver le collecteur de données avant de modifier cette configuration. Cette procédure échoue si le collecteur de données est activé.  
  
 Pour afficher le nom de l’instance actuelle, interrogez la vue système [syscollector_config_store](../../relational-databases/system-catalog-views/syscollector-config-store-transact-sql.md) .  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle de base de données fixe dc_admin (avec autorisation EXECUTE) pour exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant montre comment configurer le collecteur de données pour qu'il utilise une instance d'entrepôt de données de gestion sur un serveur distant. Dans cet exemple, le serveur distant est nommé `RemoteSERVER` et la base de données est installée sur l'instance par défaut.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_set_warehouse_instance_name N'RemoteSERVER'; -- the default instance is assumed on the remote server  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [syscollector_config_store &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-config-store-transact-sql.md)  
  
  
