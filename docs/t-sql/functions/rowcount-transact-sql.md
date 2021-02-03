---
description: '&#x40;&#x40;ROWCOUNT (Transact-SQL)'
title: '@@ROWCOUNT (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 08/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@ROWCOUNT_TSQL'
- '@@ROWCOUNT'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@ROWCOUNT function'
- number of rows affected by statement
- row affected by statements [SQL Server]
- statements [SQL Server], last statement
- counting rows
ms.assetid: 97a47998-81d9-4331-a244-9eb8b6fe4a56
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 6eca1ac99c93c8faa062b46b09fe3715663a5c87
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159719"
---
# <a name="x40x40rowcount-transact-sql"></a>&#x40;&#x40;ROWCOUNT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne le nombre de lignes affectées par la dernière instruction. Si le nombre de lignes est supérieur à 2 milliards, utilisez [ROWCOUNT_BIG](../../t-sql/functions/rowcount-big-transact-sql.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
@@ROWCOUNT  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 **int**  
  
## <a name="remarks"></a>Notes  
 Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] peuvent définir la valeur de @@ROWCOUNT comme suit :  
  
-   En attribuant à @@ROWCOUNT le nombre de lignes affectées ou lues. Les lignes peuvent être, ou ne pas être, envoyées au client.  
  
-   En conservant la valeur de @@ROWCOUNT issue de l’exécution de l’instruction précédente.  
  
-   En réinitialisant @@ROWCOUNT à 0 sans renvoyer la valeur au client.  
  
 Les instructions qui réalisent une affectation simple attribuent toujours à @@ROWCOUNT la valeur 1. Aucune ligne n'est envoyée au client. Parmi ces instructions figurent : SET @*variable_locale*, RETURN, READTEXT, et les instructions de sélection sans requête, comme SELECT GETDATE() ou SELECT **'** _Texte générique_*_'_*.  
  
 Les instructions qui effectuent une affectation dans une requête ou qui utilisent RETURN dans une requête attribuent à @@ROWCOUNT le nombre de lignes affectées ou lues par la requête ; par exemple : SELECT @*local_variable* = c1 FROM t1.  
  
 Les instructions DML (Data Manipulation Language) attribuent à @@ROWCOUNT le nombre de lignes affectées par la requête et renvoient cette valeur au client. Les instructions DML peuvent ne pas envoyer de lignes au client.  
  
 DECLARE CURSOR et FETCH attribuent à @@ROWCOUNT la valeur 1.  
  
 Les instructions EXECUTE conservent la valeur @@ROWCOUNT précédente.  
  
 Les instructions telles que USE, SET \<option>, DEALLOCATE CURSOR, CLOSE CURSOR, PRINT, RAISERROR, BEGIN TRANSACTION ou COMMIT TRANSACTION réinitialisent la valeur ROWCOUNT à 0.  
  
 Les procédures stockées compilées en mode natif conservent la valeur @@ROWCOUNT précédente. Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] à l’intérieur des procédures stockées compilées en mode natif ne définissent pas @@ROWCOUNT. Pour plus d’informations, consultez [Procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant exécute une instruction `UPDATE` et utilise `@@ROWCOUNT` pour déterminer si des lignes ont été modifiées.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE HumanResources.Employee   
SET JobTitle = N'Executive'  
WHERE NationalIDNumber = 123456789  
IF @@ROWCOUNT = 0  
PRINT 'Warning: No rows were updated';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md)  
  
