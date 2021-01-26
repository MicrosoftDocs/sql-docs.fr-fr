---
description: DROP AGGREGATE (Transact-SQL)
title: DROP AGGREGATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_AGGREGATE_TSQL
- DROP AGGREGATE
dev_langs:
- TSQL
helpviewer_keywords:
- aggregate functions [SQL Server], removing
- removing user-defined functions
- dropping user-defined functions
- user-defined functions [CLR integration]
- deleting user-defined functions
- DROP AGGREGATE statement
ms.assetid: 84ffc4e7-c451-4f1f-9a67-7fc3a120e53f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c9eee09f1c0612434f0facda8bd4db1015f04e84
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688995"
---
# <a name="drop-aggregate-transact-sql"></a>DROP AGGREGATE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime une fonction d'agrégation définie par l'utilisateur de la base de données active. Les fonctions d’agrégation définies par l’utilisateur sont créées à l’aide de [CREATE AGGREGATE](../../t-sql/statements/create-aggregate-transact-sql.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
DROP AGGREGATE [ IF EXISTS ] [ schema_name . ] aggregate_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *IF EXISTS*  
 **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] à la [version actuelle](../../sql-server/what-s-new-in-sql-server-2016.md)).  
  
 Supprime de manière conditionnelle l’agrégat, uniquement s’il existe déjà.  
  
 *schema_name*  
 Nom du schéma auquel appartient la fonction d'agrégation définie par l'utilisateur.  
  
 *aggregate_name*  
 Nom de la fonction d'agrégation définie par l'utilisateur à supprimer.  
  
## <a name="remarks"></a>Remarques  
 DROP AGGREGATE ne s'exécute pas si des vues, fonctions ou procédures stockées créées avec une liaison de schéma référencent la fonction d'agrégation définie par l'utilisateur à supprimer.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter DROP AGGREGATE, un utilisateur doit, au minimum, posséder l'autorisation ALTER sur le schéma auquel appartient l'agrégation définie par l'utilisateur ou l'autorisation CONTROL sur l'agrégation.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime l'agrégation `Concatenate`.  
  
```sql  
DROP AGGREGATE dbo.Concatenate;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE AGGREGATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-aggregate-transact-sql.md)   
 [Créer des agrégats définis par l'utilisateur](../../relational-databases/user-defined-functions/create-user-defined-aggregates.md)  
