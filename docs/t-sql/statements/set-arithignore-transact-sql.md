---
description: SET ARITHIGNORE (Transact-SQL)
title: SET ARITHIGNORE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET ARITHIGNORE
- SET_ARITHIGNORE_TSQL
- ARITHIGNORE
- ARITHIGNORE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET ARITHIGNORE statement
- overflow errors [SQL Server]
- ARITHIGNORE option
- divide-by-zero errors
ms.assetid: 71b2c2a5-c83a-4dfe-8469-237987a6e503
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: edb7e4a13ad5339cedf3ca465453fe158c42b668
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091896"
---
# <a name="set-arithignore-transact-sql"></a>SET ARITHIGNORE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Détermine si un message d'erreur est renvoyé lorsqu'une erreur de division par zéro ou de dépassement de capacité se produit durant une requête.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database

SET ARITHIGNORE { ON | OFF }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

SET ARITHIGNORE OFF
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="remarks"></a>Notes
 Le paramètre SET ARITHIGNORE contrôle uniquement la situation où un message d'erreur est renvoyé. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] renvoie une valeur NULL dans un calcul qui comprend une erreur de dépassement de capacité ou de division par zéro, indépendamment de ce paramètre. Le paramètre SET ARITHABORT peut être utilisée pour déterminer si la requête est achevée. Cette valeur n'affecte pas les erreurs survenant durant des instructions INSERT, UPDATE et DELETE.  
  
 Si la valeur de SET ARITHABORT ou de SET ARITHIGNORE est définie à OFF et que SET ANSI_WARNINGS a la valeur ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] renvoie malgré tout un message d'erreur quand il rencontre une erreur de division par zéro ou de dépassement de capacité.  
  
 L'option SET ARITHIGNORE est définie lors de l'exécution, et non pas durant l'analyse.  
  
 Pour afficher la valeur actuelle de ce paramètre, exécutez la requête suivante.  
  
```sql  
DECLARE @ARITHIGNORE VARCHAR(3) = 'OFF';  
IF ( (128 & @@OPTIONS) = 128 ) SET @ARITHIGNORE = 'ON';  
SELECT @ARITHIGNORE AS ARITHIGNORE;  
```  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant illustre l'utilisation des paramètres `SET ARITHIGNORE` avec les deux types d'erreur survenant dans une requête.  
  
```sql  
SET ARITHABORT OFF;  
SET ANSI_WARNINGS OFF  
GO  
  
PRINT 'Setting ARITHIGNORE ON';  
GO  
-- SET ARITHIGNORE ON and testing.  
SET ARITHIGNORE ON;  
GO  
SELECT 1 / 0 AS DivideByZero;  
GO  
SELECT CAST(256 AS TINYINT) AS Overflow;  
GO  
  
PRINT 'Setting ARITHIGNORE OFF';  
GO  
-- SET ARITHIGNORE OFF and testing.  
SET ARITHIGNORE OFF;  
GO  
SELECT 1 / 0 AS DivideByZero;  
GO  
SELECT CAST(256 AS TINYINT) AS Overflow;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L’exemple suivant illustre les erreurs de dépassement et de division par zéro. Cet exemple ne retourne pas de message d’erreur pour ces erreurs, car ARITHIGNORE a la valeur OFF.  
  
```sql  
-- SET ARITHIGNORE OFF and testing.  
SET ARITHIGNORE OFF;  
SELECT 1 / 0 AS DivideByZero;  
SELECT CAST(256 AS TINYINT) AS Overflow;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ARITHABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithabort-transact-sql.md)  
  
  

