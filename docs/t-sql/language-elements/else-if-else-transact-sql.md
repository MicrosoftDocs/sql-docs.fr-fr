---
description: ELSE (IF...ELSE) (Transact-SQL)
title: ELSE (IF...ELSE) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ELSE
- ELSE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ELSE (IF...ELSE) keyword
- ELSE keyword
- IF keyword
ms.assetid: 6f2b4278-0dea-4603-bbd3-7cbad602a645
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1217953d76c37fd4e03e41336ec2f6846fdd255b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102482"
---
# <a name="else-ifelse-transact-sql"></a>ELSE (IF...ELSE) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Impose les conditions d'exécution d'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]. L’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] (*sql_statement*) qui suit *Boolean_expression* est exécutée si *Boolean_expression* a la valeur TRUE. Le mot clé facultatif ELSE est une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] alternative exécutée lorsque la valeur renvoyée par *Boolean_expression* est FALSE ou NULL.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
IF Boolean_expression   
     { sql_statement | statement_block }   
[ ELSE   
     { sql_statement | statement_block } ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *Boolean_expression*  
 Expression qui renvoie TRUE ou FALSE. Si *Boolean_expression* contient une instruction SELECT, cette dernière doit être mise entre parenthèses.  
  
 { *sql_statement* | *statement_block* }  
 Toute instruction ou tout groupe d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] valide tel que défini dans un bloc d'instructions. Pour définir un bloc (traitement) d'instructions, utilisez les mots clé BEGIN et END du langage de contrôle de flux. Bien que toutes les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] soient valides à l’intérieur d’un bloc BEGIN…END, certaines instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ne doivent pas être regroupées dans un même lot (bloc d’instructions).  
  
## <a name="result-types"></a>Types des résultats  
 **Booléen**  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-a-simple-boolean-expression"></a>R. Utilisation d'une expression booléenne simple  
 L'exemple suivant comprend une expression booléenne simple (`1=1`) ayant la valeur true. Par conséquent, il imprime la première instruction.  
  
```sql
IF 1 = 1 PRINT 'Boolean_expression is true.'  
ELSE PRINT 'Boolean_expression is false.' ;  
```  
  
 L'exemple suivant comprend une expression booléenne simple (`1=2`) ayant la valeur false. Par conséquent, il imprime la seconde instruction.  
  
```sql
IF 1 = 2 PRINT 'Boolean_expression is true.'  
ELSE PRINT 'Boolean_expression is false.' ;  
GO  
```  
  
### <a name="b-using-a-query-as-part-of-a-boolean-expression"></a>B. Utilisation d'une requête dans le cadre d'une expression booléenne  
 L'exemple suivant exécute une requête dans le cadre de l'expression booléenne. Étant donné que 10 vélos de la table `Product` répondent à la clause `WHERE`, la première instruction print s'exécute. Remplacez `> 5` par `> 15` pour voir comment la deuxième partie de l'instruction pourrait s'exécuter.  
  
```sql
USE AdventureWorks2012;  
GO  
IF   
(SELECT COUNT(*) FROM Production.Product WHERE Name LIKE 'Touring-3000%' ) > 5  
PRINT 'There are more than 5 Touring-3000 bicycles.'  
ELSE PRINT 'There are 5 or less Touring-3000 bicycles.' ;  
GO  
```  
  
### <a name="c-using-a-statement-block"></a>C. Utilisation d'un bloc d'instructions  
 L'exemple suivant exécute une requête dans le cadre de l'expression booléenne, puis exécute des blocs d'instructions légèrement différents selon le résultat de l'expression booléenne. Chaque bloc d'instructions commence par `BEGIN` et se termine par `END`.  
  
```sql
USE AdventureWorks2012;  
GO  
DECLARE @AvgWeight DECIMAL(8,2), @BikeCount INT  
IF   
(SELECT COUNT(*) FROM Production.Product WHERE Name LIKE 'Touring-3000%' ) > 5  
BEGIN  
   SET @BikeCount =   
        (SELECT COUNT(*)   
         FROM Production.Product   
         WHERE Name LIKE 'Touring-3000%');  
   SET @AvgWeight =   
        (SELECT AVG(Weight)   
         FROM Production.Product   
         WHERE Name LIKE 'Touring-3000%');  
   PRINT 'There are ' + CAST(@BikeCount AS VARCHAR(3)) + ' Touring-3000 bikes.'  
   PRINT 'The average weight of the top 5 Touring-3000 bikes is ' + CAST(@AvgWeight AS VARCHAR(8)) + '.';  
END  
ELSE   
BEGIN  
SET @AvgWeight =   
        (SELECT AVG(Weight)  
         FROM Production.Product   
         WHERE Name LIKE 'Touring-3000%' );  
   PRINT 'Average weight of the Touring-3000 bikes is ' + CAST(@AvgWeight AS VARCHAR(8)) + '.' ;  
END ;  
GO  
```  
  
### <a name="d-using-nested-ifelse-statements"></a>D. Utilisation d'instructions IF...ELSE imbriquées  
 L’exemple suivant montre comment une instruction IF ... ELSE peut s’imbriquer dans une autre. Définissez la variable `@Number` à `5`, `50` et `500` pour tester chaque instruction.  
  
```sql
DECLARE @Number INT;  
SET @Number = 50;  
IF @Number > 100  
   PRINT 'The number is large.';  
ELSE   
   BEGIN  
      IF @Number < 10  
      PRINT 'The number is small.';  
   ELSE  
      PRINT 'The number is medium.';  
   END ;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-a-query-as-part-of-a-boolean-expression"></a>E. Utilisation d’une requête dans le cadre d’une expression booléenne  
 L’exemple suivant utilise `IF...ELSE` pour déterminer quelle réponse donner à l’utilisateur entre deux réponses, en fonction du poids d’un élément dans la table `DimProduct`.  
  
```sql
-- Uses AdventureWorks  
  
DECLARE @maxWeight FLOAT, @productKey INTEGER  
SET @maxWeight = 100.00  
SET @productKey = 424  
IF @maxWeight <= (SELECT Weight FROM DimProduct WHERE ProductKey=@productKey)   
    (SELECT @productKey, EnglishDescription, Weight, 'This product is too heavy to ship and is only available for pickup.' FROM DimProduct WHERE ProductKey=@productKey)  
ELSE  
    (SELECT @productKey, EnglishDescription, Weight, 'This product is available for shipping or pickup.' FROM DimProduct WHERE ProductKey=@productKey)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [Langage de contrôle de flux &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [IF...ELSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/if-else-transact-sql.md)  
  
  


