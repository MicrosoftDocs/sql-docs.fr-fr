---
description: OPENJSON (Transact-SQL)
title: OPENJSON (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- OPENJSON
- OPENJSON_TSQL
helpviewer_keywords:
- OPENJSON rowset function
- JSON, importing
- JSON, converting from
ms.assetid: 233d0877-046b-4dcc-b5da-adeb22f78531
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 1c7b948b1f883d1de6c6bb6556aee25c0e859695
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98085821"
---
# <a name="openjson-transact-sql"></a>OPENJSON (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

**OPENJSON** est une fonction table qui analyse du texte JSON et retourne des objets et propriétés à partir de l’entrée JSON sous forme de lignes et de colonnes. Autrement dit, **OPENJSON** offre une vue de l’ensemble de lignes d’un document JSON. Vous pouvez spécifier explicitement les colonnes dans l’ensemble de lignes et les chemins de propriétés JSON utilisés pour remplir les colonnes. Étant donné que **OPENJSON** retourne un ensemble de lignes, vous pouvez utiliser **OPENJSON** dans la clause `FROM` d’une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)], tout comme vous pouvez utiliser n’importe quel autre table, vue ou fonction table.  
  
Utilisez **OPENJSON** pour importer des données JSON dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou pour les convertir au format relationnel pour une application ou un service qui ne peut pas les consommer directement.  
  
> [!NOTE]  
>  La fonction **OPENJSON** est disponible uniquement sous le niveau de compatibilité 130 ou plus. Si le niveau de compatibilité de votre base de données est inférieur à 130, SQL Server ne peut pas trouver ni exécuter la fonction **OPENJSON**. Les autres fonctions JSON sont disponibles à tous les niveaux de compatibilité.
> 
> Vous pouvez vérifier le niveau de compatibilité dans la vue `sys.databases` ou dans les propriétés de base de données. Vous pouvez changer le niveau de compatibilité d’une base de données à l’aide de la commande suivante :  
> 
> `ALTER DATABASE DatabaseName SET COMPATIBILITY_LEVEL = 130`
>
> Le niveau de compatibilité 120 peut être la valeur par défaut, même dans une nouvelle base de données Azure SQL Database.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique")[Conventions de syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
OPENJSON( jsonExpression [ , path ] )  [ <with_clause> ]

<with_clause> ::= WITH ( { colName type [ column_path ] [ AS JSON ] } [ ,...n ] )
```  

La fonction table **OPENJSON** analyse l’expression *jsonExpression* fournie comme premier argument et retourne une ou plusieurs lignes contenant des données des objets JSON dans l’expression. *jsonExpression* peut contenir des sous-objets imbriqués. Si vous voulez analyser un sous-objet depuis *jsonExpression*, vous pouvez spécifier un paramètre **path** pour ce sous-objet JSON.

### <a name="openjson"></a>openjson

![Syntaxe pour OPENJSON TVF](../../relational-databases/json/media/openjson-syntax.png "Syntaxe OPENJSON")  

Par défaut, la fonction table **OPENJSON** retourne trois colonnes qui contiennent le nom de la clé, la valeur et le type de chaque paire {clé:valeur} trouvée dans *jsonExpression*. En guise d’alternative, vous pouvez spécifier explicitement le schéma du jeu de résultats retourné par **OPENJSON** en fournissant *with_clause*.
  
### <a name="with_clause"></a>with_clause
  
![Syntaxe pour la clause WITH dans OPENJSON TVF](../../relational-databases/json/media/openjson-shema-syntax.png "Syntaxe OPENJSON WITH")

*with_clause* contient la liste des colonnes avec leurs types que **OPENJSON** doit retourner. Par défaut, **OPENJSON** fait correspondre les clés dans *jsonExpression* avec les noms de colonne dans *with_clause* (dans ce cas, les clés correspondantes impliquent un respect de la casse). Si un nom de colonne ne correspond pas à un nom de clé, vous pouvez fournir un *column_path* facultatif, qui est une [expression de chemin JSON](../../relational-databases/json/json-path-expressions-sql-server.md) qui fait référence à une clé dans *jsonExpression*. 

## <a name="arguments"></a>Arguments

### <a name="jsonexpression"></a>*jsonExpression*

Expression de caractères Unicode contenant du texte JSON.  
  
OPENJSON effectue une itération sur les éléments du tableau ou les propriétés de l’objet dans l’expression JSON et renvoie une seule ligne pour chaque élément ou propriété. L’exemple suivant retourne chaque propriété de l’objet fourni en tant que *jsonExpression* :  

```sql
DECLARE @json NVARCHAR(2048) = N'{
   "String_value": "John",
   "DoublePrecisionFloatingPoint_value": 45,
   "DoublePrecisionFloatingPoint_value": 2.3456,
   "BooleanTrue_value": true,
   "BooleanFalse_value": false,
   "Null_value": null,
   "Array_value": ["a","r","r","a","y"],
   "Object_value": {"obj":"ect"}
}';

