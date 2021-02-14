---
title: 'Fonction SQL : variable () (XQuery) | Microsoft Docs'
description: 'Découvrez comment utiliser la fonction d’extension XQuery SQL : variable () pour exposer une variable qui contient une valeur relationnelle SQL dans une expression XQuery.'
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sql:variable() function
- sql:variable function
ms.assetid: 6e2e5063-c1cf-4b5a-b642-234921e3f4f7
author: rothja
ms.author: jroth
ms.openlocfilehash: d879088a98bfa7e826c53e4b90822bc65caff57a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350382"
---
# <a name="xquery-extension-functions---sqlvariable"></a>Fonctions d’extension XQuery : sql:variable()
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Expose une variable qui contient une valeur relationnelle SQL dans une expression XQuery.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sql:variable("variableName") as xdt:anyAtomicType?  
```  
  
## <a name="remarks"></a>Notes  
 Comme décrit dans la rubrique [liaison de données relationnelles dans des données XML](../t-sql/xml/binding-relational-data-inside-xml-data.md), vous pouvez utiliser cette fonction lorsque vous utilisez des [méthodes de type de données XML](../t-sql/xml/xml-data-type-methods.md) pour exposer une valeur relationnelle dans XQuery.  
  
 Par exemple, la [méthode Query ()](../t-sql/xml/query-method-xml-data-type.md) est utilisée pour spécifier une requête sur une instance XML stockée dans une variable ou une colonne de type de données **XML** . En outre, vous pouvez parfois souhaiter que la requête utilise des valeurs d'une variable ou d'un paramètre [!INCLUDE[tsql](../includes/tsql-md.md)] afin de rassembler des données relationnelles et XML. Pour ce faire, vous utilisez la fonction **SQL : variable** .  
  
 La valeur SQL est mappée à une valeur XQuery correspondante et son type est un type de base XQuery équivalent au type SQL correspondant.  
  
 Vous ne pouvez faire référence qu’à une instance **XML** dans le contexte de l’expression source d’une instruction INSERT XML-DML. Sinon, vous ne pouvez pas faire référence à des valeurs de type **XML** ou à un type défini par l’utilisateur Common Language Runtime (CLR).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-the-sqlvariable-function-to-bring-a-transact-sql-variable-value-into-xml"></a>R. Utilisation de la fonction sql:variable() pour insérer une valeur variable Transact-SQL dans XML  
 L'exemple suivant construit une instance XML composée des éléments suivants :  
  
-   une valeur (`ProductID`) issue d'une colonne non-XML. La [fonction SQL : Column ()](../xquery/xquery-extension-functions-sql-column.md) permet de lier cette valeur dans le code XML.  
  
-   une valeur (`ListPrice`) issue d'une colonne non-XML d'une autre table. Là encore, `sql:column()` permet de lier cette valeur au document XML ;  
  
-   une valeur (`DiscountPrice`) issue d'une variable [!INCLUDE[tsql](../includes/tsql-md.md)]. La méthode `sql:variable()` permet de lier cette valeur au document XML ;  
  
-   Valeur ( `ProductModelName` ) à partir d’une colonne de type **XML** , pour rendre la requête plus intéressante.  
  
 Voici la requête :  
  
```sql
DECLARE @price money  
  
SET @price=2500.00  
SELECT ProductID, Production.ProductModel.ProductModelID,CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  
       <Product   
           ProductID="{ sql:column("Production.Product.ProductID") }"  
           ProductModelID= "{ sql:column("Production.Product.ProductModelID") }"  
           ProductModelName="{/pd:ProductDescription[1]/@ProductModelName }"  
           ListPrice="{ sql:column("Production.Product.ListPrice") }"  
           DiscountPrice="{ sql:variable("@price") }"  
        />')   
FROM Production.Product   
JOIN Production.ProductModel  
ON Production.Product.ProductModelID = Production.ProductModel.ProductModelID  
WHERE ProductID=771  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   La requête XQuery dans la méthode `query()` construit le document XML.  
  
-   Le `namespace` mot clé est utilisé pour définir un préfixe d’espace de noms dans le [prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md). Cette opération est réalisée car la valeur d'attribut `ProductModelName` est extraite de la colonne de type `CatalogDescription xml`, à laquelle un schéma est associé.  
  
 Voici le résultat obtenu :  
  
```xml
<Product ProductID="771" ProductModelID="19"   
         ProductModelName="Mountain 100"   
         ListPrice="3399.99" DiscountPrice="2500" />  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server les fonctions d’extension XQuery](./xquery-extension-functions-sql-column.md)   
 [Comparer du XML typé et du XML non typé](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Données XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Créer des instances de données XML](../relational-databases/xml/create-instances-of-xml-data.md)   
 [méthodes de type de données xml](../t-sql/xml/xml-data-type-methods.md)   
 [Langage de manipulation de données XML &#40;DML XML&#41;](../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
