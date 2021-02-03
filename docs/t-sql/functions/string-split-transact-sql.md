---
title: STRING_SPLIT (Transact-SQL) | Microsoft Docs
description: Référence Transact-SQL pour la fonction STRING_SPLIT. Cette fonction table divise une chaîne en sous-chaînes en fonction d’un délimiteur de caractère.
ms.date: 11/28/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STRING_SPLIT
- STRING_SPLIT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STRING_SPLIT function
ms.assetid: 3273dbf3-0b4f-41e1-b97e-b4f67ad370b9
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current||=azure-sqldw-latest||>= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: 472da04cc9a93a8f7846927ad9ae4595766e9116
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207890"
---
# <a name="string_split-transact-sql"></a>STRING_SPLIT (Transact-SQL)

[!INCLUDE[tsql-appliesto-ss2016-asdb-asdw-xxx-md.md](../../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)]

Fonction table qui divise une chaîne en lignes de sous-chaînes, en fonction d’un caractère de séparation spécifié.

#### <a name="compatibility-level-130"></a>Niveau de compatibilité 130

STRING_SPLIT nécessite que le niveau de compatibilité soit au moins 130. Quand le niveau est inférieur à 130, SQL Server ne peut pas trouver la fonction STRING_SPLIT.

Pour changer le niveau de compatibilité d’une base de données, consultez [Afficher ou changer le niveau de compatibilité d’une base de données](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md).

> [!NOTE]
> La configuration de la compatibilité n’est pas nécessaire pour STRING_SPLIT dans Azure Synapse Analytics.