SELECT * FROM OpenJson(@json);
```

**Résultats :**

| key                                | value                 | type |
| :--                                | :----                 | :--- |
| String_value                       | John                  | 1 |
| DoublePrecisionFloatingPoint_value | 45                    | 2 |
| DoublePrecisionFloatingPoint_value | 2.3456                | 2 |
| BooleanTrue_value                  | true                  | 3 |
| BooleanFalse_value                 | false                 | 3 |
| Null_value                         | NULL                  | 0 |
| Array_value                        | ["a","r","r","a","y"] | 4 |
| Object_value                       | {"obj":"ect"}         | 5 |
| &nbsp; | &nbsp; | &nbsp; |

- La DoublePrecisionFloatingPoint_value est conforme à la norme IEEE-754.

### <a name="path"></a>*path*

Expression de chemin JSON facultative qui fait référence à un objet ou un tableau dans *jsonExpression*. **OPENJSON** recherche dans le texte JSON à la position spécifiée et analyse uniquement le fragment référencé. Pour plus d’informations, consultez [Expressions de chemin JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).

Dans [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)], vous pouvez fournir une variable comme valeur de *path*.
  
L’exemple suivant retourne un objet imbriqué en spécifiant le *chemin* :  

```sql  
DECLARE @json NVARCHAR(4000) = N'{  
      "path": {  
            "to":{  
                 "sub-object":["en-GB", "en-UK","de-AT","es-AR","sr-Cyrl"]  
                 }  
              }  
 }';

SELECT [key], value
FROM OPENJSON(@json,'$.path.to."sub-object"')
```  
  
 **Résultats**  
  
|Clé|Valeur|  
|---------|-----------|  
|0|en-GB|  
|1|en-UK|  
|2|de-AT|  
|3|es-AR|  
|4|sr-Cyrl|  

Quand **OPENJSON** analyse un tableau JSON, la fonction retourne les index des éléments dans le texte JSON en tant que clés.

La comparaison utilisée pour faire correspondre les étapes de chemin aux propriétés de l’expression JSON respecte la casse et ne tient pas compte du classement (autrement dit, il s’agit d’une comparaison BIN2). 

### <a name="with_clause"></a>*with_clause*

Définit explicitement le schéma de sortie pour la fonction **OPENJSON** à retourner. *with_clause* peut contenir les éléments suivants :

*colName* est le nom de la colonne de sortie.  
  
Par défaut, **OPENJSON** utilise le nom de la colonne pour rechercher une propriété dans le texte JSON. Par exemple, si vous spécifiez la colonne *name* dans le schéma, OPENJSON essaie de remplir cette colonne avec la propriété « name » dans le texte JSON. Vous pouvez remplacer ce mappage par défaut à l’aide de l’argument *column_path*.  
  
*type*  
Type de données de la colonne de sortie.  

> [!NOTE]
> Si vous utilisez également l’option **AS JSON**, le *type* de la colonne doit être `NVARCHAR(MAX)`.
  
*column_path*  
Chemin JSON qui spécifie la propriété à retourner dans la colonne spécifiée. Pour plus d’informations, consultez la description du paramètre *path* plus haut dans cette rubrique.  
  
Utilisez *column_path* pour remplacer les règles de mappage par défaut lorsque le nom d’une colonne de sortie ne correspond pas à celui de la propriété.  
  
La comparaison utilisée pour faire correspondre les étapes de chemin aux propriétés de l’expression JSON respecte la casse et ne tient pas compte du classement (autrement dit, il s’agit d’une comparaison BIN2).  
  
Pour plus d’informations sur les chemins, consultez [Expressions de chemin JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
*AS JSON*  
Utilisez l’option **AS JSON** dans une définition de colonne pour spécifier que la propriété référencée contient un objet ou tableau JSON interne. Si vous spécifiez l’option **AS JSON**, le type de la colonne doit être NVARCHAR(MAX).

- Si vous ne spécifiez pas **AS JSON** pour une colonne, la fonction retourne une valeur scalaire (par exemple, int, string, true, false) à partir de la propriété JSON spécifiée dans le chemin spécifié. Si le chemin représente un objet ou un tableau et que la propriété est introuvable dans le chemin spécifié, la fonction retourne la valeur Null en mode lax ou une erreur en mode strict. Ce comportement est semblable à celui de la fonction **JSON_VALUE**.  
  
- Si vous spécifiez **AS JSON** pour une colonne, la fonction retourne un fragment JSON à partir de la propriété JSON spécifiée dans le chemin spécifié. Si le chemin représente une valeur scalaire et que la propriété est introuvable dans le chemin spécifié, la fonction retourne la valeur Null en mode lax ou une erreur en mode strict. Ce comportement est semblable à celui de la fonction **JSON_QUERY**.  
  
> [!NOTE]  
> Si vous voulez renvoyer un fragment JSON imbriqué à partir d’une propriété JSON, vous devez fournir l’indicateur **AS JSON**. Sans cette option, si la propriété est introuvable, OPENJSON retourne une valeur NULL au lieu de l’objet ou du tableau JSON référencé, ou bien une erreur d’exécution est renvoyée en mode strict.  
  
Par exemple, la requête suivante retourne et formate les éléments d’un tableau :  
  
```sql  
DECLARE @json NVARCHAR(MAX) = N'[  
  {  
    "Order": {  
      "Number":"SO43659",  
      "Date":"2011-05-31T00:00:00"  
    },  
    "AccountNumber":"AW29825",  
    "Item": {  
      "Price":2024.9940,  
      "Quantity":1  
    }  
  },  
  {  
    "Order": {  
      "Number":"SO43661",  
      "Date":"2011-06-01T00:00:00"  
    },  
    "AccountNumber":"AW73565",  
    "Item": {  
      "Price":2024.9940,  
      "Quantity":3  
    }  
  }
]'  
   
