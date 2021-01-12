---
description: SET FORCEPLAN (Transact-SQL)
title: SET FORCEPLAN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET_FORCEPLAN_TSQL
- SET FORCEPLAN
- FORCEPLAN
- FORCEPLAN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- joins [SQL Server], overriding query optimizer process
- FORCEPLAN option
- SET FORCEPLAN statement
- query optimizer [SQL Server], optimizing process
- overriding query optimizer process
ms.assetid: b6c0b08f-2060-4696-9e12-50cb7e674321
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 35fee4ee42f2b401b584e752e1a834c991b03483
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093374"
---
# <a name="set-forceplan-transact-sql"></a>SET FORCEPLAN (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Lorsque FORCEPLAN a pour valeur ON, l'optimiseur de requête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] traite une jointure dans le même ordre que celui où les tables apparaissent dans la clause FROM d'une requête. Qui plus est, la définition de FORCEPLAN à ON force l'utilisation d'une jointure de boucles imbriquées sauf si d'autres types de jointures sont nécessaires pour construire un plan pour la requête ou si elles sont demandées avec des indicateurs de requête ou de jointure.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
SET FORCEPLAN { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Notes
 SET FORCEPLAN a priorité sur la logique utilisée par l'optimiseur pour traiter une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] SELECT. Les données renvoyées par l'instruction SELECT sont identiques, que cette option soit ou non utilisée. La seule différence est la façon dont [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] traite les tables durant l'exécution de la requête.  
  
 Les indicateurs de l'optimiseur de requête peuvent également être utilisés dans les requêtes pour modifier la façon dont [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] traite l'instruction SELECT.  
  
 L'option SET FORCEPLAN est appliquée lors de l'exécution et non pas au moment de l'analyse.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations SET FORCEPLAN sont octroyées par défaut à tous les utilisateurs.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant effectue une jointure de quatre tables. L'option `SHOWPLAN_TEXT` est activée afin que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] renvoie des informations indiquant comment la requête est traitée différemment après l'activation de `SET FORCE_PLAN`.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Make sure FORCEPLAN is set to OFF.  
SET SHOWPLAN_TEXT OFF;  
GO  
SET FORCEPLAN OFF;  
GO  
SET SHOWPLAN_TEXT ON;  
GO  
-- Example where the query plan is not forced.  
SELECT p.LastName, p.FirstName, v.Name  
FROM Person.Person AS p  
   INNER JOIN HumanResources.Employee AS e  
   ON e.BusinessEntityID = p.BusinessEntityID  
   INNER JOIN Purchasing.PurchaseOrderHeader AS poh  
   ON e.BusinessEntityID = poh.EmployeeID  
   INNER JOIN Purchasing.Vendor AS v  
   ON poh.VendorID = v.BusinessEntityID;  
GO  
-- SET FORCEPLAN to ON.  
SET SHOWPLAN_TEXT OFF;  
GO  
SET FORCEPLAN ON;  
GO  
SET SHOWPLAN_TEXT ON;  
GO  
-- Reexecute inner join to see the effect of SET FORCEPLAN ON.  
SELECT p.LastName, p.FirstName, v.Name  
FROM Person.Person AS p  
   INNER JOIN HumanResources.Employee AS e   
   ON e.BusinessEntityID = p.BusinessEntityID  
   INNER JOIN Purchasing.PurchaseOrderHeader AS poh  
   ON e.BusinessEntityID = poh.EmployeeID  
   INNER JOIN Purchasing.Vendor AS v  
   ON poh.VendorID = v.BusinessEntityID;  
GO  
SET SHOWPLAN_TEXT OFF;  
GO  
SET FORCEPLAN OFF;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET SHOWPLAN_ALL &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-all-transact-sql.md)   
 [SET SHOWPLAN_TEXT &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-text-transact-sql.md)  
  
  
