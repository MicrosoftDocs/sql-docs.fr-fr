---
description: '&#x40;&#x40;PROCID (Transact-SQL)'
title: '@@PROCID (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@PROCID'
- '@@PROCID_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- stored procedures [SQL Server], identification numbers
- UDTs [SQL Server], object identifiers
- '@@PROCID function'
- user-defined functions [SQL Server], object identifiers
- triggers [SQL Server], object identifiers
- identification numbers [SQL Server], modules
- IDs [SQL Server], modules
- module object identifiers [SQL Server]
ms.assetid: 0d4882c7-edb8-49b1-a470-2c7497b8998f
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ca5a2c5fe01e67a4d2fb9486169e22c16d36f0a8
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88445641"
---
# <a name="x40x40procid-transact-sql"></a>&#x40;&#x40;PROCID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne l'identificateur (ID) d'objet du module [!INCLUDE[tsql](../../includes/tsql-md.md)] en cours. Un module [!INCLUDE[tsql](../../includes/tsql-md.md)] peut consister en une procédure stockée, une fonction définie par l'utilisateur ou un déclencheur. @@PROCID ne peut pas être spécifié dans des modules CLR ou dans le fournisseur d’accès aux données en cours de traitement.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
@@PROCID  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 **int**  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise `@@PROCID` comme paramètre d'entrée dans la fonction `OBJECT_NAME` pour retourner le nom de la procédure stockée dans le message `RAISERROR`.  
  
```  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ( 'usp_FindName', 'P' ) IS NOT NULL   
DROP PROCEDURE usp_FindName;  
GO  
CREATE PROCEDURE usp_FindName  
    @lastname varchar(40) = '%',   
    @firstname varchar(20) = '%'  
AS  
DECLARE @Count int;  
DECLARE @ProcName nvarchar(128);  
SELECT LastName, FirstName  
FROM Person.Person   
WHERE FirstName LIKE @firstname AND LastName LIKE @lastname;  
SET @Count = @@ROWCOUNT;  
SET @ProcName = OBJECT_NAME(@@PROCID);  
RAISERROR ('Stored procedure %s returned %d rows.', 16,10, @ProcName, @Count);  
GO  
EXECUTE dbo.usp_FindName 'P%', 'A%';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [Fonctions de métadonnées &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)  
  
  
