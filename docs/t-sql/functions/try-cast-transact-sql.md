---
description: TRY_CAST (Transact-SQL)
title: TRY_CAST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TRY_CAST_TSQL
- TRY_CAST
dev_langs:
- TSQL
helpviewer_keywords:
- TRY_CAST function
ms.assetid: ea3a16de-995b-415c-b5f0-9355cf7bb401
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current||>= sql-server-2016 ||>= sql-server-linux-2017||>= aps-pdw-2016||= azure-sqldw-latest
ms.openlocfilehash: d431626368ec87622bed482ca19bade8c4700231
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211340"
---
# <a name="try_cast-transact-sql"></a>TRY_CAST (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne une valeur convertie en type de données spécifié si la conversion aboutit ; sinon, retourne NULL.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
TRY_CAST ( expression AS data_type [ ( length ) ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *expression*  
 Valeur à caster. Toute expression valide.  
  
 *data_type*  
 Type de données vers lequel effectuer le transtypage d’*expression*.  
  
 *length*  
 Entier facultatif qui spécifie la longueur du type de données cible.  
  
 La plage des valeurs acceptables est déterminée par la valeur de *data_type*.  
  
## <a name="return-types"></a>Types de retour  
 Retourne une valeur convertie en type de données spécifié si la conversion aboutit ; sinon, retourne NULL.  
  
## <a name="remarks"></a>Remarques  
 **TRY_CAST** prend la valeur qui lui est transmise et tente de la convertir vers le type *data_type* spécifié. Si le transtypage réussit, **TRY_CAST** renvoie la valeur dans le type *data_type* spécifié ; si une erreur se produit, la valeur NULL est renvoyée. Toutefois, si vous demandez une conversion qui n’est pas autorisée explicitement, **TRY_CAST** échoue avec une erreur.  
  
 **TRY_CAST** n’est pas un nouveau mot clé réservé et est disponible à tous les niveaux de compatibilité. **TRY_CAST** a la même sémantique que **TRY_CONVERT** lors de la connexion à des serveurs distants.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-try_cast-returns-null"></a>R. TRY_CAST retourne une valeur Null  
 L'exemple suivant montre que TRY_CAST retourne Null lorsque la conversion échoue.  
  
```sql  
SELECT   
    CASE WHEN TRY_CAST('test' AS float) IS NULL   
    THEN 'Cast failed'  
    ELSE 'Cast succeeded'  
END AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
------------  
Cast failed  
  
(1 row(s) affected)  
```  
  
 L'exemple suivant montre que l'expression doit être au format attendu.  
  
```sql  
SET DATEFORMAT dmy;  
SELECT TRY_CAST('12/31/2010' AS datetime2) AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
----------------------  
NULL  
  
(1 row(s) affected)  
```  
  
### <a name="b-try_cast-fails-with-an-error"></a>B. TRY_CAST échoue avec une erreur  
 L'exemple suivant montre que TRY_CAST retourne une erreur lorsque la conversion n'est pas autorisée explicitement.  
  
```sql  
SELECT TRY_CAST(4 AS xml) AS Result;  
GO  
```  
  
 Le résultat de cette instruction est une erreur, car un entier ne peut pas être converti en un type de données XML.  
  
```  
Explicit conversion from data type int to xml is not allowed.  
```  
  
### <a name="c-try_cast-succeeds"></a>C. TRY-CAST réussit  
 Cet exemple suivant montre que l'expression doit être au format attendu.  
  
```sql
SET DATEFORMAT mdy;  
SELECT TRY_CAST('12/31/2010' AS datetime2) AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
----------------------------------  
2010-12-31 00:00:00.0000000  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [TRY_CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/try-convert-transact-sql.md)   
 [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
  
