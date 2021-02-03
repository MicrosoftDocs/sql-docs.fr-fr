---
description: Constantes (Transact-SQL)
title: Constantes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- uniqueidentifier data type
- bit data type
- constants [SQL Server]
- scalar values
- money data type, constants
- binary [SQL Server], constants
- float data type
- Unicode [SQL Server], constants
- constants [Transact-SQL]
- integer constants
- decimal data type, constants
- character strings [SQL Server], constants
- positive values [SQL Server]
- formats [SQL Server], constants
- datetime data type [SQL Server]
- literals [SQL Server], constants
- real data type
- negative values
ms.assetid: 58ae3ff3-b1d5-41b2-9a2f-fc7ab8c83e0e
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 62c93d062c5903deb2e3dd291ba26953bcfb00be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172596"
---
# <a name="constants-transact-sql"></a>Constantes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Une constante, également appelée valeur littérale ou scalaire, est un symbole représentant une valeur de donnée spécifique. Le format d'une constante dépend du type de données dont elle représente la valeur.
  
## <a name="character-string-constants"></a>Constantes de type chaîne de caractères
Une constante de type chaîne de caractères est placée entre des guillemets simples. Elle se compose de caractères alphanumériques (a-z, A-Z et 0-9) ainsi que de caractères spéciaux tels que le point d'exclamation (!), l'arobase (@) et le dièse (#). Le classement par défaut de la base de données active est attribué aux constantes de la chaîne de caractères. Si la clause COLLATE est utilisée, la conversion vers la page de codes par défaut de la base de données se produit toujours avant la conversion vers le classement spécifié par la clause COLLATE. Les chaînes de caractères tapées par l'utilisateur sont analysées au moyen de la page de codes de l'ordinateur et sont converties, le cas échéant, dans la page de codes par défaut de la base de données.

> [!NOTE]
> Lorsqu’un [classement UTF8](../../relational-databases/collations/collation-and-unicode-support.md#utf8) est spécifié à l’aide de la clause COLLATE, la conversion vers la page de code par défaut de la base de données se produit toujours avant la conversion du classement spécifié par la clause COLLATE. La conversion n’est pas effectuée directement vers le classement Unicode spécifié. Pour plus d'informations, consultez [Chaîne Unicode](#unicode-strings).
  
Si l'option QUOTED_IDENTIFIER a été désactivée (OFF) pour une connexion, les chaînes de caractères peuvent également être placées entre des guillemets doubles. Notez que [OLE DB Driver pour SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) et [ODBC Driver pour SQL Server](../../connect/odbc/download-odbc-driver-for-sql-server.md) Microsoft utilisent automatiquement `SET QUOTED_IDENTIFIER ON`. Il est recommandé d'utiliser des guillemets simples.
  
Si un guillemet simple est inséré dans une chaîne de caractères placée entre des guillemets simples, celui-ci doit être représenté par deux guillemets simples. Ceci n'est pas nécessaire dans une chaîne insérée entre des guillemets doubles.
  
Voici des exemples de chaînes de caractères :
  
```
'Cincinnati'  
'O''Brien'  
'Process X is 50% complete.'  
'The level for job_id: %d should be between %d and %d.'  
"O'Brien"  
```  
  
Les chaînes vides sont représentées par deux guillemets simples, sans aucun caractère ou espace entre eux. En mode de compatibilité 6.x, une chaîne vide est traitée comme un espace simple.
  
Les constantes de chaînes de caractères prennent en charge les [classements](../../relational-databases/collations/collation-and-unicode-support.md) améliorés.
  
> [!NOTE]  
> Les constantes de caractères dont la taille est supérieure à 8 000 octets sont des données de type **varchar(max)**.  
  
## <a name="unicode-strings"></a>Chaînes Unicode
Une chaîne Unicode a le même format qu'une chaîne de caractères, mais celle-ci est précédée de l'identificateur N (N correspond à National Language dans la norme SQL-92). 

> [!IMPORTANT]  
> Le préfixe N doit être en majuscule. 

Par exemple, `'Michél'` est une constante de type caractère tandis que `N'Michél'` est une constante Unicode. Les constantes Unicode sont interprétées comme des données Unicode et ne sont pas évaluées à l'aide d'une page de codes. Les constantes Unicode présentent un classement, qui contrôle essentiellement les comparaisons et le respect de la casse. Le classement par défaut de la base de données active est attribué aux constantes Unicode. Si la clause COLLATE est utilisée, la conversion vers le classement par défaut de la base de données se produit toujours avant la conversion vers le classement spécifié par la clause COLLATE. Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md#storage_differences).
  
Les constantes de chaînes Unicode prennent en charge les classements évolués.
  
> [!NOTE]  
> Les constantes Unicode dont la taille est supérieure à 8 000 octets sont des données de type **nvarchar(max)**.  
  
## <a name="binary-constants"></a>Constantes binaires
Une constante binaire a le préfixe `0x` et se compose d'une chaîne de nombres hexadécimaux. La chaîne n'est pas entourée de guillemets.
  
Voici des exemples de chaînes binaires :
  
```
0xAE  
0x12Ef  
0x69048AEFDD010E  
0x  (empty binary string)  
```  
  
> [!NOTE]  
>  Les constantes binaires dont la taille est supérieure à 8 000 octets sont des données de type **varbinary(max)**.  
  
## <a name="bit-constants"></a>Constantes de type bit
Les constantes de type **bit** sont représentées par les nombres 0 ou 1, et elles ne sont pas entourées de guillemets. Si un nombre supérieur à 1 est utilisé, il est converti en 1.
  
## <a name="datetime-constants"></a>Constantes datetime
Les constantes **datetime** sont représentées à l’aide de valeurs de date libellées avec des caractères dans un format spécifique. Elles sont entourées de guillemets simples.
  
Voici des exemples de constantes **datetime** :
  
```
'December 5, 1985'  
'5 December, 1985'  
'851205'  
'12/5/98'  
```  
  
Exemples de constantes datetime :
  
```
'14:30:24'  
'04:24 PM'  
```  
  
## <a name="integer-constants"></a>constantes entières
Les constantes **entières** sont représentées par une chaîne de nombres qui ne sont pas entourés de guillemets et qui ne contiennent pas de virgule décimale. Les constantes **entières** doivent être des nombres entiers et ne peuvent pas contenir de décimales.
  
Voici des exemples de constantes **entières** :
  
```
1894  
2  
```  
  
## <a name="decimal-constants"></a>Constantes décimales
Les constantes **décimales** sont représentées par une chaîne de nombres qui ne sont pas entourés de guillemets et qui contiennent une virgule décimale.
  
Voici des exemples de constantes **décimales** :
  
```
1894.1204  
2.0  
```  
  
## <a name="float-and-real-constants"></a>Constantes float et real
Les constantes de type **float** et **real** sont représentées par la notation scientifique.
  
Voici des exemples de valeurs **float** ou **real** :
  
```
101.5E5  
0.5E-2  
```  
  
## <a name="money-constants"></a>Constantes money
Les constantes de type **money** sont représentées par une chaîne de nombres avec une virgule décimale facultative et peuvent être précédées d’un symbole monétaire. Les constantes **money** n’apparaissent pas entre guillemets.
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'impose aucune règle de regroupement, telle que l'insertion d'une virgule (,) tous les trois chiffres dans les chaînes qui représentent une valeur monétaire.
  
> [!NOTE]  
>  Les virgules sont ignorées partout dans la valeur littérale **money** spécifiée.  
  
Voici des exemples de constantes **money** :
  
```
$12  
$542023.14  
```  
  
## <a name="uniqueidentifier-constants"></a>Constantes uniqueidentifier
Les constantes de type **uniqueidentifier** correspondent à une chaîne représentant un GUID. Elles peuvent être spécifiées dans un format de chaîne de caractères ou binaire.
  
Les deux exemples ci-après spécifient le même identificateur globalement unique (GUID) :
  
```
'6F9619FF-8B86-D011-B42D-00C04FC964FF'  
0xff19966f868b11d0b42d00c04fc964ff  
```  
  
## <a name="specifying-negative-and-positive-numbers"></a>Définition d'un nombre négatif ou positif  
Pour indiquer si un nombre est positif ou négatif, appliquez l’opérateur unaire **+** ou **-** à une constante numérique. Ceci crée une expression numérique représentant la valeur numérique signée. Quand l’opérateur unaire **+** ou **-** n’est pas appliqué, une constante numérique est positive.
  
Expressions **integer** signées :  
  
```
+145345234
-2147483648
```
Expressions **decimal** signées :  
  
```
+145345234.2234
-2147483648.10
```
  
Expressions **float** signées :  
  
```
+123E-3
-12E5
```
  
Expressions **money** signées :  
  
```
-$45.56
+$423456.99
```
  
## <a name="enhanced-collations"></a>Classements évolués  
[!INCLUDE[ssde_md](../../includes/ssde_md.md)] prend en charge les constantes de type caractère et chaîne Unicode qui prennent en charge des classements améliorés. Pour plus d’informations, consultez la clause [COLLATE &#40;Transact-SQL&#41;](../../t-sql/statements/collations.md).
  
## <a name="see-also"></a>Voir aussi
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
[Opérateurs &#40;Transact-SQL &#41;](../../t-sql/language-elements/operators-transact-sql.md)
[Support du classement et Unicode](../../relational-databases/collations/collation-and-unicode-support.md)  
[Priorité de classement](../../t-sql/statements/collation-precedence-transact-sql.md)    
  
