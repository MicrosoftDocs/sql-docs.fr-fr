---
description: BINARY_CHECKSUM  (Transact-SQL)
title: BINARY_CHECKSUM  (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BINARY_CHECKSUM
- BINARY_CHECKSUM_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- BINARY_CHECKSUM function
- binary [SQL Server], checksum values
ms.assetid: 07fece4d-58e3-446e-a3b5-92fe24d2d1fb
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d37596cba9b0786f6105a3e9f9c5c22a46eef7d0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193470"
---
# <a name="binary_checksum--transact-sql"></a>BINARY_CHECKSUM  (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Retourne la valeur de total de contrôle binaire calculée à partir d'une ligne d'une table ou d'une liste d'expressions.
  
![Icône Lien d’article](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien d’article") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
BINARY_CHECKSUM ( * | expression [ ,...n ] )   
```  
  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*\**  
Indique que le calcul englobe toutes les colonnes de la table. BINARY_CHECKSUM ignore les colonnes de types de données incomparables dans son calcul. Les types de données incomparables sont notamment  
* **cursor**  
* **image**  
* **ntext**  
* **text**  
* **xml**  

et les types définis par l’utilisateur CLR (Common Language Runtime) incomparables.
  
*expression*  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de tout type. BINARY_CHECKSUM ignore les expressions de types de données incomparables dans son calcul.

## <a name="return-types"></a>Types de retour  
 **int**
  
## <a name="remarks"></a>Notes  
`BINARY_CHECKSUM(*)`, calculé sur n’importe quelle ligne d’une table, retourne la même valeur tant que la ligne n’est pas modifiée. `BINARY_CHECKSUM` a les propriétés d'une fonction de hachage : lorsque c’est appliqué à deux listes d'expressions, la même valeur est retournée si les éléments correspondants dans les deux listes sont du même type et ont une valeur égale lorsqu'ils sont comparés à l'aide de l'opérateur d'égalité (=). Pour cette définition, nous disons que les valeurs NULL d’un type spécifié apparaissent comme étant équivalentes. Si au moins l’une des valeurs de la liste d’expressions change, la somme de contrôle des expressions peut également changer. Cependant, cette modification n’est pas garantie. Nous conseillons donc d’utiliser `BINARY_CHECKSUM` pour vérifier si des valeurs ont changé, uniquement si votre application peut accepter une modification parfois manquée. Sinon, envisagez d’utiliser `HASHBYTES` à la place. Avec un algorithme de hachage MD5 spécifié, la probabilité que `HASHBYTES` retourne le même résultat pour deux entrées différentes est beaucoup plus faible qu’avec `BINARY_CHECKSUM`.
  
`BINARY_CHECKSUM` peut opérer sur une liste d’expressions et retourne la même valeur pour une liste spécifiée. Lorsque la fonction `BINARY_CHECKSUM` porte sur deux listes d'expressions, elle retourne la même valeur si les éléments correspondants des deux listes sont de type et de représentation en octets identiques. Pour cette définition, les valeurs NULL d'un type spécifié sont considérées comme utilisant la même représentation en octets.
  
`BINARY_CHECKSUM` et `CHECKSUM` sont des fonctions similaires. elles peuvent être utilisées pour calculer une somme de contrôle dans une liste d'expressions, et l'ordre des expressions affecte la valeur résultante. L’ordre des colonnes utilisé pour `BINARY_CHECKSUM(*)` est celui spécifié dans la définition de la table ou de la vue, y compris les colonnes calculées.
  
`BINARY_CHECKSUM` et `CHECKSUM` retournent des valeurs différentes pour les types de données de chaîne. Avec les paramètres régionaux, des chaînes dont la représentation est différente peuvent apparaître comme étant équivalentes. Les types de données de chaîne sont  

* **char**  
* **nchar**  
* **nvarchar**  
* **varchar**  

ou  

* **sql_variant** (si le type de base de **sql_variant** est un type de données de chaîne).  
  
Par exemple, les valeurs `BINARY_CHECKSUM` des chaînes « McCavity » et « Mccavity » sont différentes. À l’inverse, `CHECKSUM` retourne les mêmes valeurs de somme de contrôle pour ces chaînes sur un serveur qui ne respecte pas la casse. Vous devez éviter de comparer les valeurs de `CHECKSUM` avec les valeurs de `BINARY_CHECKSUM`.
 
`BINARY_CHECKSUM` prend en charge n’importe quelle longueur de type **varbinary(max)** et jusqu’à 255 caractères de type **nvarchar(max)**.
  
## <a name="examples"></a>Exemples  
Cet exemple utilise `BINARY_CHECKSUM` pour détecter des modifications dans une ligne d’une table.
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE myTable (column1 INT, column2 VARCHAR(256));  
GO  
INSERT INTO myTable VALUES (1, 'test');  
GO  
SELECT BINARY_CHECKSUM(*) from myTable;  
GO  
UPDATE myTable set column2 = 'TEST';  
GO  
SELECT BINARY_CHECKSUM(*) from myTable;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi
[Fonctions d’agrégation &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[CHECKSUM_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-agg-transact-sql.md)  
[CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md)  
[HASHBYTES &#40;Transact-SQL&#41;](../../t-sql/functions/hashbytes-transact-sql.md)  
  
  
