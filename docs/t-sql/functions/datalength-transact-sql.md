---
description: DATALENGTH (Transact-SQL)
title: DATALENGTH (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/20/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATALENGTH_TSQL
- DATALENGTH
dev_langs:
- TSQL
helpviewer_keywords:
- number of bytes representing expression
- data types [SQL Server], length
- DATALENGTH function
- expressions [SQL Server], length
- lengths [SQL Server], data
ms.assetid: 00f377f1-cc3e-4eac-be47-b3e3f80267c9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6a047c6178f9defea3abca3a1526c4b11a8fce88
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183564"
---
# <a name="datalength-transact-sql"></a>DATALENGTH (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette fonction retourne le nombre d’octets utilisés pour représenter une expression.

> [!NOTE]
> Pour retourner le nombre de caractères dans une expression de chaîne, utilisez la fonction [LEN](../../t-sql/functions/len-transact-sql.md).
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DATALENGTH ( expression )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*expression*  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de n’importe quel type de données.
  
## <a name="return-types"></a>Types de retour
**bigint** si le type de données de *expression* est **nvarchar(max)**, **varbinary(max)** ou **varchar(max)**  ; sinon, **int**.
  
## <a name="remarks"></a>Remarques  
`DATALENGTH` devient vraiment utile lorsqu’il est utilisé avec des types de données qui peuvent stocker des données de longueur variable, telles que :
- **image**
- **ntext**
- **nvarchar**
- **text**
- **varbinary**
- **varchar**
  
Pour une valeur NULL, `DATALENGTH` retourne NULL.
  
> [!NOTE]  
> Les niveaux de compatibilité peuvent affecter les valeurs de retour. Pour plus d’informations sur les niveaux de compatibilité, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  

> [!NOTE]
> Utilisez [LEN](../../t-sql/functions/len-transact-sql.md) pour retourner le nombre de caractères encodés dans une expression de chaîne donnée et [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md) pour retourner la taille en octets d’une expression de chaîne donnée. Ces sorties peuvent différer selon le type de données et le type d’encodage utilisé dans la colonne. Pour plus d’informations sur les différences de stockage entre les différents types d’encodage, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md).

## <a name="examples"></a>Exemples  
Cet exemple recherche la longueur de la colonne `Name` dans la table `Product` :
  
```sql
USE AdventureWorks2016  
GO
SELECT length = DATALENGTH(EnglishProductName), EnglishProductName  
FROM dbo.DimProduct  
ORDER BY EnglishProductName;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi
[LEN &#40;Transact-SQL&#41;](../../t-sql/functions/len-transact-sql.md)  
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)
