---
description: Fonctions logiques - IIF (Transact-SQL)
title: IIF (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IIF_TSQL
- IIF
dev_langs:
- TSQL
helpviewer_keywords:
- IIF function
ms.assetid: e3ccf8ed-1cec-43ac-90b7-d8597c24b050
author: cawrites
ms.author: chadam
ms.openlocfilehash: 81b2e383d44386e14e6cca59ebf3973bc2dfc20c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208818"
---
# <a name="logical-functions---iif-transact-sql"></a>Fonctions logiques - IIF (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Retourne l'une des deux valeurs possibles, selon que l'expression booléenne renvoie true ou false dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
IIF( boolean_expression, true_value, false_value )
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *boolean_expression*  
 Expression booléenne valide.  
  
 Si cet argument n’est pas une expression booléenne, une erreur de syntaxe est générée.  
  
 *true_value*  
 Valeur à renvoyer si *boolean_expression* a la valeur true.  
  
 *false_value*  
 Valeur à renvoyer si *boolean_expression* a la valeur false.  
  
## <a name="return-types"></a>Types de retour  
 Renvoie le type de données ayant la priorité la plus élevée parmi les types dans *true_value* et *false_value*. Pour plus d’informations, consultez [Priorités des types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="remarks"></a>Remarques  
 IIF est un moyen rapide d'écrire une expression CASE. Elle évalue l'expression booléenne passée comme premier argument, puis retourne l'un des deux autres arguments selon le résultat de l'évaluation. Autrement dit, *true_value* est renvoyé si l’expression booléenne a pour valeur true, et *false_value* est retourné si l’expression booléenne a pour valeur false ou une valeur inconnue. *true_value* et *false_value* peuvent être de n’importe quel type. Les règles qui s'appliquent à l'expression CASE pour les expressions booléennes, la gestion de la valeur NULL et les types de retour s'appliquent également à IIF. Pour plus d’informations, consultez [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md).  
  
 Le fait que IIF soit convertie en CASE a également un impact sur d'autres aspects du comportement de cette fonction. Étant donné que les expressions CASE peuvent être imbriquées seulement jusqu'à un niveau de 10, les instructions IIF peuvent également être imbriquées uniquement jusqu'à un niveau maximal de 10. En outre, IIF est exécutée à distance sur d'autres serveurs en tant qu'expression CASE sémantiquement équivalente, avec tous les comportements d'une expression CASE exécutée à distance.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-simple-iif-example"></a>R. Exemple IIF simple  
  
```sql  
DECLARE @a INT = 45, @b INT = 40;
SELECT [Result] = IIF( @a > @b, 'TRUE', 'FALSE' );
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
--------  
TRUE  
```  
  
### <a name="b-iif-with-null-constants"></a>B. IIF avec des constantes NULL  
  
```sql 
SELECT [Result] = IIF( 45 > 30, NULL, NULL );
```  
  
 Le résultat de cette instruction est une erreur.  
  
### <a name="c-iif-with-null-parameters"></a>C. IIF avec des paramètres NULL  
  
```sql  
DECLARE @P INT = NULL, @S INT = NULL;  
SELECT [Result] = IIF( 45 > 30, @P, @S );
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
--------  
NULL  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [CHOOSE &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-choose-transact-sql.md)  
  
  
