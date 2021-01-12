---
description: DROP ROUTE (Transact-SQL)
title: DROP ROUTE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP ROUTE
- DROP_ROUTE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dropping routes
- DROP ROUTE statement
- deleting routes
- routes [Service Broker], removing
- removing routes
ms.assetid: d8fab0bc-d54a-46ca-9437-552db7477d40
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0a699415666b17a53ebbedf596a16ebfe8fae4e7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98081069"
---
# <a name="drop-route-transact-sql"></a>DROP ROUTE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime un itinéraire, en effaçant les informations de cet itinéraire dans la table de routage de la base de données active.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DROP ROUTE route_name  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *route_name*  
 Nom de l'itinéraire à supprimer. Les noms du serveur, de la base de données et du schéma ne peuvent pas être spécifiés.  
  
## <a name="remarks"></a>Remarques  
 La table de routage qui stocke les itinéraires est une table de métadonnées consultable via la vue de catalogue **sys.routes**. La mise à jour de la table de routage s'effectue uniquement au moyen des instructions CREATE ROUTE, ALTER ROUTE et DROP ROUTE.  
  
 Un itinéraire peut être supprimé, indépendamment du fait que des conversations l'empruntent ou non. Cependant, si aucun autre itinéraire ne mène au service distant, les messages de ces conversations demeurent dans les files d'attente de transmission jusqu'à ce qu'un itinéraire rejoignant le service distant soit créé ou que le délai de la conversation ait expiré.  
  
## <a name="permissions"></a>Autorisations  
 L'autorisation de suppression d'un itinéraire est accordée par défaut au propriétaire de l'itinéraire, aux membres du rôle de base de données fixe db_ddladmin ou db_owner, ainsi qu'aux membres du rôle serveur fixe sysadmin.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime l'itinéraire `ExpenseRoute`.  
  
```sql  
DROP ROUTE ExpenseRoute ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-route-transact-sql.md)   
 [CREATE ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/create-route-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.routes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-routes-transact-sql.md)  
  
  
