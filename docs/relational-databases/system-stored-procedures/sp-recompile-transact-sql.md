---
description: sp_recompile (Transact-SQL)
title: sp_recompile (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_recompile_TSQL
- sp_recompile
dev_langs:
- TSQL
helpviewer_keywords:
- sp_recompile
ms.assetid: 6192ca87-febd-4075-8199-14b4fa609b8c
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a5ff0697d2ca09677455c700b1dfc45f16f1ef72
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200649"
---
# <a name="sp_recompile-transact-sql"></a>sp_recompile (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Provoque la recompilation des procédures stockées, des déclencheurs et des fonctions définies par l'utilisateur lors de leur prochaine exécution. Pour cela, il supprime le plan existant du cache de procédures, ce qui force la création d'un nouveau plan lors de la prochaine exécution de la procédure ou du déclencheur. Dans une collection [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], l'événement SP:CacheInsert est journalisé au lieu de l'événement SP:Recompile.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
  
sp_recompile [ @objname = ] 'object'  
```  
  
## <a name="arguments"></a>Arguments  
 [ @objname =] '*objet*'  
 Nom qualifié ou non qualifié d'une procédure stockée, d'un déclencheur, d'une table, d'une vue ou d'une fonction définie par l'utilisateur dans la base de données actuelle. l' *objet* est de type **nvarchar (776)**, sans valeur par défaut. Si *Object* est le nom d’une procédure stockée, d’un déclencheur ou d’une fonction définie par l’utilisateur, la procédure stockée, le déclencheur ou la fonction est recompilé lors de sa prochaine exécution. Si *Object* est le nom d’une table ou d’une vue, toutes les procédures stockées, les déclencheurs ou les fonctions définies par l’utilisateur qui font référence à cette table ou cette vue seront recompilés lors de leur prochaine exécution.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou un nombre différent de zéro (échec)  
  
## <a name="remarks"></a>Notes  
 sp_recompile ne recherche un objet que dans la base de données active.  
  
 Les requêtes utilisées par les procédures stockées ou les déclencheurs et les fonctions définies par l'utilisateur ne sont optimisées que quand elles sont compilées. À mesure que vous ajoutez des index à votre base de données ou que vous y apportez d'autres changements modifiant ses statistiques, les procédures stockées, les déclencheurs et les fonctions définies par l'utilisateur compilés peuvent perdre de leur efficacité. En recompilant les procédures stockées et les déclencheurs qui agissent sur une table, vous pouvez réoptimiser les requêtes.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recompile automatiquement les procédures stockées, les déclencheurs et les fonctions définies par l'utilisateur quand il est avantageux de le faire.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER pour l'objet spécifié.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant engendre la recompilation des procédures stockées, des déclencheurs et des fonctions définies par l'utilisateur qui agissent sur la table `Customer` lors de leur prochaine exécution.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_recompile N'Sales.Customer';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