SELECT *
FROM OPENJSON ( @json )  
WITH (   
              Number   VARCHAR(200)   '$.Order.Number',  
              Date     DATETIME       '$.Order.Date',  
              Customer VARCHAR(200)   '$.AccountNumber',  
              Quantity INT            '$.Item.Quantity',  
              [Order]  NVARCHAR(MAX)  AS JSON  
 )
```  
  
**Résultats**
  
|Number|Date|Customer|Quantité|JSON|  
|------------|----------|--------------|--------------|-----------|  
|SO43659|2011-05-31T00:00:00|AW29825|1|{"Number":"SO43659","Date":"2011-05-31T00:00:00"}|  
|SO43661|2011-06-01T00:00:00|AW73565|3|{"Number":"SO43661","Date":"2011-06-01T00:00:00"}|  
  
## <a name="return-value"></a>Valeur retournée
Les colonnes que la fonction OPENJSON retourne dépendent de l’option WITH.  
  
1. Lorsque vous appelez OPENJSON avec le schéma par défaut (autrement dit, lorsque vous ne spécifiez pas un schéma explicite dans la clause WITH), la fonction retourne une table comportant les colonnes suivantes :  
    1.  **Key**. Valeur nvarchar(4000) qui contient le nom de la propriété spécifiée ou de l’index de l’élément dans le tableau spécifié. Le classement de la colonne Key est BIN2.  
    2.  **Valeur**. Valeur nvarchar(max) qui contient la valeur de la propriété. La colonne de valeur hérite son classement de *jsonExpression*.
    3.  **Type**. Valeur entière qui contient le type de la valeur. La colonne **Type** est retournée uniquement lorsque vous utilisez OPENJSON avec le schéma par défaut. La colonne Type a l’une des valeurs suivantes :  
  
        |Valeur de la colonne Type|Type de données JSON|  
        |------------------------------|--------------------|  
        |0|null|  
        |1|string|  
        |2|nombre|  
        |3|true/false|  
        |4|tableau|  
        |5|object|  
  
     Seules les propriétés de premier niveau sont retournées. L’instruction échoue si le texte JSON n’est pas formaté correctement.  

2. Lorsque vous appelez OPENJSON et que vous spécifiez un schéma explicite dans la clause WITH, la fonction retourne une table avec le schéma que vous avez défini dans la clause WITH.

> [!NOTE]  
> Les colonnes **Clé**, **Valeur** et **Type** ne sont retournées que lorsque vous utilisez OPENJSON avec le schéma par défaut et ne sont pas disponibles avec un schéma explicite.

## <a name="remarks"></a>Notes  

Le chemin *json_path* utilisé dans le deuxième argument **OPENJSON** ou dans *with_clause* peut commencer par le mot clé **lax** ou **strict**.

- En mode **lax**, **OPENJSON** ne déclenche pas d’erreur si l’objet ou la valeur dans le chemin spécifié est introuvable. Si le chemin est introuvable, **OPENJSON** retourne un jeu de résultats vide ou une valeur NULL.
- En mode **strict**, **OPENJSON** renvoie une erreur si le chemin est introuvable.

Certains exemples de cette page spécifient explicitement le mode path, lax ou strict. Le mode path est facultatif. Si vous ne spécifiez pas explicitement un mode path, le mode lax est la valeur par défaut. Pour plus d’informations sur le mode path et les expressions de chemin, consultez [Expressions de chemin JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).    

Les noms de colonne dans *with_clause* sont mis en correspondance avec les clés dans le texte JSON. Si vous spécifiez le nom de colonne `[Address.Country]`, il est mis en correspondance avec la clé `Address.Country`. Si vous voulez faire référence à une clé imbriquée `Country` au sein de l’objet `Address`, vous devez spécifier le chemin `$.Address.Country` dans le chemin de la colonne.

*json_path* peut contenir des clés avec des caractères alphanumériques. Échappez le nom de la clé dans *json_path* avec des guillemets doubles si vous avez des caractères spéciaux dans les clés. Par exemple, `$."my key $1".regularKey."key with . dot"` correspond à la valeur 1 dans le texte JSON suivant :

```json
{
  "my key $1": {
    "regularKey":{
      "key with . dot": 1
    }
  }
}
```  

## <a name="examples"></a>Exemples  
  
### <a name="example-1---convert-a-json-array-to-a-temporary-table"></a>Exemple 1 : convertir un tableau JSON en table temporaire

L’exemple suivant fournit la liste des identificateurs sous la forme d’un tableau JSON de nombres. La requête convertit le tableau JSON en table des identificateurs et filtre tous les produits avec les ID spécifiés.  
  
```sql  
DECLARE @pSearchOptions NVARCHAR(4000) = N'[1,2,3,4]'

