---
description: sp_add_log_shipping_primary_secondary (Transact-SQL)
title: sp_add_log_shipping_primary_secondary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_log_shipping_primary_secondary_TSQL
- sp_add_log_shipping_primary_secondary
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_shipping_primary_secondary
ms.assetid: 23b3e100-5318-410e-b8f3-51c89b2dd777
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 46c33fa71e9f0c3b49a693896b7349d3f5bdf7bf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186037"
---
# <a name="sp_add_log_shipping_primary_secondary-transact-sql"></a>sp_add_log_shipping_primary_secondary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette procédure stockée ajoute une entrée pour une base de données secondaire sur le serveur principal.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_log_shipping_primary_secondary  
[ @primary_database = ] 'primary_database',  
[ @secondary_server = ] 'secondary_server',   
[ @secondary_database = ] 'secondary_database'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @primary_database = ] 'primary_database'` Nom de la base de données sur le serveur principal. *primary_database* est de **type sysname**, sans valeur par défaut.  
  
`[ @secondary_server = ] 'secondary_server',` Nom du serveur secondaire. *secondary_server* est de **type sysname**, sans valeur par défaut.  
  
`[ @secondary_database = ] 'secondary_database'` Nom de la base de données secondaire. *secondary_database* est de **type sysname**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 **sp_add_log_shipping_primary_secondary** doit être exécuté à partir de la base de données **Master** sur le serveur principal.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 Cet exemple illustre l’utilisation de **sp_add_log_shipping_primary_secondary** pour ajouter une entrée pour la base de données secondaire **LogShipAdventureWorks** au serveur secondaire Flatiron.  
  
```  
EXEC master.dbo.sp_add_log_shipping_primary_secondary   
@primary_database = N'AdventureWorks'   
, @secondary_server = N'flatiron'   
, @secondary_database = N'LogShipAdventureWorks' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
