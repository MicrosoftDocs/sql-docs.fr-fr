---
description: WHILE (Transact-SQL)
title: WHILE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WHILE_TSQL
- WHILE
dev_langs:
- TSQL
helpviewer_keywords:
- statements [SQL Server], repeated executions
- statement blocks [SQL Server]
- repeated statement executions
- nested WHILE loops
- WHILE keyword
ms.assetid: 52dd29ab-25d7-4fd3-a960-ac55c30c9ea9
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 894ec6f75bc3c79a2e3fb1037bbb28dac0604510
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211043"
---
# <a name="while-transact-sql"></a>WHILE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


  Définit la condition qui fera se répéter l'exécution d'une instruction SQL ou d'un bloc d'instructions. L'exécution des instructions se répète aussi longtemps que la condition spécifiée demeure vraie. L'exécution des instructions de la boucle WHILE peut être contrôlée de l'intérieur de la boucle avec les mots clés BREAK et CONTINUE.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
WHILE Boolean_expression   
     { sql_statement | statement_block | BREAK | CONTINUE }  
  
```  
  
```syntaxsql
-- Syntax for Azure Azure Synapse Analytics and Parallel Data Warehouse  
  
WHILE Boolean_expression   
     { sql_statement | statement_block | BREAK }  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *Boolean_expression*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md) qui renvoie **TRUE** ou **FALSE**. Si l'expression booléenne contient une instruction SELECT, cette dernière doit être mise entre parenthèses.  
  
 {*sql_statement* | *statement_block*}  
 Toute instruction ou tout groupe d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] tels que définis dans un bloc d'instructions. Pour définir un bloc d'instructions, utilisez les mots clés de contrôle de flux BEGIN et END.  
  
 BREAK  
 Provoque la sortie de la boucle WHILE la plus récente. Toutes les instructions situées après le mot clé END, marquant la fin de cette boucle, sont alors exécutées.  
  
 CONTINUE  
 Provoque le redémarrage de la boucle WHILE, en ignorant toutes les instructions qui suivent le mot clé CONTINUE.  
  
## <a name="remarks"></a>Remarques  
 Si plusieurs boucles WHILE sont imbriquées, le BREAK à l'intérieur de la boucle provoque le retour à la boucle précédente. Avant que la boucle précédente ne prenne le relais, toutes les instructions situées après la fin de la boucle intérieure sont d'abord exécutées.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-break-and-continue-with-nested-ifelse-and-while"></a>R. Utilisation de BREAK et CONTINUE avec des IF…ELSE et des WHILE imbriqués  
 Dans l'exemple suivant, si le prix moyen d'un produit est inférieur à `$300`, la boucle `WHILE` double les prix puis sélectionne le prix maximum. Si le prix maximum est inférieur ou égal à `$500`, la boucle `WHILE` redémarre et double de nouveau les prix. Cette boucle continue à doubler les prix jusqu'à ce que le prix maximum soit supérieur à `$500`, puis le programme sort de la boucle `WHILE` et affiche un message.  
  
```sql  
USE AdventureWorks2012;  
GO  
WHILE (SELECT AVG(ListPrice) FROM Production.Product) < $300  
BEGIN  
   UPDATE Production.Product  
      SET ListPrice = ListPrice * 2  
   SELECT MAX(ListPrice) FROM Production.Product  
   IF (SELECT MAX(ListPrice) FROM Production.Product) > $500  
      BREAK  
   ELSE  
      CONTINUE  
END  
PRINT 'Too much for the market to bear';  
```  
  
### <a name="b-using-while-in-a-cursor"></a>B. Utilisation de WHILE dans un curseur  
 L'exemple suivant utilise `@@FETCH_STATUS` pour contrôler les activités d'un curseur dans une boucle `WHILE`.  
  
```sql  
DECLARE @EmployeeID as NVARCHAR(256)
DECLARE @Title as NVARCHAR(50)

DECLARE Employee_Cursor CURSOR FOR  
SELECT LoginID, JobTitle   
FROM AdventureWorks2012.HumanResources.Employee  
WHERE JobTitle = 'Marketing Specialist';  
OPEN Employee_Cursor;  
FETCH NEXT FROM Employee_Cursor INTO @EmployeeID, @Title;  
WHILE @@FETCH_STATUS = 0  
   BEGIN  
      Print '   ' + @EmployeeID + '      '+  @Title 
      FETCH NEXT FROM Employee_Cursor INTO @EmployeeID, @Title;  
   END;  
CLOSE Employee_Cursor;  
DEALLOCATE Employee_Cursor;  
GO 
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-simple-while-loop"></a>C. Boucle While simple  
 Dans l'exemple suivant, si le prix moyen d'un produit est inférieur à `$300`, la boucle `WHILE` double les prix puis sélectionne le prix maximum. Si le prix maximum est inférieur ou égal à `$500`, la boucle `WHILE` redémarre et double de nouveau les prix. Cette boucle continue à doubler les prix jusqu’à ce que le prix maximal soit supérieur à `$500`, puis le programme sort de la boucle `WHILE`.  
  
```sql  
-- Uses AdventureWorks  
  
WHILE ( SELECT AVG(ListPrice) FROM dbo.DimProduct) < $300  
BEGIN  
    UPDATE dbo.DimProduct  
        SET ListPrice = ListPrice * 2;  
    SELECT MAX ( ListPrice) FROM dbo.DimProduct  
    IF ( SELECT MAX (ListPrice) FROM dbo.DimProduct) > $500  
        BREAK;  
END  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [Langage de contrôle de flux &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [Curseurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/cursors-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)  
  
  