![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  

```syntaxsql
STRING_SPLIT ( string , separator )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

 *string*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md) de n’importe quel type de caractère (par exemple, **nvarchar**, **varchar**, **nchar** ou **char**).  
  
 *separator*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md) d’un seul caractère de n’importe quel type de caractère (par exemple **nvarchar(1)** , **varchar(1)** , **nchar(1)** ou **char(1)** ) qui est utilisée comme séparateur pour les sous-chaînes concaténées.  
  
## <a name="return-types"></a>Types de retour  

Retourne une table d’une seule colonne dont les lignes sont les sous-chaînes. Le nom de la colonne est **value**. Retourne **nvarchar** si un des arguments d’entrée est de type **nvarchar** ou **nchar**. Sinon, retourne **varchar**. La longueur du type de retour est identique à la longueur de l’argument de chaîne.  
  
## <a name="remarks"></a>Notes  

**STRING_SPLIT** prend en entrée une chaîne qui a des sous-chaînes délimitées, et un caractère à utiliser comme délimiteur ou séparateur. STRING_SPLIT produit une table à une seule colonne dont les lignes contiennent les sous-chaînes. Le nom de la colonne de sortie est **value**.

Les lignes résultantes peuvent être dans n’importe quel ordre. Il n’est _pas_ garanti que l’ordre corresponde à l’ordre des sous-chaînes dans la chaîne en entrée. Vous pouvez remplacer l’ordre de tri final avec une clause ORDER BY sur l’instruction SELECT (`ORDER BY value`).

0x0000 (**char(0)** ) est un caractère non défini dans les classements Windows et ne peut pas être inclus dans STRING_SPLIT.

Les sous-chaînes vides de longueur nulle sont présentes quand la chaîne en entrée contient plusieurs occurrences consécutives du caractère délimiteur. Les sous-chaînes vides sont traitées de la même façon que les sous-chaînes contenant du texte. Vous pouvez filtrer les lignes contenant la sous-chaîne vide avec la clause WHERE (`WHERE value <> ''`). Si la chaîne en entrée est NULL, la fonction table STRING_SPLIT retourne une table vide.  

Par exemple, l’instruction SELECT suivante utilise le caractère « espace » comme séparateur :

```sql
SELECT value FROM STRING_SPLIT('Lorem ipsum dolor sit amet.', ' ');
```

À titre d’exemple, l’instruction SELECT précédente a retourné la table de résultats suivante :  
  
|value|  
| :-- |  
|Lorem|  
|ipsum|  
|dolor|  
|sit|  
|amet.|  
| &nbsp; |

## <a name="examples"></a>Exemples  
  
### <a name="a-split-comma-separated-value-string"></a>R. Diviser une chaîne de valeurs séparées par des virgules

Analysez une liste de valeurs séparées par des virgules et retournez tous les jetons non vides :  

```sql
DECLARE @tags NVARCHAR(400) = 'clothing,road,,touring,bike'  
  
SELECT value  
FROM STRING_SPLIT(@tags, ',')  
WHERE RTRIM(value) <> '';
```

STRING_SPLIT retourne une chaîne vide si aucun élément ne figure entre les séparateurs. La condition RTRIM(value) <> '' supprime les jetons vides.  
  
### <a name="b-split-comma-separated-value-string-in-a-column"></a>B. Diviser une chaîne de valeurs séparées par des virgules dans une colonne

La table de produits a une colonne avec une liste de balises séparées par des virgules, illustrée dans l’exemple suivant :  
  
|ProductId|Nom|Balises|  
|---------------|----------|----------|  
|1|Full-Finger Gloves|clothing,road,touring,bike|  
|2|LL Headset|bike|  
|3|HL Mountain Frame|bike,mountain|  
  
La requête suivante transforme chaque liste de balises et les joint à la ligne d’origine :  

```sql  
SELECT ProductId, Name, value  
FROM Product  
    CROSS APPLY STRING_SPLIT(Tags, ',');  
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|ProductId|Nom|value|  
|---------------|----------|-----------|  
|1|Full-Finger Gloves|clothing|  
|1|Full-Finger Gloves|Route|  
|1|Full-Finger Gloves|touring|  
|1|Full-Finger Gloves|bike|  
|2|LL Headset|bike|  
|3|HL Mountain Frame|bike|  
|3|HL Mountain Frame|mountain|  

  >[!NOTE]
  > L’ordre de la sortie peut varier, car il n’est _pas_ garanti que l’ordre corresponde à l’ordre des sous-chaînes dans la chaîne d’entrée.
  
### <a name="c-aggregation-by-values"></a>C. Agrégation par valeurs

Les utilisateurs doivent créer un rapport qui indique le nombre de produits pour chaque balise, classés par nombre de produits, afin de filtrer uniquement les balises avec plus de deux produits.  

```sql  
SELECT value as tag, COUNT(*) AS [Number of articles]  
FROM Product  
    CROSS APPLY STRING_SPLIT(Tags, ',')  
GROUP BY value  
HAVING COUNT(*) > 2  
ORDER BY COUNT(*) DESC;  
```

### <a name="d-search-by-tag-value"></a>D. Rechercher par valeur de balise

Les développeurs doivent créer des requêtes qui recherchent les articles par mots clés. Ils peuvent utiliser les requêtes suivantes :  
  
Pour rechercher les produits avec une seule balise (clothing) :  

```sql
SELECT ProductId, Name, Tags  
FROM Product  
WHERE 'clothing' IN (SELECT value FROM STRING_SPLIT(Tags, ','));  
```

Recherchez les produits avec deux balises spécifiées (clothing et road) :  

```sql  
SELECT ProductId, Name, Tags  
FROM Product  
WHERE EXISTS (SELECT *  
    FROM STRING_SPLIT(Tags, ',')  
    WHERE value IN ('clothing', 'road'));  
```

### <a name="e-find-rows-by-list-of-values"></a>E. Rechercher des lignes par liste de valeurs

Les développeurs doivent créer une requête qui recherche des articles selon une liste d’ID. Ils peuvent utiliser la requête suivante :  

```sql  
SELECT ProductId, Name, Tags  
FROM Product  
JOIN STRING_SPLIT('1,2,3',',')
    ON value = ProductId;  
```  

L’utilisation de STRING_SPLIT précédente est un remplacement pour un antimodèle courant. Un tel antimodèle peut impliquer la création d’une chaîne SQL dynamique dans la couche application ou dans Transact-SQL. Un antimodèle peut aussi être obtenu avec l’opérateur LIKE. Considérez l’exemple d’instruction SELECT suivante :

```sql  
SELECT ProductId, Name, Tags  
FROM Product  
WHERE ',1,2,3,' LIKE '%,' + CAST(ProductId AS VARCHAR(20)) + ',%';  
```

## <a name="see-also"></a>Voir aussi

- [LEFT &#40;Transact-SQL&#41;](../../t-sql/functions/left-transact-sql.md)
- [LTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/ltrim-transact-sql.md)
- [RIGHT &#40;Transact-SQL&#41;](../../t-sql/functions/right-transact-sql.md)
- [RTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/rtrim-transact-sql.md)
- [SUBSTRING &#40;Transact-SQL&#41;](../../t-sql/functions/substring-transact-sql.md)
- [TRIM &#40;Transact-SQL&#41;](../../t-sql/functions/trim-transact-sql.md)
- [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)
