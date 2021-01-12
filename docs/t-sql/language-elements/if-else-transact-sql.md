---
title: IF...ELSE (Transact-SQL) | Microsoft Docs
description: Référence du langage Transact-SQL pour les instructions IF-ELSE afin de fournir le flux de contrôle dans les instructions Transact-SQL.
ms.date: 07/11/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IF_TSQL
- IF
dev_langs:
- TSQL
helpviewer_keywords:
- IF...ELSE keyword
- ELSE (IF...ELSE) keyword
- ELSE keyword
- IF keyword
ms.assetid: 676c881f-dee1-417a-bc51-55da62398e81
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c8e21beb67e0ed283f463fff629776f33c1b07a0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98085302"
---
# <a name="ifelse-transact-sql"></a>IF...ELSE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Impose les conditions d'exécution d'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qui suit le mot clé IF et sa condition est exécutée si la condition est remplie, c'est-à-dire lorsque l'expression booléenne retourne la valeur TRUE. Le mot clé facultatif ELSE introduit une autre instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qui est exécutée lorsque la condition IF n'est pas remplie : l'expression booléenne retourne alors la valeur FALSE.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
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
 Expression qui renvoie TRUE ou FALSE. Si l'expression booléenne contient une instruction SELECT, cette dernière doit être mise entre parenthèses.  
  
 { *sql_statement*| *statement_block* }  
 Représente toute instruction ou tout groupe d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] tel que défini dans un bloc d'instructions. À moins que vous n'utilisiez un bloc d'instructions, la condition IF ou ELSE ne peut affecter les performances que d'une seule instruction [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Pour définir un bloc d'instructions, utilisez les mots clés de contrôle de flux BEGIN et END.  
  
## <a name="remarks"></a>Notes  
 Une construction IF...ELSE peut être utilisée dans des traitements d'instructions, des procédures stockées et des requêtes ad hoc. Lorsque cette construction est utilisée dans une procédure stockée, elle est souvent utilisée pour rechercher la présence de certains paramètres.  
  
 Les tests de la condition IF peuvent être imbriqués après une autre condition IF ou après une condition ELSE. La limite concernant le nombre de niveaux imbriqués dépend de la mémoire disponible.  
  
## <a name="example"></a>Exemple  
  
```sql
IF DATENAME(weekday, GETDATE()) IN (N'Saturday', N'Sunday')
       SELECT 'Weekend';
ELSE 
       SELECT 'Weekday';
```  
  
 Pour plus d’exemples, consultez [ELSE &#40;IF...ELSE&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/else-if-else-transact-sql.md).  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L’exemple suivant utilise `IF...ELSE` pour déterminer quelle réponse donner à l’utilisateur entre deux réponses, en fonction du poids d’un élément dans la table `DimProduct`.  
  
```sql
-- Uses AdventureWorksDW  

DECLARE @maxWeight FLOAT, @productKey INTEGER  
SET @maxWeight = 100.00  
SET @productKey = 424  
IF @maxWeight <= (SELECT Weight from DimProduct WHERE ProductKey = @productKey)   
    SELECT @productKey AS ProductKey, EnglishDescription, Weight, 'This product is too heavy to ship and is only available for pickup.' 
        AS ShippingStatus
    FROM DimProduct WHERE ProductKey = @productKey
ELSE  
    SELECT @productKey AS ProductKey, EnglishDescription, Weight, 'This product is available for shipping or pickup.' 
        AS ShippingStatus
    FROM DimProduct WHERE ProductKey = @productKey
```  
  
## <a name="see-also"></a>Voir aussi  
 [BEGIN...END &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-end-transact-sql.md)   
 [END &#40;BEGIN...END&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/end-begin-end-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHILE &#40;Transact-SQL&#41;](../../t-sql/language-elements/while-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [Control-of-Flow Language &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md) [ELSE &#40;IF...ELSE&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/else-if-else-transact-sql.md) 
