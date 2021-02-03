---
description: binary et varbinary (Transact-SQL)
title: binary et varbinary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- binary_TSQL
- varbinary_TSQL
- binary
- varbinary
dev_langs:
- TSQL
helpviewer_keywords:
- varbinary data type
- binary [SQL Server], about binary data type
ms.assetid: bcce65f9-10db-4b3e-bfaf-dfc06c6f820f
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8d517738dd159716368af1ab000e1791376c89d2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206579"
---
# <a name="binary-and-varbinary-transact-sql"></a>binary et varbinary (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Données de type binaire de longueur fixe (binary) ou variable (varbinary).
  
## <a name="arguments"></a>Arguments

**binary** [ ( _n_ ) ] Données binaires de longueur fixe de _n_ octets, où _n_ est une valeur comprise entre 1 et 8 000. La taille de stockage est égale à _n_ octets.
  
**varbinary** [ ( _n_ | **max**) ] Données binaires de longueur variable. _n_ peut être une valeur comprise entre 1 et 8 000. **max** indique que la taille de stockage maximale est de 2^31-1 octets. La taille mémoire est la longueur réelle des données entrées, plus deux octets. Les données entrées peuvent avoir une longueur de 0 octet. Le synonyme SQL ANSI de **varbinary** est **binary varying**.
  
## <a name="remarks"></a>Remarques  
Quand la valeur de _n_ n’est pas spécifiée dans une définition de données ou une instruction de déclaration de variable, la longueur par défaut est 1. Quand la valeur de _n_ n’est pas précisée avec la fonction CAST, la longueur par défaut est 30.

| Type de données | Utilisation quand... |
| --- | --- |
| **binary** | les tailles des entrées de données de la colonne sont cohérentes.|
| **varbinary** | les tailles des entrées de données de la colonne varient considérablement.|
| **varbinary(max)** | la taille des entrées de données de la colonne dépasse 8 000 octets.|


## <a name="converting-binary-and-varbinary-data"></a>Conversion de données binary et varbinary
Lors de la conversion de données de type chaîne en type **binary** ou **varbinary** de longueur différente, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] complète ou tronque les données à droite. Ces types de données chaîne sont :

* **char** 
* **varchar**
* **nchar**
* **nvarchar**
* **binary**
* **varbinary**
* **text**
* **ntext**
* **image**

Quand d’autres types de données sont convertis en **binary** ou **varbinary**, les données sont complétées ou tronquées à gauche. Elles sont complétées avec des zéros hexadécimaux.
  
La conversion de données en types de données **binary** et **varbinary** est utile si les données **binary** constituent le moyen de déplacement de données le plus pratique. À un moment donné, vous pouvez convertir un type de valeur en une valeur binaire de taille suffisante et puis la reconvertir à nouveau. Cette conversion génère toujours des résultats de même valeur si les deux conversions s’effectuent sur la même version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La représentation binaire d'une valeur peut varier d'une version à l'autre de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
Vous pouvez convertir les types **int**, **smallint** et **tinyint** en type **binary** ou **varbinary**. Si vous reconvertissez la valeur **binary** en un entier, cette valeur sera différente de la valeur entière initiale s’il y a eu troncation. Par exemple, l’instruction SELECT suivante montre que la valeur entière `123456` est généralement stockée sous forme binaire `0x0001e240` :
  
```sql
SELECT CAST( 123456 AS BINARY(4) );  
```  
  
Cependant, l’instruction `SELECT` suivante montre que si la cible **binary** est trop petite pour traiter la valeur entière, les chiffres les plus à gauche sont tronqués discrètement de façon à ce que le même nombre soit stocké en tant que `0xe240` :
  
```sql
SELECT CAST( 123456 AS BINARY(2) );  
```  
  
L'instruction suivante montre que la troncation discrète peut affecter les opérations arithmétiques sans engendrer d'erreur :
  
```sql
DECLARE @BinaryVariable2 BINARY(2);  
  
SET @BinaryVariable2 = 123456;  
SET @BinaryVariable2 = @BinaryVariable2 + 1;  
  
SELECT CAST( @BinaryVariable2 AS INT);  
GO  
```  
  
Le résultat final est `57921`, et non pas `123457`.
  
> [!NOTE]  
>  Les conversions entre un type de données quelconque et les types de données **binary** ne sont pas nécessairement identiques dans toutes les versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Conversion de type de données &#40;moteur de base de données&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
  
  
