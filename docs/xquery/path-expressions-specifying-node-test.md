---
title: Spécification d’un test de nœud dans une étape d’expression de chemin | Microsoft Docs
description: Découvrez comment spécifier un test de nœud dans l’étape d’axe d’une expression de chemin d’accès XQuery.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- axis step [XQuery]
- node test [XQuery]
ms.assetid: ffe27a4c-fdf3-4c66-94f1-7e955a36cadd
author: rothja
ms.author: jroth
ms.openlocfilehash: 578d10bc078edf886eb7980c3b7a099c99ef59a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338540"
---
# <a name="path-expressions---specifying-node-test"></a>Expressions de chemin : spécification d’un test de nœud
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Une étape d'axe dans une expression de chemin d'accès inclut les composants suivants :  
  
-   [Axe](../xquery/path-expressions-specifying-axis.md)  
  
-   un test de nœud ;  
  
-   [Qualificateurs d'étape (facultatif)](../xquery/path-expressions-specifying-predicates.md)  
  
 Pour plus d’informations, consultez [expressions de chemin d’accès &#40;XQuery&#41;](../xquery/path-expressions-xquery.md).  
  
 Un test de nœud est une condition et représente le deuxième composant de l'étape d'axe dans une expression de chemin d'accès. Tous les nœuds sélectionnés par une étape doivent satisfaire cette condition. Dans le cas de l'expression du chemin d'accès `/child::ProductDescription`, le test de nœud correspond donc à `ProductDescription`. En d'autres termes, cette étape ne récupère que les enfants du nœud élément dont le nom est ProductDescription.  
  
 Une condition du test de nœud peut inclure les éléments suivants :  
  
-   Nom de nœud. Seuls les nœuds de type identique au nœud principal et portant le nom indiqué sont renvoyés.  
  
-   Type de nœud. Seuls les nœuds du type précisé sont renvoyés.  
  
> [!NOTE]  
>  Les noms de nœuds précisés dans les expressions de chemin d'accès XQuery ne sont pas soumis aux mêmes règles dépendantes du classement que les requêtes Transact-SQL et respectent toujours la casse.  
  
## <a name="node-name-as-node-test"></a>Nom de nœud en tant que test de nœud  
 Pour indiquer un nom de nœud en tant que test de nœud dans une étape d'expression du chemin d'accès, vous devez au préalable comprendre le concept de type de nœud principal. Chaque axe child (enfant), parent ou attribute (attribute) possède un type de nœud principal. Par exemple :  
  
-   Un axe attribute ne peut contenir que des attributs. L'axe attribute correspond par conséquent au type de nœud principal de l'axe.  
  
-   Pour les autres axes, si les nœuds sélectionnés par l'axe peuvent contenir des nœuds éléments, l'élément correspond alors au type de nœud principal de cet axe.  
  
 Si vous indiquez un nom de nœud en tant que test de nœud, l'étape renvoie les types de nœuds suivants :  
  
-   Nœuds du type de nœud principal de l'axe  
  
-   Nœuds portant le même nom que celui indiqué dans le test de nœud.  
  
 Par exemple, considérons l'expression de chemin d'accès suivante :  
  
```  
child::ProductDescription   
```  
  
 Cette expression à une seule étape indique un axe enfant (`child`) et précise le nom de nœud `ProductDescription` comme test de nœud. L'expression ne renvoie que les nœuds du type du nœud principal de l'axe enfant, les nœuds éléments, et dont le nom est ProductDescription.  
  
 L'expression du chemin d'accès `/child::PD:ProductDescription/child::PD:Features/descendant::*,` possède pour sa part trois étapes. Ces étapes indiquent les axes child et descendant. Dans chacune des étapes, le nom du nœud est spécifié comme test de nœud. Le caractère générique (à savoir `*`) de la troisième étape indique tous les nœuds du type de nœud principal correspondant à l'axe descendant. Le type de nœud principal de l'axe détermine en effet le type de nœuds sélectionné, ainsi que les filtres de nom de nœud que les nœuds ont sélectionnés.  
  
 Par conséquent, lorsque cette expression est exécutée sur des documents XML du catalogue de produits dans la table **ProductModel** , elle récupère tous les enfants du nœud d’élément de l' \<Features> enfant du nœud d’élément de l' \<ProductDescription> élément.  
  
 L’expression de chemin d’accès, `/child::PD:ProductDescription/attribute::ProductModelID` , est constituée de deux étapes. Ces deux étapes indiquent un nom de nœud comme test de nœud. La deuxième étape utilise également un axe attribute. Ainsi, chaque étape sélectionne des nœuds du type de nœud principal de son axe portant le nom spécifié comme test de nœud. Ainsi, l’expression retourne le nœud d’attribut **ProductModelID** du \<ProductDescription> nœud d’élément.  
  
 En spécifiant les noms des nœuds comme tests de nœud, vous pouvez également utiliser le caractère générique (*) dans le nom local d'un nœud ou dans le préfixe de son espace de noms, comme illustré dans l'exemple suivant :  
  
```  
declare @x xml  
set @x = '  
<greeting xmlns="ns1">  
   <salutation>hello</salutation>  
</greeting>  
<greeting xmlns="ns2">  
   <salutation>welcome</salutation>  
</greeting>  
<farewell xmlns="ns1" />'  
select @x.query('//*:greeting')  
select @x.query('declare namespace ns="ns1"; /ns:*')  
```  
  
## <a name="node-type-as-node-test"></a>Type de nœud comme test de nœud  
 Pour interroger des types de nœuds autres que des nœuds éléments, lancez un test de type de nœud. Le tableau suivant répertorie les quatre tests de type de nœud disponibles.  
  
|Type de nœud|Retours| Exemple|  
|---------------|-------------|-------------|  
|`comment()`|Vrai (valeur True) dans le cas d'un nœud de commentaire.|`following::comment()` sélectionne tous les nœuds de commentaire apparaissant après le nœud de contexte.|  
|`node()`|Vrai (valeur True) dans le cas d'un nœud de type quelconque.|`preceding::node()` sélectionne tous les nœuds apparaissant avant le nœud de contexte.|  
|`processing-instruction()`|Vrai (valeur True) dans le cas d'un nœud d'instructions de traitement.|`self::processing instruction()` sélectionne tous les nœuds d'instructions de traitement apparaissant dans le nœud de contexte.|  
|`text()`|Vrai (valeur True) dans le cas d'un nœud de texte.|`child::text()` sélectionne tous les nœuds de texte enfants du nœud de contexte.|  
  
 Si le type de nœud, tel que text() ou comment(), est spécifié comme test de nœud, l'étape ne renvoie que les nœuds du type spécifié indépendamment du type de nœud principal de l'axe. Par exemple, l'expression de chemin d'accès suivant ne renvoie que les enfants du nœud de commentaire du nœud de contexte :  
  
```  
child::comment()  
```  
  
 De même, `/child::ProductDescription/child::Features/child::comment()` récupère les enfants de nœuds de commentaire de l' \<Features> enfant de nœud d’élément du \<ProductDescription> nœud d’élément.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants comparent nom de nœud et type de nœud.  
  
### <a name="a-results-of-specifying-the-node-name-and-the-node-type-as-node-tests-in-a-path-expression"></a>R. Résultats de la spécification du nom de nœud et du type de nœud en tant que tests de nœuds dans une expression de chemin d'accès  
 Dans l’exemple suivant, un document XML simple est assigné à une variable de type **XML** . Le document est interrogé à l'aide de plusieurs expressions de chemin d'accès. Les résultats sont ensuite comparés.  
  
```  
declare @x xml  
set @x='  
<a>  
 <b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
 </b>  
</a>'  
select @x.query('  
/child::a/child::b/descendant::*  
')  
```  
  
 Cette expression recherche les nœuds éléments descendants du nœud élément `<b>`.  
  
 L'astérisque (`*`) figurant dans le test de nœud indique un caractère générique représentant le nom du nœud. L'axe descendant possède un type de nœud primaire défini sur le nœud élément. Ainsi, l'expression renvoie tous les nœuds éléments descendants du nœud élément `<b>`. En d'autres termes, les nœuds éléments `<c>` et `<d>` sont renvoyés, comme illustré dans le résultat ci-dessous :  
  
```  
<c>text2  
     <d>text3</d>  
</c>  
<d>text3</d>  
```  
  
 Si vous indiquez un axe descendant-or-self plutôt qu'un axe descendant, le nœud de contexte est renvoyé tout comme ses descendants :  
  
```  
/child::a/child::b/descendant-or-self::*  
```  
  
 Cette expression renvoie le nœud élément `<b>` ainsi que les nœuds éléments descendants. En renvoyant les nœuds descendants, le type de nœud primaire de l'axe descendant-or-self, correspondant au type de nœud élément, détermine les type de nœuds qui sont renvoyés.  
  
 Voici le résultat obtenu :  
  
```  
<b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
</b>  
  
<c>text2  
     <d>text3</d>  
</c>  
  
<d>text3</d>   
```  
  
 L'expression précédente utilisait un caractère générique comme nom de nœud. Vous pouvez à la place utiliser la fonction `node()` comme illustré dans l'expression suivante :  
  
```  
/child::a/child::b/descendant::node()  
```  
  
 Étant donné que `node()` est un type de nœud, vous recevrez tous les nœuds de l’axe descendant. Voici le résultat obtenu :  
  
```  
text1  
<c>text2  
     <d>text3</d>  
</c>  
text2  
<d>text3</d>  
text3  
```  
  
 Si vous indiquez un axe descendant-or-self et `node()` comme test de nœud, tous les descendants, éléments et nœuds de texte, ainsi que le nœud de contexte, à savoir l'élément `<b>`, sont alors renvoyés.  
  
```  
<b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
</b>  
text1  
<c>text2  
     <d>text3</d>  
</c>  
text2  
<d>text3</d>  
text3  
```  
  
### <a name="b-specifying-a-node-name-in-the-node-test"></a>B. Indication d'un nom de nœud dans le test de nœud  
 Cet exemple spécifie un nom de nœud comme test de nœud dans toutes les expressions de chemin d'accès. Par conséquent, toutes les expressions renvoient les nœuds du type de nœud principal de l'axe dont le nom de nœud est indiqué dans le test de nœud.  
  
 L’expression de requête suivante renvoie l' `Warranty` élément <> à partir du document XML du catalogue de produits stocké dans la `Production.ProductModel` table :  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::wm:Warranty  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   Le mot clé `namespace` mentionné dans le prologue XQuery définit un préfixe utilisé dans le corps de la requête. Pour plus d’informations sur le prologue XQuery, consultez [prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md) .  
  
-   Les trois étapes de l'expression de chemin d'accès indiquent l'axe enfant et un nom de nœud comme test de nœud.  
  
-   La partie facultative de qualificateur d'étape de l'étape d'axe n'est précisée dans aucune étape de l'expression.  
  
 La requête retourne le <`Warranty`> éléments enfants de la <`Features`> élément enfant de l’élément <> `ProductDescription` .  
  
 Voici le résultat obtenu :  
  
```  
<wm:Warranty xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
  <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
  <wm:Description>parts and labor</wm:Description>  
</wm:Warranty>     
```  
  
 Dans la requête suivante, l'expression de chemin d'accès indique un caractère générique (`*`) dans un test de nœud.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::*  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Le caractère générique est précisé pour le nom du nœud. Par conséquent, la requête retourne tous les enfants de nœud d’élément du nœud d’élément <`Features`> enfant du nœud d’élément <`ProductDescription`>.  
  
 Cette requête est similaire à la précédente sauf qu'un espace de noms combiné au caractère générique est indiqué. Tous les enfants du nœud élément se trouvant dans cet espace de noms sont ainsi renvoyés. Notez que l' `Features` élément <> peut contenir des éléments provenant de différents espaces de noms.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::wm:*  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Vous pouvez utiliser le caractère générique en tant que préfixe d'espace de noms, comme illustré dans la requête suivante :  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::*:Maintenance  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Cette requête retourne le <`Maintenance`> nœuds d’élément enfants dans tous les espaces de noms du document XML du catalogue de produits.  
  
### <a name="c-specifying-node-kind-in-the-node-test"></a>C. Indication du type de nœud dans le test de nœud  
 Cet exemple spécifie le type du nœud servant de base au test de ce nœud dans toutes les expressions de chemin d'accès. Ainsi, toutes les expressions de chemin d'accès renvoient les nœuds de type indiqué dans le test de nœud.  
  
 Dans la requête suivante, l'expression de chemin d'accès précise un type de nœud dans sa troisième étape :  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::text()  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Dans la requête qui suit, les points ci-dessous sont renseignés :  
  
-   L'expression du chemin d'accès possède trois étapes séparées par une barre oblique (`/`).  
  
-   Chacune de ces étapes indique un axe child (enfant).  
  
-   Les deux premières étapes précisent un nom de nœud comme test de nœud alors que la troisième indique un type de nœud comme test de nœud.  
  
-   L’expression retourne les enfants du nœud de texte de la <`Features`> élément enfant du `ProductDescription` nœud d’élément <>.  
  
 Un seul nœud de texte est renvoyé. Voici le résultat obtenu :  
  
```  
These are the product highlights.   
```  
  
 La requête suivante retourne les enfants du nœud de commentaire de l' `ProductDescription` élément <> :  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::comment()  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Notez les points suivants dans la requête précédente :  
  
-   La deuxième étape indique un type de nœud comme test de nœud.  
  
-   Par conséquent, l’expression retourne les enfants du nœud de commentaire des `ProductDescription` nœuds d’élément <>.  
  
 Voici le résultat obtenu :  
  
```  
<!-- add one or more of these elements... one for each specific product in this product model -->  
<!-- add any tags in <specifications> -->      
```  
  
 La requête qui suit récupère les nœuds d'instructions de traitement de premier niveau :  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::processing-instruction()  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Voici le résultat obtenu :  
  
```  
<?xml-stylesheet href="ProductDescription.xsl" type="text/xsl"?>   
```  
  
 Vous pouvez transmettre un paramètre littéral de chaîne au test de nœud `processing-instruction()`. Dans ce cas, la requête renvoie les instructions de traitement dont la valeur d'attribut de nom est le littéral de chaîne indiqué dans l'argument.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::processing-instruction("xml-stylesheet")  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
## <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limites suivantes s'appliquent :  
  
-   Les tests de nœud SequenceType étendu ne sont pas pris en charge.  
  
-   processing-instruction(nom) n'est pas pris en charge. Placez plutôt le nom entre guillemets.  
  
  