SELECT *
FROM products
INNER JOIN OPENJSON(@pSearchOptions) AS productTypes
 ON product.productTypeID = productTypes.value
```  
  
Cette requête équivaut à l’exemple suivant. Toutefois, dans l’exemple ci-dessous, vous devez incorporer des nombres dans la requête au lieu de les passer en tant que paramètres.  
  
```sql  
SELECT *
FROM products
WHERE product.productTypeID IN (1,2,3,4)
```  
  
### <a name="example-2---merge-properties-from-two-json-objects"></a>Exemple 2 : fusionner des propriétés de deux objets JSON

L’exemple suivant sélectionne une union de toutes les propriétés de deux objets JSON. Les deux objets ont une propriété *name* en double. L’exemple utilise la valeur de clé pour exclure la ligne en double des résultats.  
  
```sql  
DECLARE @json1 NVARCHAR(MAX),@json2 NVARCHAR(MAX)

SET @json1=N'{"name": "John", "surname":"Doe"}'

SET @json2=N'{"name": "John", "age":45}'

SELECT *
FROM OPENJSON(@json1)
UNION ALL
SELECT *
FROM OPENJSON(@json2)
WHERE [key] NOT IN (SELECT [key] FROM OPENJSON(@json1))
```  
  
### <a name="example-3---join-rows-with-json-data-stored-in-table-cells-using-cross-apply"></a>Exemple 3 : joindre des lignes avec des données JSON stockées dans les cellules d’une table à l’aide de CROSS APPLY

Dans l’exemple suivant, la table `SalesOrderHeader` a une colonne de texte `SalesReason` qui contient un tableau de `SalesOrderReasons` au format JSON. Les objets `SalesOrderReasons` contiennent des propriétés telles que *Quality* et *Manufacturer*. L’exemple crée un rapport qui joint chaque ligne de commande client au motif de vente associé. L’opérateur OPENJSON développe le tableau JSON des motifs de vente comme si les motifs étaient stockés dans une table enfant distincte. Ensuite, l’opérateur CROSS APPLY joint chaque ligne de commande client aux lignes retournées par la fonction table OPENJSON.  
  
```sql  
SELECT SalesOrderID,OrderDate,value AS Reason
FROM Sales.SalesOrderHeader
CROSS APPLY OPENJSON(SalesReasons)
```  
  
> [!TIP] 
> Lorsque vous devez développer des tableaux JSON stockés dans des champs individuels et les joindre à leurs lignes parents, vous utilisez généralement l’opérateur [!INCLUDE[tsql](../../includes/tsql-md.md)] CROSS APPLY. Pour plus d’informations sur CROSS APPLY, consultez [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md).  
  
La même requête peut être réécrite à l’aide de `OPENJSON` avec un schéma explicitement défini de lignes à retourner :  
  
```sql  
SELECT SalesOrderID, OrderDate, value AS Reason  
FROM Sales.SalesOrderHeader  
     CROSS APPLY OPENJSON (SalesReasons) WITH (value NVARCHAR(100) '$')
