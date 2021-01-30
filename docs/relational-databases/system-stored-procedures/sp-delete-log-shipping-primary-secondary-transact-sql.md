---
description: sp_delete_log_shipping_primary_secondary (Transact-SQL)
title: sp_delete_log_shipping_primary_secondary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_log_shipping_primary_secondary_TSQL
- sp_delete_log_shipping_primary_secondary
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_log_shipping_primary_secondary
ms.assetid: d6f71a12-f7b1-4a1c-9639-a533b8287b0c
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: aad84b3a0d02300c0b26a457bc4e16fc21a41dbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211129"
---
# <a name="sp_delete_log_shipping_primary_secondary-transact-sql"></a>sp_delete_log_shipping_primary_secondary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime l'entrée d'une base de données secondaire sur le serveur principal.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_delete_log_shipping_primary_secondary  
    [ @primary_database = ] 'primary_database',   
    [ @secondary_server = ] 'secondary_server',   
    [ @secondary_database = ] 'secondary_database'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @primary_database = ] 'primary_database'` Nom de la base de données sur le serveur principal. *primary_database* est de **type sysname**, sans valeur par défaut.  
  
`[ @secondary_server = ] 'secondary_server'` Nom du serveur secondaire. *secondary_server* est de **type sysname**, sans valeur par défaut.  
  
`[ @secondary_database = ] 'secondary_database'` Nom de la base de données secondaire. *secondary_database* est de **type sysname**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun.  
  
## <a name="remarks"></a>Notes  
 **sp_delete_log_shipping_primary_secondary** doit être exécuté à partir de la base de données **Master** sur le serveur principal. Cette procédure stockée supprime l’entrée d’une base de données secondaire de **log_shipping_primary_secondaries** sur le serveur principal.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple suivant, `sp_delete_log_shipping_primary_secondary` est utilisée pour supprimer la base de données secondaire `LogShipAdventureWorks` du serveur secondaire `FLATIRON`.  
  
```  
EXEC master.dbo.sp_delete_log_shipping_primary_secondary  
@primary_database = N'AdventureWorks'  
,@secondary_server = N'FLATIRON'  
,@secondary_database = N'LogShipAdventureWorks';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
