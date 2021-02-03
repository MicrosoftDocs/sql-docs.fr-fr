---
description: value(), méthode (Type de données xml)
title: value(), méthode (Type de données xml)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- value method
- value() method
ms.assetid: 298a7361-dc9a-4902-9b1e-49a093cd831d
author: MightyPen
ms.author: genemi
ms.openlocfilehash: dce7148340136615b79d4bf4e4ec9dd15627c88e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181724"
---
# <a name="value-method-xml-data-type"></a>value(), méthode (Type de données xml)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Exécute une requête XQuery sur le code XML et retourne une valeur de type SQL. Cette méthode retourne une valeur scalaire.  
  
 Cette méthode est généralement utilisée pour extraire une valeur d’une instance XML stockée dans une colonne, une variable ou un paramètre de type **xml**. Vous pouvez ainsi spécifier des requêtes SELECT qui combinent ou comparent des données XML avec des données de colonnes non-XML.  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
value (XQuery, SQLType)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *XQuery*  
 Expression de *requête Xml* (littéral de chaîne) qui récupère des données à l’intérieur de l’instance XML. La requête XQuery doit retourner au plus une valeur. Dans le cas contraire, une erreur est retournée.  
  
 *SQLType*  
 Type SQL préféré (littéral de chaîne) à retourner. Le type de retour de cette méthode correspond au paramètre *SQLType*. *SQLType* ne peut pas être un type de données **xml**, un type CLR (Common Language Runtime) défini par l’utilisateur, ni un type de données **image**, **text**, **ntext** ou **sql_variant**. *SQLType* peut être un type de données SQL défini par l’utilisateur.  
  
 La méthode **value()** utilise l’opérateur [!INCLUDE[tsql](../../includes/tsql-md.md)] CONVERT de manière implicite et essaie de convertir le résultat de l’expression de requête Xml, la représentation de chaîne sérialisée, du type XSD vers le type SQL correspondant spécifié par la conversion [!INCLUDE[tsql](../../includes/tsql-md.md)]. Pour plus d’informations sur les règles de conversion de type pour CONVERT, consultez [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
> [!NOTE]  
>  Pour des raisons de performances, au lieu d’utiliser la méthode **value()** dans un prédicat pour effectuer une comparaison avec une valeur relationnelle, utilisez **exist()** avec **sql:column()** . L'exemple D vous en donne l'illustration.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-the-value-method-against-an-xml-type-variable"></a>R. Utilisation de la méthode value() sur une variable de type xml  
 Dans l'exemple suivant, une instance XML est stockée dans une variable de type `xml`. La méthode `value()` récupère la valeur d'attribut `ProductID` à partir du code XML. La valeur est ensuite assignée à une variable `int`.  
  
```sql
DECLARE @myDoc XML  
DECLARE @ProdID INT  
SET @myDoc = '<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>  
</ProductDescription>  
</Root>'  
  
SET @ProdID =  @myDoc.value('(/Root/ProductDescription/@ProductID)[1]', 'int' )  
SELECT @ProdID  
```  
  
 La valeur 1 est retournée comme résultat.  
  
 Bien qu'il n'y ait qu'un seul attribut `ProductID` dans l'instance XML, les règles de types statiques exigent que vous spécifiiez de manière explicite que l'expression de chemin d'accès retourne un singleton. Par conséquent, le `[1]` supplémentaire est spécifié à la fin de l'expression de chemin d'accès. Pour plus d’informations sur les types statiques, consultez [Requête Xml et typage statique](../../xquery/xquery-and-static-typing.md).  
  
### <a name="b-using-the-value-method-to-retrieve-a-value-from-an-xml-type-column"></a>B. Utilisation de la méthode value() pour récupérer une valeur à partir d'une colonne de type xml  
 La requête suivante porte sur une colonne de type **xml** (`CatalogDescription`) dans la base de données `AdventureWorks`. La requête récupère des valeurs d'attributs `ProductModelID` à partir de chaque instance XML stockée dans la colonne.  
  
```sql
SELECT CatalogDescription.value('             
    declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";             
       (/PD:ProductDescription/@ProductModelID)[1]', 'int') AS Result             
FROM Production.ProductModel             
WHERE CatalogDescription IS NOT NULL             
ORDER BY Result DESC             
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Le mot clé `namespace` est utilisé pour définir un préfixe d'espace de noms.  
  
-   Conformément aux exigences de type statique, `[1]` est ajouté à la fin de l'expression de chemin d'accès dans la méthode `value()` afin d'indiquer de manière explicite que l'expression de chemin d'accès retourne un singleton.  
  
 Voici le résultat partiel :  
  
```  
-----------  
35           
34           
...  
```  
  
### <a name="c-using-the-value-and-exist-methods-to-retrieve-values-from-an-xml-type-column"></a>C. Utilisation des méthodes value() et exist() pour récupérer des valeurs à partir d'une colonne de type xml  
 L’exemple suivant illustre l’utilisation de la méthode `value()` et de la [méthode exist()](../../t-sql/xml/exist-method-xml-data-type.md) du type de données **xml**. La méthode `value()` est utilisée pour récupérer des valeurs d'attributs `ProductModelID` à partir du code XML. La méthode `exist()` de la clause `WHERE` est utilisée pour filtrer les lignes de la table.  
  
 La requête récupère des ID de modèle de produit à partir d’instances XML qui incluent des informations de garantie (l’élément <`Warranty`>) comme caractéristiques. La condition de la clause `WHERE` utilise la méthode `exist()` pour récupérer uniquement les lignes qui satisfont cette condition.  
  
```sql
SELECT CatalogDescription.value('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
           (/PD:ProductDescription/@ProductModelID)[1] ', 'int') AS Result  
FROM  Production.ProductModel  
WHERE CatalogDescription.exist('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
  
     /PD:ProductDescription/PD:Features/wm:Warranty ') = 1  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   La colonne `CatalogDescription` est une colonne XML typée. Cela signifie qu'une collection de schémas lui est associée. Dans le [prologue de la requête Xml](../../xquery/modules-and-prologs-xquery-prolog.md), la déclaration de l’espace de noms permet de définir le préfixe utilisé ultérieurement dans le corps de la requête.  
  
-   Si la méthode `exist()` retourne un `1` (True), cela indique que l’instance XML contient l’élément enfant <`Warranty`> comme caractéristique.  
  
-   La méthode `value()` de la clause `SELECT` récupère ensuite les valeurs d'attributs `ProductModelID` comme entiers.  
  
 Voici le résultat partiel :  
  
```  
Result       
-----------  
19           
23           
...  
```  
  
### <a name="d-using-the-exist-method-instead-of-the-value-method"></a>D. Utilisation de la méthode exist() au lieu de la méthode value()  
 Pour des raisons de performances, au lieu d'utiliser la méthode `value()` dans un prédicat pour comparer avec une valeur relationnelle, utilisez `exist()` avec `sql:column()`. Par exemple :  
  
```sql
CREATE TABLE T (c1 INT, c2 VARCHAR(10), c3 XML)  
GO  
  
SELECT c1, c2, c3   
FROM T  
WHERE c3.value( '/root[1]/@a', 'integer') = c1  
GO  
```  
  
 Cela peut être écrit de la manière suivante :  
  
```sql
SELECT c1, c2, c3   
FROM T  
WHERE c3.exist( '/root[@a=sql:column("c1")]') = 1  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Ajouter des espaces de noms aux requêtes avec WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Créer des instances de données XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [méthodes de type de données xml](../../t-sql/xml/xml-data-type-methods.md)   
 [Langage de manipulation de données XML &#40;DML XML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
