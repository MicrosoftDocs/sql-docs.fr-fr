---
description: DEALLOCATE (Transact-SQL)
title: DEALLOCATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DEALLOCATE
- DEALLOCATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- locking [SQL Server], cursors
- DEALLOCATE statement
- deallocations [SQL Server]
- deleting cursor references
- removing cursor references
ms.assetid: c75cf73d-0268-4c57-973d-b8a84ff801fa
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2b6ad1892bec2cb627e4565c798506bcea91b530
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208780"
---
# <a name="deallocate-transact-sql"></a>DEALLOCATE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Supprime une référence de curseur. Une fois la dernière référence de curseur désallouée, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libère les structures de données contenant le curseur.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DEALLOCATE { { [ GLOBAL ] cursor_name } | @cursor_variable_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *cursor_name*  
 Nom d'un curseur déjà déclaré. Si un curseur global et un curseur local ont tous les deux le même nom *cursor_name*, *cursor_name* fait référence au curseur global si `GLOBAL` est spécifié, et au curseur local si `GLOBAL` n’est pas spécifié.  
  
 @*cursor_variable_name*  
 Nom d’une variable de **curseur**. @*cursor_variable_name* doit être de type **curseur**.  
  
## <a name="remarks"></a>Remarques  
Les instructions affectant les curseurs font référence à ceux-ci à l'aide d'un nom ou d'une variable de curseur. `DEALLOCATE` supprime l’association entre un curseur et le nom ou la variable de curseur. S'il s'agit du dernier nom ou de la dernière variable qui référence le curseur, ce dernier est désalloué et les ressources qu'il utilisait sont libérées. `DEALLOCATE` libère les verrous de défilement qui protègent l’isolation des extractions. Les verrous de transaction, utilisés pour protéger les mises à jour, notamment les mises à jour pointées par le curseur, sont maintenus jusqu'à la fin de la transaction.  
  
L’instruction `DECLARE CURSOR` alloue et associe un curseur à un nom de curseur.  
  
```sql  
DECLARE abc SCROLL CURSOR FOR  
SELECT * FROM Person.Person;  
```  
  
Dès qu’un nom de curseur est associé à un curseur, vous ne pouvez pas lui affecter un autre curseur de même étendue (globale ou locale) tant que ce curseur n’a pas été désalloué.  
  
 Une variable de curseur est associée à un curseur à l'aide de l'une des deux méthodes suivantes :  
  
-   En spécifiant son nom avec une instruction `SET` qui définit un curseur sur une variable de curseur.  
  
    ```sql  
    DECLARE @MyCrsrRef CURSOR;  
    SET @MyCrsrRef = abc;  
    ```  
  
-   en créant et en associant un curseur à une variable sans qu'il y ait de nom de curseur défini.  
  
    ```sql  
    DECLARE @MyCursor CURSOR;  
    SET @MyCursor = CURSOR LOCAL SCROLL FOR  
    SELECT * FROM Person.Person;  
    ```  
  
 Une instruction `DEALLOCATE <@cursor_variable_name>` supprime seulement la référence de la variable nommée au curseur. La variable est libérée uniquement lorsqu'elle est hors de portée à la fin d'un lot, d'une procédure stockée ou d'un déclencheur. Après une instruction `DEALLOCATE <@cursor_variable_name>`, la variable peut être associée à un autre curseur avec l’instruction SET.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
DECLARE @MyCursor CURSOR;  
SET @MyCursor = CURSOR LOCAL SCROLL FOR  
    SELECT * FROM Sales.SalesPerson;  
  
DEALLOCATE @MyCursor;  
  
SET @MyCursor = CURSOR LOCAL SCROLL FOR  
    SELECT * FROM Sales.SalesTerritory;  
GO  
```  
  
Il n'est pas nécessaire de désallouer explicitement une variable de curseur. Elle est implicitement désallouée une fois qu'elle est hors de portée.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, tout utilisateur valide a l’autorisation d’utiliser l’instruction `DEALLOCATE`.  
  
## <a name="examples"></a>Exemples  
 Le script suivant montre comment les curseurs sont maintenus jusqu'à ce que le dernier nom ou la variable les référençant aient été libérés.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create and open a global named cursor that  
-- is visible outside the batch.  
DECLARE abc CURSOR GLOBAL SCROLL FOR  
    SELECT * FROM Sales.SalesPerson;  
OPEN abc;  
GO  
-- Reference the named cursor with a cursor variable.  
DECLARE @MyCrsrRef1 CURSOR;  
SET @MyCrsrRef1 = abc;  
-- Now deallocate the cursor reference.  
DEALLOCATE @MyCrsrRef1;  
-- Cursor abc still exists.  
FETCH NEXT FROM abc;  
GO  
-- Reference the named cursor again.  
DECLARE @MyCrsrRef2 CURSOR;  
SET @MyCrsrRef2 = abc;  
-- Now deallocate cursor name abc.  
DEALLOCATE abc;  
-- Cursor still exists, referenced by @MyCrsrRef2.  
FETCH NEXT FROM @MyCrsrRef2;  
-- Cursor finally is deallocated when last referencing  
-- variable goes out of scope at the end of the batch.  
GO  
-- Create an unnamed cursor.  
DECLARE @MyCursor CURSOR;  
SET @MyCursor = CURSOR LOCAL SCROLL FOR  
SELECT * FROM Sales.SalesTerritory;  
-- The following statement deallocates the cursor  
-- because no other variables reference it.  
DEALLOCATE @MyCursor;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CLOSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/close-transact-sql.md)   
 [Curseurs](../../relational-databases/cursors.md)   
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [FETCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/fetch-transact-sql.md)   
 [OPEN &#40;Transact-SQL&#41;](../../t-sql/language-elements/open-transact-sql.md)  
  
  
