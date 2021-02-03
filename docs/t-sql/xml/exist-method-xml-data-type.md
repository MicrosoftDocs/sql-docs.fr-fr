---
description: Méthode exist() (type de données xml)
title: Méthode exist() (type de données xml)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- exist() method
- exist method
ms.assetid: a55b75e0-0a17-4787-a525-9b095410f7af
author: MightyPen
ms.author: genemi
ms.openlocfilehash: b29a0143b2fca6bbcafaa94acd60e096a121480d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206890"
---
# <a name="exist-method-xml-data-type"></a>Méthode exist() (type de données xml)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie un **bit** qui représente l’une des conditions suivantes :  
  
-   1 : représente True, si l'expression XQuery dans une requête renvoie un résultat non vide. En d'autres termes, elle renvoie au moins un nœud XML.  
  
-   0 : représente False, si elle renvoie un résultat vide.  
  
-   NULL si l’instance de type de données **xml** par rapport à laquelle la requête a été exécutée contient NULL.  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
exist (XQuery)   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 XQuery  
 Expression XQuery, représentant un littéral de chaîne.  
  
## <a name="remarks"></a>Notes  
  
> [!NOTE]  
>  La méthode **exist()** retourne 1 pour l’expression de requête Xml qui retourne un résultat non vide. Si vous spécifiez les fonctions **true()** ou **false()** au sein de la méthode **exist()** , la méthode **exist()** retourne 1, car les fonctions **true()** et **false()** retournent respectivement les valeurs booléennes True et False. De fait, elles retournent un résultat non vide. Par conséquent, la méthode **exist()** retourne 1 (True), comme l’illustre l’exemple suivant :  
  
```sql
DECLARE @x XML;  
SET @x='';  
SELECT @x.exist('true()');   
```  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants montrent comment spécifier la méthode **exist()** .  
  
### <a name="example-specifying-the-exist-method-against-an-xml-type-variable"></a>Exemple : Spécification de la méthode exist() par rapport à une variable de type xml  
 Dans l’exemple suivant, @x est une variable de type **xml** (xml non typé) et @f est une variable de type entier qui stocke la valeur renvoyée par la méthode **exist()** . La méthode **exist()** renvoie True (1) si la valeur de date stockée dans l’instance XML est `2002-01-01`.  
  
```sql  
DECLARE @x XML;  
DECLARE @f BIT;  
SET @x = '<root Somedate = "2002-01-01Z"/>';  
SET @f = @x.exist('/root[(@Somedate cast as xs:date?) eq xs:date("2002-01-01Z")]');  
SELECT @f;  
```  
  
 La comparaison des dates dans la méthode **exist()** fait ressortir les points suivants :  
  
-   Le code `cast as xs:date?` permet de caster la valeur en type **xs:date** à des fins de comparaison.  
  
-   La valeur de l'attribut **\@Somedate** est non typée. Lorsqu’elle est comparée, elle est implicitement castée en type indiqué à droite de la comparaison, en l’occurrence le type **xs:date**.  
  
-   Au lieu de **caster en tant que xs:date()** , vous pouvez utiliser la fonction constructeur **xs:date()** . Pour plus d’informations, consultez [Fonctions constructeurs &#40;requête Xml&#41;](../../xquery/constructor-functions-xquery.md).  
  
 L’exemple suivant est similaire au précédent, sauf qu'il possède un élément <`Somedate`>.  
  
```sql
DECLARE @x XML;  
DECLARE @f BIT;  
SET @x = '<Somedate>2002-01-01Z</Somedate>';  
SET @f = @x.exist('/Somedate[(text()[1] cast as xs:date ?) = xs:date("2002-01-01Z") ]')  
SELECT @f;  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   La méthode **text()** renvoie un nœud de texte qui contient la valeur non typée `2002-01-01`. (Le type de requête Xml est **xdt:untypedAtomic**.) Vous devez explicitement caster cette valeur typée de **x** en **xsd:date** car le cast implicite n’est pas pris en charge dans ce cas.  
  
### <a name="example-specifying-the-exist-method-against-a-typed-xml-variable"></a>Exemple : Spécification de la méthode exist() par rapport à une variable xml typée  
 L’exemple suivant illustre l’utilisation de la méthode **exist()** par rapport à une variable de type **xml**. Il s'agit d'une variable XML typée, car elle spécifie le nom de la collection d'espaces de noms de schéma, `ManuInstructionsSchemaCollection`.  
  
 Dans l’exemple, un document contenant des instructions de fabrication est d’abord affecté à cette variable, puis la méthode **exist()** est utilisée pour savoir si le document comprend un élément <`Location`> dont l’attribut **LocationID** a pour valeur 50.  
  
 La méthode **exist()** spécifiée par rapport à la variable @x renvoie 1 (True) si le document contenant des instructions de fabrication comprend un élément <`Location`> avec `LocationID=50`. Sinon, la méthode renvoie 0 (False).  
  
```sql
DECLARE @x XML (Production.ManuInstructionsSchemaCollection);  
SELECT @x=Instructions  
FROM Production.ProductModel  
WHERE ProductModelID=67;  
--SELECT @x  
DECLARE @f INT;  
SET @f = @x.exist(' declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
    /AWMI:root/AWMI:Location[@LocationID=50]  
');  
SELECT @f;  
```  
  
### <a name="example-specifying-the-exist-method-against-an-xml-type-column"></a>Exemple : Spécification de la méthode exist() par rapport à une colonne de type xml  
 La requête suivante extrait les ID de modèle de produit dont les descriptions de catalogue ne comprennent pas les spécifications, en l’occurrence l’élément <`Specifications`> :  
  
```sql
SELECT ProductModelID, CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
    <Product   
        ProductModelID= "{ sql:column("ProductModelID") }"   
        />  
') AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
    declare namespace  pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     /pd:ProductDescription[not(pd:Specifications)]'  
    ) = 1;  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   La clause WHERE sélectionne uniquement les lignes de la table **ProductDescription** qui satisfont à la condition spécifiée par rapport à la colonne de type **CatalogDescription xml**.  
  
-   La méthode **exist()** de la clause WHERE renvoie 1 (True) si le document XML ne comprend aucun élément <`Specifications`>. Remarquez l’utilisation de la [fonction not() (requête Xml)](../../xquery/functions-on-boolean-values-not-function.md).  
  
-   La [sql:column() function (requête Xml)](../../xquery/xquery-extension-functions-sql-column.md) permet d’extraire la valeur d’une colonne non-XML.  
  
-   Cette requête renvoie un ensemble de lignes vide.  
  
 La requête spécifie les méthodes **query()** et **exist()** du type de données xml et ces deux méthodes déclarent les mêmes espaces de noms dans le prologue de la requête. Dans ce cas, vous pouvez recourir à WITH XMLNAMESPACES pour déclarer le préfixe puis l'utiliser dans la requête.  
  
```sql
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
    <Product   
        ProductModelID= "{ sql:column("ProductModelID") }"   
        />  
') AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
     /pd:ProductDescription[not(pd:Specifications)]'  
    ) = 1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Ajouter des espaces de noms aux requêtes avec WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Créer des instances de données XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [méthodes de type de données xml](../../t-sql/xml/xml-data-type-methods.md)   
 [Langage de manipulation de données XML &#40;DML XML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
