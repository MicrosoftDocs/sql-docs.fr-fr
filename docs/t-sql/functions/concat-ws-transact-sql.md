---
description: CONCAT_WS (Transact-SQL)
title: CONCAT_WS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/25/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- CONCAT_WS
- CONCAT_WS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CONCAT_WS function
ms.assetid: f1375fd7-a2fd-48bf-922a-4f778f0deb1f
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: dd7dec83f385c17055548626b2232e65af02ab09
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101295"
---
# <a name="concat_ws-transact-sql"></a>CONCAT_WS (Transact-SQL)
[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

Cette fonction retourne une chaîne qui résulte de la concaténation ou de la jointure de deux valeurs de chaîne ou plus, de bout en bout. Elle sépare ces valeurs de chaîne concaténées avec le délimiteur spécifié dans le premier argument de la fonction. (`CONCAT_WS` indique *concaténer avec un séparateur*.)

##  <a name="syntax"></a>Syntaxe   
```syntaxsql
CONCAT_WS ( separator, argument1, argument2 [, argumentN]... )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*separator* Expression de tout type de caractère (`char`, `nchar`, `nvarchar` ou `varchar`).

*argument1, argument2, argumentN* Expression de tout type. La fonction `CONCAT_WS` nécessite au moins deux arguments et pas plus de 254 arguments.

## <a name="return-types"></a>Types de retour
Valeur de chaîne dont la longueur et le type dépendent de l’entrée.

## <a name="remarks"></a>Notes   
`CONCAT_WS` accepte un nombre variable d’arguments de chaîne et les concatène (ou les joint) en une seule chaîne. Elle sépare ces valeurs de chaîne concaténées avec le délimiteur spécifié dans le premier argument de la fonction. `CONCAT_WS` nécessite un argument de séparateur et un minimum de deux autres arguments de valeur de chaîne ; sinon, `CONCAT_WS` génère une erreur. `CONCAT_WS` convertit implicitement tous les arguments en types chaîne avant la concaténation. 

La conversion implicite en chaînes respecte les règles existantes de conversion de type de données. Consultez [CONCAT (Transact-SQL)](../../t-sql/functions/concat-transact-sql.md) pour plus d’informations sur le comportement et les conversions de type de données.

### <a name="treatment-of-null-values"></a>Traitement des valeurs NULL

`CONCAT_WS` ignore le paramètre `SET CONCAT_NULL_YIELDS_NULL {ON|OFF}`.

Si `CONCAT_WS` reçoit des arguments dont toutes les valeurs sont NULL, elle retourne une chaîne vide de type varchar(1).

`CONCAT_WS` ignore les valeurs NULL durant la concaténation et n’ajoute pas le séparateur entre les valeurs NULL. Par conséquent, `CONCAT_WS` peut gérer correctement la concaténation des chaînes avec des valeurs « vides », par exemple, un deuxième champ d’adresse. Consultez l’exemple B pour plus d’informations.

Si un scénario implique des valeurs NULL séparées par un délimiteur, utilisez la fonction `ISNULL`. Consultez l’exemple C pour plus d’informations.

## <a name="examples"></a>Exemples   

### <a name="a--concatenating-values-with-separator"></a>R.  Concaténer des valeurs avec séparateur
Cet exemple concatène trois colonnes de la table sys.databases, en séparant les valeurs par `-`.   

```sql
SELECT CONCAT_WS( ' - ', database_id, recovery_model_desc, containment_desc) AS DatabaseInfo
FROM sys.databases;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   

|DatabaseInfo |  
|---------|
|1 - SIMPLE - NONE |
|2 - SIMPLE - NONE |
|3 - FULL - NONE |
|4 - SIMPLE - NONE |


### <a name="b--skipping-null-values"></a>B.  Ignorer les valeurs NULL
Cet exemple ignore les valeurs `NULL` dans la liste des arguments.

```sql
SELECT CONCAT_WS(',','1 Microsoft Way', NULL, NULL, 'Redmond', 'WA', 98052) AS Address;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   

```
Address
------------   
1 Microsoft Way,Redmond,WA,98052
```

### <a name="c--generating-csv-file-from-table"></a>C.  Générer un fichier CSV à partir de la table
Cet exemple utilise une virgule `,` comme séparateur et ajoute le caractère de retour chariot `char(13)` dans le format de valeurs séparées par des colonnes du jeu de résultats.

```sql
SELECT 
STRING_AGG(CONCAT_WS( ',', database_id, recovery_model_desc, containment_desc), char(13)) AS DatabaseInfo
FROM sys.databases
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   

```
DatabaseInfo
------------   
1,SIMPLE,NONE
2,SIMPLE,NONE
3,FULL,NONE 
4,SIMPLE,NONE 
```

CONCAT_WS ignore les valeurs NULL dans les colonnes. Agencez une colonne de valeurs NULL avec la fonction `ISNULL` et fournissez une valeur par défaut. Pour plus d’informations, consultez cet exemple :

```sql
SELECT 
STRING_AGG(CONCAT_WS( ',', database_id, ISNULL(recovery_model_desc,''), ISNULL(containment_desc,'N/A')), char(13)) AS DatabaseInfo
FROM sys.databases;
```

## <a name="see-also"></a>Voir aussi
 [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  

