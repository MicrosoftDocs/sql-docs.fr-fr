---
title: 'Fonction SQL : Column () (XQuery) | Microsoft Docs'
description: 'Découvrez comment la fonction XQuery SQL : Column () peut être utilisée pour lier des données relationnelles non XML à l’intérieur de XML et rassembler des données relationnelles et XML.'
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
- sql:column function
- sql:column() function
ms.assetid: e8f67bdf-b489-49a9-9d0f-2069c1750467
author: rothja
ms.author: jroth
ms.openlocfilehash: 3d2f550e3516594a269257786e109df9c84a1082
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341868"
---
# <a name="xquery-extension-functions---sqlcolumn"></a>Fonctions d’extension XQuery : sql:column()
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Comme décrit dans la rubrique [liaison de données relationnelles dans XML](../t-sql/xml/binding-relational-data-inside-xml-data.md), vous pouvez utiliser la fonction **SQL : Column ()** quand vous utilisez des [méthodes de type de données XML](../t-sql/xml/xml-data-type-methods.md) pour exposer une valeur relationnelle dans XQuery.  
  
 Par exemple, la [méthode Query () (type de données XML)](../t-sql/xml/query-method-xml-data-type.md) est utilisée pour spécifier une requête sur une instance XML stockée dans une variable ou une colonne de type **XML** . Il se peut également que vous vouliez que votre requête utilise des valeurs provenant d'une autre colonne non XML, afin de regrouper des données relationnelles et des données XML. Pour ce faire, vous utilisez la fonction **SQL : Column ()** .  
  
 La valeur SQL est alors mappée à sa valeur XQuery correspondante et son type à son type de base XQuery équivalent au type SQL.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sql:column("columnName")  
```  
  
## <a name="remarks"></a>Notes  
 Notez que la référence à une colonne spécifiée dans la fonction **SQL : Column ()** à l’intérieur d’un XQuery fait référence à une colonne de la ligne en cours de traitement.  
  
 Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , vous pouvez uniquement faire référence à une instance **XML** dans le contexte de l’expression source d’une instruction INSERT XML-DML ; sinon, vous ne pouvez pas faire référence à des colonnes de type **XML** ou à un type CLR défini par l’utilisateur.  
  
 La fonction **SQL : Column ()** n’est pas prise en charge dans les opérations de jointure. Vous pouvez utiliser l'opération APPLY à la place.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-sqlcolumn-to-retrieve-the-relational-value-inside-xml"></a>R. Utilisation de sql:column() pour récupérer une valeur relationnelle dans du code XML  
 Concernant la construction XML, l'exemple suivant illustre la méthode à suivre pour récupérer des valeurs dans une colonne relationnelle non XML afin de lier les données XML et les données relationnelles.  
  
 La requête construit du code XML se présentant sous la forme suivante :  
  
```xml
<Product ProductID="771" ProductName="Mountain-100 Silver, 38" ProductPrice="3399.99" ProductModelID="19"   
  ProductModelName="Mountain 100" />  
```  
  
 Il est à noter les points suivants dans le code XML ainsi construit :  
  
-   Les valeurs des attributs **ProductID**, **ProductName** et **ProductPrice** sont obtenues à partir de la table **Product** .  
  
-   La valeur de l’attribut **ProductModelID** est récupérée à partir de la table **ProductModel** .  
  
-   Pour rendre la requête plus intéressante, la valeur de l’attribut **ProductModelName** est obtenue à partir de la colonne **CatalogDescription** de **type XML**. Comme les informations du catalogue de modèles de produits XML ne sont pas stockées pour tous les modèles de produits, l'instruction `if` ne permet de récupérer la valeur que si elle existe.  
  
    ```sql
    SELECT P.ProductID, CatalogDescription.query('  
    declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
           <Product   
               ProductID=       "{ sql:column("P.ProductID") }"  
               ProductName=     "{ sql:column("P.Name") }"  
               ProductPrice=    "{ sql:column("P.ListPrice") }"  
               ProductModelID= "{ sql:column("PM.ProductModelID") }" >  
               { if (not(empty(/pd:ProductDescription))) then  
                 attribute ProductModelName { /pd:ProductDescription[1]/@ProductModelName }  
                else   
                   ()  
    }  
            </Product>  
    ') as Result  
    FROM Production.ProductModel PM, Production.Product P  
    WHERE PM.ProductModelID = P.ProductModelID  
    AND   CatalogDescription is not NULL  
    ORDER By PM.ProductModelID  
    ```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Les valeurs étant issues de deux tables différentes, la clause FROM doit mentionner ces deux tables. La condition stipulée dans la clause WHERE filtre le résultat et ne récupère ainsi que les produits dont les modèles disposent d'une description mentionnée dans le catalogue.  
  
-   Le mot clé **namespace** dans le [prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md) définit le préfixe d’espace de noms XML, « PD », qui est utilisé dans le corps de la requête. Il reste à noter que les alias de table, à savoir « P » et « PM », sont définis dans la clause FROM de la requête même.  
  
-   La fonction **SQL : Column ()** permet de placer des valeurs non-XML dans XML.  
  
 Voici le résultat partiel :  
  
```  
ProductID               Result  
-----------------------------------------------------------------  
771         <Product ProductID="771"                   ProductName="Mountain-100 Silver, 38"   
                  ProductPrice="3399.99" ProductModelID="19"   
                  ProductModelName="Mountain 100" />  
...  
```  
  
 La requête suivante construit du code XML contenant des informations spécifiques à chaque produit. Ces informations comprennent les colonnes ProductID (correspondant à l'identificateur du produit), ProductName (le nom du produit), ProductPrice (son prix) et le cas échéant, ProductModelName (le nom de son modèle) pour les produits appartenant à un modèle donné ; dans le cas présenté ci-dessous, ProductModelID = 19. Le XML est ensuite affecté à la @x variable de type **XML** .  
  
```sql
declare @x xml  
SELECT @x = CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
       <Product   
           ProductID=       "{ sql:column("P.ProductID") }"  
           ProductName=     "{ sql:column("P.Name") }"  
           ProductPrice=    "{ sql:column("P.ListPrice") }"  
           ProductModelID= "{ sql:column("PM.ProductModelID") }" >  
           { if (not(empty(/pd:ProductDescription))) then  
             attribute ProductModelName { /pd:ProductDescription[1]/@ProductModelName }  
            else   
               ()  
}  
        </Product>  
')   
FROM Production.ProductModel PM, Production.Product P  
WHERE PM.ProductModelID = P.ProductModelID  
And P.ProductModelID = 19  
select @x  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server les fonctions d’extension XQuery]()   
 [Comparer du XML typé et du XML non typé](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Données XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Créer des instances de données XML](../relational-databases/xml/create-instances-of-xml-data.md)   
 [méthodes de type de données xml](../t-sql/xml/xml-data-type-methods.md)   
 [Langage de manipulation de données XML &#40;DML XML&#41;](../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
