---
description: Méthode query() (type de données xml)
title: Méthode query() (type de données xml)
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- query method
- query() method
ms.assetid: f48f6f7b-219f-463a-bf36-bc10f21afaeb
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 19dd6786f9ee59e4185b68c1ab49a9f5c9924a9a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200224"
---
# <a name="query-method-xml-data-type"></a>Méthode query() (type de données xml)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Spécifie une requête Xml sur une instance du type de données **XML**. Le résultat est de type **xml**. La méthode renvoie une instance XML non typé.  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
query ('XQuery')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
XQuery  
Expression de requête Xml de type chaîne qui interroge les nœuds XML, tels que des éléments ou des attributs, dans une instance XML.  
  
## <a name="examples"></a>Exemples  
Cette section propose des exemples d’utilisation de la méthode query() de type de données **xml**.  
  
### <a name="a-using-the-query-method-against-an-xml-type-variable"></a>R. Utilisation de la méthode query() sur une variable de type xml  
L'exemple suivant déclare une variable **\@myDoc** de type **xml** et lui affecte une instance XML. La méthode **query()** est ensuite utilisée pour spécifier une requête Xml portant sur le document.  
  
La requête récupère l’élément enfant <`Features`> de l’élément <`ProductDescription`> :  
  
```sql
DECLARE @myDoc XML  
SET @myDoc = '<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>  
</ProductDescription>  
</Root>'  
SELECT @myDoc.query('/Root/ProductDescription/Features')  
```  
  
La sortie suivante montre le résultat :  
  
```  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>        
```  
  
### <a name="b-using-the-query-method-against-an-xml-type-column"></a>B. Utilisation de la méthode query() sur une colonne de type XML  
Dans l’exemple suivant, la méthode **query()** est utilisée pour indiquer une requête Xml portant sur la colonne **CatalogDescription** de type **xml** tirée de la base données **AdventureWorks** :  
  
```sql
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
<Product ProductModelID="{ /PD:ProductDescription[1]/@ProductModelID }" />  
') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
     /PD:ProductDescription/PD:Features/wm:Warranty ') = 1  
```  
  
Notez les points suivants de la requête précédente :  
  
-   La colonne CatalogDescription est de type **xml**, ce qui signifie qu’elle dispose d’une collection de schémas associée. Dans le [prologue de la requête Xml](../../xquery/modules-and-prologs-xquery-prolog.md), le mot clé **namespace** définit le préfixe utilisé plus loin dans le corps de la requête.  
  
-   La méthode **query()** construit un élément <`Product`> XML qui a un attribut **ProductModelID** dans lequel la valeur de l’attribut **ProductModelID** est récupérée à partir de la base de données. Pour plus d’informations sur la construction XML, consultez [Construction XML &#40;requête Xml&#41;](../../xquery/xml-construction-xquery.md).  
  
-   La [méthode exist() (type de données XML)](../../t-sql/xml/exist-method-xml-data-type.md) indiquée dans la clause WHERE ne recherche que les lignes contenant l’élément <`Warranty`> dans le code XML. Une fois encore, le mot clé **namespace** définit deux préfixes d’espace de noms.  
  
La sortie suivante montre le résultat partiel :  
  
```  
<Product ProductModelID="19"/>   
<Product ProductModelID="23"/>   
...  
```  
  
Notez que les méthodes query() et exist() déclarent toutes deux le préfixe PD. Quand de tels cas se présentent, vous pouvez utiliser WITH XMLNAMESPACES pour définir au préalable les préfixes et les utiliser dans la requête.  
  
```sql
WITH XMLNAMESPACES 
(  
   'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD,  
   'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS WM
)  
SELECT CatalogDescription.query('<Product ProductModelID="{ /PD:ProductDescription[1]/@ProductModelID }" />')
       AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('/PD:ProductDescription/PD:Features/WM:Warranty ') = 1;
```  
  
## <a name="see-also"></a>Voir aussi  
 [Ajouter des espaces de noms aux requêtes avec WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Créer des instances de données XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [méthodes de type de données xml](../../t-sql/xml/xml-data-type-methods.md)   
 [Langage de manipulation de données XML &#40;DML XML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
