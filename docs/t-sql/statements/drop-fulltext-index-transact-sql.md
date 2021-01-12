---
description: DROP FULLTEXT INDEX (Transact-SQL)
title: DROP FULLTEXT INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_FULLTEXT_INDEX_TSQL
- DROP FULLTEXT INDEX
dev_langs:
- TSQL
helpviewer_keywords:
- deleting full-text indexes
- removing full-text indexes
- full-text indexes [SQL Server], removing
- DROP FULLTEXT INDEX statement
- dropping full-text indexes
ms.assetid: 7443a4ab-1d43-4a22-bbba-a07f620892cb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a5ed7a438741e9e69f5d5df98f9b5a3958977e26
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091944"
---
# <a name="drop-fulltext-index-transact-sql"></a>DROP FULLTEXT INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Supprime un index de recherche en texte intégral d'une vue indexée ou d'une table spécifique.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DROP FULLTEXT INDEX ON table_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *table_name*  
 Nom de la table ou de la vue indexée contenant l'index de recherche en texte intégral à supprimer.  
  
## <a name="remarks"></a>Notes  
 Vous n'avez pas besoin de supprimer toutes les colonnes à partir de l'index de recherche en texte intégral avant d'utiliser la commande DROP FULLTEXT INDEX.  
  
## <a name="permissions"></a>Autorisations  
 L’utilisateur doit soit disposer de l’autorisation ALTER sur la table ou la vue indexée, soit être un membre du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** ou **db_ddladmin**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime l'index de recherche en texte intégral qui existe sur la table `JobCandidate`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DROP FULLTEXT INDEX ON HumanResources.JobCandidate;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)   
 [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)  
  
  
