---
description: Fonctions de classement - TERTIARY_WEIGHTS (Transact-SQL)
title: TERTIARY_WEIGHTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TERTIARY_WEIGHTS_TSQL
- TERTIARY_WEIGHTS
dev_langs:
- TSQL
helpviewer_keywords:
- weights [SQL Server]
- SQL tertiary collations
- TERTIARY_WEIGHTS function
ms.assetid: 7e1f5350-260b-4c61-8c84-69bb1a214f1f
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6d5b6e88f49b2452184a77ebd103e5050efe33a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209993"
---
# <a name="collation-functions---tertiary_weights-transact-sql"></a>Fonctions de classement - TERTIARY_WEIGHTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Pour chaque caractère d’une expression de chaîne non Unicode définie avec un classement SQL tertiaire, cette fonction retourne une chaîne binaire de poids.
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
TERTIARY_WEIGHTS( non_Unicode_character_string_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*non_Unicode_character_string_expression*  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de chaîne de type **char**, **varchar** ou **varchar(max)** définie sur un classement SQL tertiaire. Pour obtenir la liste de ces classements, consultez Remarques.
  
## <a name="return-types"></a>Types de retour
`TERTIARY_WEIGHTS` retourne **varbinary** quand *non_Unicode_character_string_expression* est de type **char** ou **varchar**, et elle retourne **varbinary(max)** quand la valeur *non_Unicode_character_string_expression* est un type de données **varchar(max)**.
  
## <a name="remarks"></a>Remarques  
`TERTIARY_WEIGHTS` retourne NULL quand le classement tertiaire SQL ne définit pas la valeur *non_Unicode_character_string_expression*. Le tableau suivant présente les classements tertiaires SQL :
  
|ID d'ordre de tri|classement SQL|  
|---|---|
|33|SQL_Latin1_General_Pref_CP437_CI_AS|  
|34|SQL_Latin1_General_CP437_CI_AI|  
|43|SQL_Latin1_General_Pref_CP850_CI_AS|  
|44|SQL_Latin1_General_CP850_CI_AI|  
|49|SQL_1xCompat_CP850_CI_AS|  
|53|SQL_Latin1_General_Pref_CP1_CI_AS|  
|54|SQL_Latin1_General_CP1_CI_AI|  
|56|SQL_AltDiction_Pref_CP850_CI_AS|  
|57|SQL_AltDiction_CP850_CI_AI|  
|58|SQL_Scandinavian_Pref_CP850_CI_AS|  
|82|SQL_Latin1_General_CP1250_CI_AS|  
|84|SQL_Czech_CP1250_CI_AS|  
|86|SQL_Hungarian_CP1250_CI_AS|  
|88|SQL_Polish_CP1250_CI_AS|  
|90|SQL_Romanian_CP1250_CI_AS|  
|92|SQL_Croatian_CP1250_CI_AS|  
|94|SQL_Slovak_CP1250_CI_AS|  
|96|SQL_Slovenian_CP1250_CI_AS|  
|106|SQL_Latin1_General_CP1251_CI_AS|  
|108|SQL_Ukrainian_CP1251_CI_AS|  
|113|SQL_Latin1_General_CP1253_CS_AS|  
|114|SQL_Latin1_General_CP1253_CI_AS|  
|130|SQL_Latin1_General_CP1254_CI_AS|  
|146|SQL_Latin1_General_CP1256_CI_AS|  
|154|SQL_Latin1_General_CP1257_CI_AS|  
|156|SQL_Estonian_CP1257_CI_AS|  
|158|SQL_Latvian_CP1257_CI_AS|  
|160|SQL_Lithuanian_CP1257_CI_AS|  
|183|SQL_Danish_Pref_CP1_CI_AS|  
|184|SQL_SwedishPhone_Pref_CP1_CI_AS|  
|185|SQL_SwedishStd_Pref_CP1_CI_AS|  
|186.|SQL_Icelandic_Pref_CP1_CI_AS|  
  
Utilisez `TERTIARY_WEIGHTS` pour la définition d’une colonne calculée définie sur les valeurs d’une colonne de type **char**, **varchar** ou **varchar(max)**. La définition d’index sur la colonne calculée et sur la colonne de type **char**, **varchar** ou **varchar(max)** peut améliorer les performances quand la clause ORDER BY d’une requête spécifie cette colonne de type **char**, **varchar** ou **varchar(max)**.
  
## <a name="examples"></a>Exemples  
L’exemple suivant crée, dans une table, une colonne calculée qui applique la fonction `TERTIARY_WEIGHTS` aux valeurs d’une colonne de type `char` :
  
```sql
CREATE TABLE TertColTable  
(Col1 char(15) COLLATE SQL_Latin1_General_Pref_CP437_CI_AS,  
Col2 AS TERTIARY_WEIGHTS(Col1));  
GO   
```  
  
## <a name="see-also"></a>Voir aussi
[Clause ORDER BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md)
  
  
