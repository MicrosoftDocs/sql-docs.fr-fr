---
description: DROP PROCEDURE (Transact-SQL)
title: DROP PROCEDURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP PROCEDURE
- DROP_PROCEDURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- removing stored procedures
- dropping procedure groups
- deleting stored procedures
- deleting procedure groups
- DROP PROCEDURE statement
- dropping stored procedures
- stored procedures [SQL Server], removing
- removing procedure groups
ms.assetid: 1c2d7235-7b9b-4336-8f17-429e7d82c2c3
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3915bf9f19396c8cc55179861e23052f25886b20
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98689046"
---
# <a name="drop-procedure-transact-sql"></a>DROP PROCEDURE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Supprime une ou plusieurs procédures stockées ou un ou plusieurs groupes de procédures de la base de données active dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
DROP { PROC | PROCEDURE } [ IF EXISTS ] { [ schema_name. ] procedure } [ ,...n ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP { PROC | PROCEDURE } { [ schema_name. ] procedure_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *IF EXISTS*  
 **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] à la [version actuelle](../../sql-server/what-s-new-in-sql-server-2016.md)).  
  
 Supprime, de manière conditionnelle, la procédure uniquement si elle existe déjà.  
  
 *schema_name*  
 Nom du schéma auquel appartient la procédure. Vous ne pouvez pas spécifier de nom de serveur ou de base de données.  
  
 *procedure*  
 Nom de la procédure stockée ou du groupe de procédures stockées à supprimer. Vous ne pouvez pas supprimer des procédures individuelles dans un groupe de procédures numérotées ; dans ce cas, tout le groupe de procédures est supprimé.  
  
## <a name="best-practices"></a>Bonnes pratiques  
 Avant de supprimer une procédure stockée, vérifiez les objets dépendants et modifiez-les en conséquence. La suppression d'une procédure stockée peut entraîner l'échec des scripts et des objets dépendants quand ceux-ci n'ont pas été mis à jour. Pour plus d’informations, consultez [Afficher les dépendances d’une procédure stockée](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md).  
  
## <a name="metadata"></a>Métadonnées  
 Pour afficher la liste des procédures existantes, interrogez la vue de catalogue **sys.objects**. Pour afficher la définition de procédure, interrogez la vue de catalogue **sys.sql_modules**.  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **CONTROL** sur la procédure, ou l’autorisation **ALTER** sur le schéma auquel appartient la procédure, ou encore l’appartenance au rôle serveur fixe **db_ddladmin**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime la procédure stockée `dbo.uspMyProc` de la base de données active.  
  
```sql  
DROP PROCEDURE dbo.uspMyProc;  
GO  
```  
  
 L'exemple suivant supprime plusieurs procédures stockées de la base de données active.  
  
```sql  
DROP PROCEDURE dbo.uspGetSalesbyMonth, dbo.uspUpdateSalesQuotes, dbo.uspGetSalesByYear;  
```  
  
 L’exemple suivant supprime la procédure stockée `dbo.uspMyProc` si elle existe, mais ne génère pas d’erreur si elle n’existe pas. Cette syntaxe est une nouveauté de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)].  
  
```sql  
DROP PROCEDURE IF EXISTS dbo.uspMyProc;  
GO  
```  
  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [Supprimer une procédure stockée](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)  
