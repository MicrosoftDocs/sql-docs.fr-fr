---
description: SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL)
title: SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET QUERY_GOVERNOR_COST_LIMIT
- SET_QUERY_GOVERNOR_COST_LIMIT_TSQL
- QUERY_GOVERNOR_COST_LIMIT
- QUERY_GOVERNOR_COST_LIMIT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET QUERY_GOVERNOR_COST_LIMIT statement
- connections [SQL Server], overriding
- QUERY_GOVERNOR_COST_LIMIT option
- overriding connection values
ms.assetid: 3424bb44-6915-462d-a8d7-fe834af81387
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9204c77bdc1a3e99d66cb2ee6000bf4c1ecb1609
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206976"
---
# <a name="set-query_governor_cost_limit-transact-sql"></a>SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Remplace la valeur **query governor cost limit** actuellement définie pour la connexion active.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
SET QUERY_GOVERNOR_COST_LIMIT value  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *value*  
 Valeur numérique ou entière qui spécifie la durée la plus longue pendant laquelle une requête peut s'exécuter. Les valeurs sont arrondies à l'entier le plus proche. Les valeurs négatives sont arrondies à 0. L'Administrateur de requêtes n'autorise pas l'exécution de requêtes dont le coût estimé excède cette valeur. Si vous définissez 0 (valeur par défaut) pour cette option, vous désactivez la fonction de l'administrateur de requêtes, et toutes les requêtes s'exécutent indéfiniment.  
  
 Le « coût d'une requête » correspond à la durée (en secondes) estimée nécessaire à l'exécution complète d'une requête dans une configuration matérielle donnée.  
  
## <a name="remarks"></a>Notes  
 L'utilisation de SET QUERY_GOVERNOR_COST_LIMIT s'applique à la connexion active et est effective durant celle-ci uniquement. Utilisez l’option de configuration du serveur [SET QUERY_GOVERNOR_COST_LIMIT](../../database-engine/configure-windows/configure-the-query-governor-cost-limit-server-configuration-option.md) de **sp_configure** pour modifier le coût maximal d’exécution de l’Administrateur de requêtes sur l’ensemble du serveur. Pour plus d’informations sur la configuration de cette option, consultez [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) et [Options de configuration du serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
 SET QUERY_GOVERNOR_COST_LIMIT est définie lors de l'exécution, et non pas durant l'analyse.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="see-also"></a>Voir aussi  
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