```  
  
Dans cet exemple, le chemin `$` fait référence à chaque élément du tableau. Pour caster explicitement la valeur retournée, vous pouvez utiliser ce type de requête.  
  
### <a name="example-4---combine-relational-rows-and-json-elements-with-cross-apply"></a>Exemple 4 : combiner des lignes relationnelles et des éléments JSON avec CROSS APPLY

La requête suivante combine des lignes relationnelles et des éléments JSON dans les résultats présentés dans le tableau suivant.  
  
```sql  
SELECT store.title, location.street, location.lat, location.long  
FROM store  
CROSS APPLY OPENJSON(store.jsonCol, 'lax $.location')   
     WITH (street VARCHAR(500) ,  postcode VARCHAR(500) '$.postcode' ,  
     lon int '$.geo.longitude', lat int '$.geo.latitude')  
     AS location
```  
  
**Résultats**
  
|title|street|code postal|lon|lat|  
|-----------|------------|--------------|---------|---------|  
|Whole Food Markets|17991 Redmond Way|WA  98052|47.666124|-122.10155|  
|Sears|148th Ave NE|WA  98052|47.63024|-122.141246,17|  
  
### <a name="example-5---import-json-data-into-sql-server"></a>Exemple 5 : importer des données JSON dans SQL Server

L’exemple suivant charge l’intégralité d’un objet JSON dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
```sql  
DECLARE @json NVARCHAR(max)  = N'{  
  "id" : 2,  
  "firstName": "John",  
  "lastName": "Smith",  
  "isAlive": true,  
  "age": 25,  
  "dateOfBirth": "2015-03-25T12:00:00",  
  "spouse": null  
  }';  
   
  INSERT INTO Person  
  SELECT *   
  FROM OPENJSON(@json)  
  WITH (id INT,  
        firstName NVARCHAR(50), lastName NVARCHAR(50),   
        isAlive BIT, age INT,  
        dateOfBirth DATETIME, spouse NVARCHAR(50))
```  

### <a name="example-6---simple-example-with-json-content"></a>Exemple 6 : exemple simple avec du contenu JSON

```sql
--simple cross apply example
DECLARE @JSON NVARCHAR(MAX) = N'[
{
"OrderNumber":"SO43659",
"OrderDate":"2011-05-31T00:00:00",
"AccountNumber":"AW29825",
"ItemPrice":2024.9940,
"ItemQuantity":1
},
{
"OrderNumber":"SO43661",
"OrderDate":"2011-06-01T00:00:00",
"AccountNumber":"AW73565",
"ItemPrice":2024.9940,
"ItemQuantity":3
}
]'

SELECT root.[key] AS [Order],TheValues.[key], TheValues.[value]
FROM OPENJSON ( @JSON ) AS root
CROSS APPLY OPENJSON ( root.value) AS TheValues
```

## <a name="see-also"></a>Voir aussi

- [Expressions de chemin JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md)   
- [Convert JSON Data to Rows and Columns with OPENJSON &#40;SQL Server&#41; [Convertir des données JSON en lignes et colonnes avec OPENJSON &#40;SQL Server&#41;]](../../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md)   
- [Utiliser OPENJSON avec le schéma par défaut &#40;SQL Server&#41;](../../relational-databases/json/use-openjson-with-the-default-schema-sql-server.md)   
- [Utiliser OPENJSON avec un schéma explicite &#40;SQL Server&#41;](../../relational-databases/json/use-openjson-with-an-explicit-schema-sql-server.md)  
  
