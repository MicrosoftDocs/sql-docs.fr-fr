---
title: Correspondance du type de séquence | Microsoft Docs
description: Découvrez comment faire correspondre le type de séquence retourné par une expression XQuery avec un type spécifique.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sequence type matching [XQuery]
- XQuery, sequence type matching
ms.assetid: 8c56fb69-ca04-4aba-b55a-64ae216c492d
author: rothja
ms.author: jroth
ms.openlocfilehash: 11c0eb693468b980db88995713dc6af99180c373
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352826"
---
# <a name="type-system---sequence-type-matching"></a>Système de types : mise en correspondance du type de séquence
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Une valeur d'expression XQuery est toujours une séquence de zéro élément ou plus. Un élément peut être soit une valeur atomique, soit un nœud. Le type de séquence fait référence à la possibilité de mettre en correspondance le type de séquence retourné par une expression de requête avec un type spécifique. Par exemple :  
  
-   Si la valeur d'expression est atomique, vous voudrez peut-être savoir s'il s'agit d'un type entier, décimal ou chaîne.  
  
-   Si la valeur d'expression est un nœud XML, vous voudrez peut-être savoir s'il s'agit d'un nœud de commentaire, d'un nœud d'instruction de traitement ou d'un nœud de texte.  
  
-   Vous voudrez peut-être savoir si l'expression retourne un élément XML ou un nœud d'attribut d'un nom et d'un type spécifiques.  
  
 Vous pouvez utiliser l'opérateur booléen `instance of` dans la mise en correspondance du type de séquence. Pour plus d’informations sur l' `instance of` expression, consultez [Expressions SequenceType &#40;XQuery&#41;](../xquery/sequencetype-expressions-xquery.md).  
  
## <a name="comparing-the-atomic-value-type-returned-by-an-expression"></a>Comparaison du type de valeur atomique retourné par une expression  
 Si une expression retourne une séquence de valeurs atomiques, vous pouvez être amené à rechercher le type de la valeur dans la séquence. Les exemples ci-dessous illustrent comment utiliser la syntaxe de type de séquence pour évaluer le type de valeur atomique retourné par une expression.  
  
### <a name="example-determining-whether-a-sequence-is-empty"></a>Exemple : déterminer si une séquence est vide  
 Le type de séquence **vide ()** peut être utilisé dans une expression de type de séquence pour déterminer si la séquence retournée par l’expression spécifiée est une séquence vide.  
  
 Dans l’exemple suivant, le schéma XML permet à l' `root` élément <> d’être nul :  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" nillable="true" type="byte"/>  
</schema>'  
GO  
```  
  
 Désormais, si une instance XML typée spécifie une valeur pour l' `root` élément <>, `instance of empty()` retourne false.  
  
```  
DECLARE @var XML(SC1)  
SET @var = '<root>1</root>'  
-- The following returns False  
SELECT @var.query('data(/root[1]) instance of  empty() ')  
GO  
```  
  
 Si le <`root`> élément est nul dans l’instance, sa valeur est une séquence vide et retourne la valeur `instance of empty()` true.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xsi:nil="true" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" />'  
SELECT @var.query('data(/root[1]) instance of  empty() ')  
GO  
```  
  
### <a name="example-determining-the-type-of-an-attribute-value"></a>Exemple : déterminer le type d'une valeur d'attribut  
 Parfois, vous voudrez peut-être évaluer le type de séquence retourné par une expression avant le traitement. Par exemple, vous pouvez disposer d'un schéma XML dans lequel un nœud est défini comme type d'union. Dans l'exemple ci-dessous, le schéma XML dans la collection définit l'attribut `a` comme un type d'union dont la valeur peut être de type décimal ou chaîne.  
  
```  
-- Drop schema collection if it exists.  
-- DROP XML SCHEMA COLLECTION SC.  
-- GO  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
  <element name="root">  
    <complexType>  
       <sequence/>  
         <attribute name="a">  
            <simpleType>  
               <union memberTypes="decimal string"/>  
            </simpleType>  
         </attribute>  
     </complexType>  
  </element>  
</schema>'  
GO  
```  
  
 Avant de traiter une instance XML typée, vous voudrez peut-être connaître le type de la valeur de l'attribut `a`. Dans l'exemple ci-dessous, la valeur de l'attribut `a` est un type décimal. Par conséquent, `, instance of xs:decimal` retourne True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root a="2.5"/>'  
SELECT @var.query('data((/root/@a)[1]) instance of xs:decimal')  
GO  
```  
  
 À présent, changez la valeur de l'attribut `a` en type chaîne. `instance of xs:string` retournera True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root a="Hello"/>'  
SELECT @var.query('data((/root/@a)[1]) instance of xs:string')  
GO  
```  
  
### <a name="example-cardinality-in-sequence-expressions"></a>Exemple : cardinalité dans les expressions de séquence  
 Cet exemple illustre l'effet de la cardinalité dans une expression de séquence. Le schéma XML suivant définit une <`root` élément> qui est de type Byte et qui est nillable.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" nillable="true" type="byte"/>  
</schema>'  
GO  
```  
  
 Dans la requête ci-dessous, comme l'expression retourne un singleton de type octet, `instance of` retourne True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root>111</root>'  
SELECT @var.query('data(/root[1]) instance of  xs:byte ')   
GO  
```  
  
 Si vous rendez la <`root`> élément Nil, sa valeur est une séquence vide. Cela signifie que l'expression `/root[1]` retourne une séquence vide. Par conséquent, `instance of xs:byte` retourne False. Notez que dans ce cas la cardinalité par défaut est 1.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xsi:nil="true" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"></root>'  
SELECT @var.query('data(/root[1]) instance of  xs:byte ')   
GO  
-- result = false  
```  
  
 Si vous spécifiez la cardinalité en ajoutant l'indicateur d'occurrence (`?`), l'expression de séquence retourne True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xsi:nil="true" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"></root>'  
SELECT @var.query('data(/root[1]) instance of  xs:byte? ')   
GO  
-- result = true  
```  
  
 Notez que le test dans une expression de type de séquence s'effectue en deux phases :  
  
1.  En premier lieu, le test détermine si le type d'expression correspond au type spécifié.  
  
2.  Si tel est le cas, le test détermine alors si le nombre d'éléments retournés par l'expression correspond à l'indicateur d'occurrence spécifié.  
  
 Si les deux conditions sont remplies, l'expression `instance of` retourne True.  
  
### <a name="example-querying-against-an-xml-type-column"></a>Exemple : interrogation d'une colonne de type xml  
 Dans l’exemple suivant, une requête est spécifiée par rapport à une colonne Instructions de type **XML** dans la [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] base de données. Il s'agit d'une colonne XML typée car un schéma lui est associé. Le schéma XML définit l'attribut `LocationID` du type entier. Par conséquent, dans l’expression de séquence, `instance of xs:integer?` retourne la valeur true.  
  
```  
SELECT Instructions.query('   
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
data(/AWMI:root[1]/AWMI:Location[1]/@LocationID) instance of xs:integer?') as Result   
FROM Production.ProductModel   
WHERE ProductModelID = 7  
```  
  
## <a name="comparing-the-node-type-returned-by-an-expression"></a>Comparaison du type de nœud retourné par une expression  
 Si une expression retourne une séquence de nœuds, vous pouvez être amené à rechercher le type du nœud dans la séquence. Les exemples ci-dessous illustrent comment utiliser la syntaxe de type de séquence pour évaluer le type de nœud retourné par une expression. Vous pouvez utiliser les types de séquence suivants :  
  
-   **Item ()** -correspond à n’importe quel élément de la séquence.  
  
-   **node ()** -détermine si la séquence est un nœud.  
  
-   **processing-instruction ()** -détermine si l’expression retourne une instruction de traitement.  
  
-   **Comment ()** -détermine si l’expression retourne un commentaire.  
  
-   **document-node ()** -détermine si l’expression retourne un nœud de document.  
  
 L'exemple ci-dessous illustre ces types de séquence.  
  
### <a name="example-using-sequence-types"></a>Exemple : utilisation des types de séquence  
 Dans cet exemple, plusieurs requêtes sont exécutées sur une variable XML non typée. Ces requêtes illustrent l'utilisation des types de séquence.  
  
```  
DECLARE @var XML  
SET @var = '<?xml-stylesheet href="someValue" type="text/xsl" ?>  
<root>text node  
  <!-- comment 1 -->   
  <a>Data a</a>  
  <!-- comment  2 -->  
</root>'  
```  
  
 Dans la première requête, l’expression retourne la valeur typée de l’élément <`a`>. Dans la deuxième requête, l’expression retourne l’élément <`a`>. Les deux sont des éléments. Par conséquent, les deux requêtes retournent True.  
  
```  
SELECT @var.query('data(/root[1]/a[1]) instance of item()')  
SELECT @var.query('/root[1]/a[1] instance of item()')  
```  
  
 Toutes les expressions XQuery dans les trois requêtes suivantes retournent l’enfant de nœud d’élément de l' `root` élément <>. Par conséquent, l'expression de type de séquence, `instance of node()`, retourne True et les deux autres expressions, `instance of text()` et `instance of document-node()`, retournent False.  
  
```  
SELECT @var.query('(/root/*)[1] instance of node()')  
SELECT @var.query('(/root/*)[1] instance of text()')  
SELECT @var.query('(/root/*)[1] instance of document-node()')   
```  
  
 Dans la requête suivante, l' `instance of document-node()` expression retourne true, car le parent de l' `root` élément <> est un nœud de document.  
  
```  
SELECT @var.query('(/root/..)[1] instance of document-node()') -- true  
```  
  
 Dans la requête ci-dessous, l'expression récupère le premier nœud de l'instance XML. Comme il s'agit d'un nœud d'instruction de traitement, l'expression `instance of processing-instruction()` retourne True.  
  
```  
SELECT @var.query('(/node())[1] instance of processing-instruction()')  
```  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limites spécifiques sont répertoriées ci-dessous :  
  
-   **le nœud de document () avec une** syntaxe de type de contenu n’est pas pris en charge.  
  
-   la syntaxe **de l’instruction de traitement (nom)** n’est pas prise en charge.  
  
## <a name="element-tests"></a>Tests d'élément  
 Un test d'élément permet de mettre en correspondance le nœud d'élément retourné par une expression avec un nœud d'élément avec un nom et un type spécifiques. Vous pouvez utiliser ces tests d'éléments :  
  
```  
element ()  
element(ElementName)  
element(ElementName, ElementType?)   
element(*, ElementType?)  
```  
  
## <a name="attribute-tests"></a>Tests d'attribut  
 Le test d'attribut détermine si l'attribut retourné par une expression est un nœud d'attribut. Vous pouvez utiliser ces tests d'attributs :  
  
 `attribute()`  
  
 `attribute(AttributeName)`  
  
 `attribute(AttributeName, AttributeType)`  
  
## <a name="test-examples"></a>Exemples de test  
 Les exemples ci-dessous illustrent des scénarios dans lesquels des tests d'éléments et d'attributs sont utiles.  
  
### <a name="example-a"></a>Exemple A  
 Le schéma XML suivant définit le `CustomerType` type complexe dans lequel <`firstName` éléments> et <`lastName`> sont facultatifs. Pour une instance XML spécifiée, vous pouvez être amené à déterminer si un prénom existe pour un client donné.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
targetNamespace="myNS" xmlns:ns="myNS">  
  <complexType name="CustomerType">  
     <sequence>  
        <element name="firstName" type="string" minOccurs="0"   
                  nillable="true" />  
        <element name="lastName" type="string" minOccurs="0"/>  
     </sequence>  
  </complexType>  
  <element name="customer" type="ns:CustomerType"/>  
</schema>  
'  
GO  
DECLARE @var XML(SC)  
SET @var = '<x:customer xmlns:x="myNS">  
<firstName>SomeFirstName</firstName>  
<lastName>SomeLastName</lastName>  
</x:customer>'  
```  
  
 La requête suivante utilise une `instance of element (firstName)` expression pour déterminer si le premier élément enfant de <`customer`> est un élément dont le nom est <`firstName`>. Si tel est le cas, elle retourne True.  
  
```  
SELECT @var.query('declare namespace x="myNS";   
     (/x:customer/*)[1] instance of element (firstName)')  
GO  
```  
  
 Si vous supprimez l' `firstName` élément <> de l’instance, la requête retourne false.  
  
 Vous pouvez également utiliser les éléments suivants :  
  
-   La syntaxe de type de séquence `element(ElementName, ElementType?)`, comme cela est illustré dans la requête ci-dessous. Elle met en correspondance un nœud d'élément nul ou non nul dont le nom est `firstName` et dont le type est `xs:string`.  
  
    ```  
    SELECT @var.query('declare namespace x="myNS";   
    (/x:customer/*)[1] instance of element (firstName, xs:string?)')  
    ```  
  
-   La syntaxe de type de séquence `element(*, type?)`, comme cela est illustré dans la requête ci-dessous. Elle met en correspondance le nœud d'élément si son type est `xs:string`, quel que soit son nom.  
  
    ```  
    SELECT @var.query('declare namespace x="myNS"; (/x:customer/*)[1] instance of element (*, xs:string?)')  
    GO  
    ```  
  
### <a name="example-b"></a>Exemple B  
 L'exemple ci-dessous illustre comment déterminer si le nœud retourné par une expression est un nœud d'élément avec un nom spécifique. Elle utilise le test d' **élément ()** .  
  
 Dans l’exemple suivant, les deux <`Customer`> éléments de l’instance XML faisant l’objet d’une requête sont de deux types différents, `CustomerType` et `SpecialCustomerType` . Supposons que vous voulez connaître le type de l' `Customer` élément <> retourné par l’expression. La collection de schémas XML suivante définit les types `CustomerType` et `SpecialCustomerType`.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
          targetNamespace="myNS"  xmlns:ns="myNS">  
  <complexType name="CustomerType">  
    <sequence>  
      <element name="firstName" type="string"/>  
      <element name="lastName" type="string"/>  
    </sequence>  
  </complexType>  
  <complexType name="SpecialCustomerType">  
     <complexContent>  
       <extension base="ns:CustomerType">  
        <sequence>  
            <element name="Age" type="int"/>  
        </sequence>  
       </extension>  
     </complexContent>  
    </complexType>  
   <element name="customer" type="ns:CustomerType"/>  
</schema>  
'  
GO  
```  
  
 Cette collection de schémas XML est utilisée pour créer une variable **XML** typée. L’instance XML assignée à cette variable a deux <`customer`> éléments de deux types différents. Le premier élément est de type `CustomerType` et le second élément de type `SpecialCustomerType`.  
  
```  
DECLARE @var XML(SC)  
SET @var = '  
<x:customer xmlns:x="myNS">  
   <firstName>FirstName1</firstName>  
   <lastName>LastName1</lastName>  
</x:customer>  
<x:customer xsi:type="x:SpecialCustomerType" xmlns:x="myNS" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
   <firstName> FirstName2</firstName>  
   <lastName> LastName2</lastName>  
   <Age>21</Age>  
</x:customer>'  
```  
  
 Dans la requête ci-dessous, l'expression `instance of element (*, x:SpecialCustomerType ?)` retourne False, car l'expression retourne le premier élément customer qui n'est pas de type `SpecialCustomerType`.  
  
```  
SELECT @var.query('declare namespace x="myNS";   
    (/x:customer)[1] instance of element (*, x:SpecialCustomerType ?)')  
```  
  
 Si vous modifiez l’expression de la requête précédente et récupérez la deuxième <`customer` élément> ( `/x:customer)[2]` ), le `instance of` retourne la valeur true.  
  
### <a name="example-c"></a>Exemple C  
 Cet exemple utilise le test d'attribut. Le schéma XML ci-dessous définit le type complexe CustomerType avec les attributs CustomerID et Age. L'attribut Age est facultatif. Pour une instance XML spécifique, vous pouvez déterminer si l’attribut Age est présent dans l' `customer` élément <>.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
       targetNamespace="myNS" xmlns:ns="myNS">  
<complexType name="CustomerType">  
  <sequence>  
     <element name="firstName" type="string" minOccurs="0"   
               nillable="true" />  
     <element name="lastName" type="string" minOccurs="0"/>  
  </sequence>  
  <attribute name="CustomerID" type="integer" use="required" />  
  <attribute name="Age" type="integer" use="optional" />  
 </complexType>  
 <element name="customer" type="ns:CustomerType"/>  
</schema>  
'  
GO  
```  
  
 La requête ci-dessous retourne True, car il existe un nœud d'attribut du nom `Age` dans l'instance XML sur laquelle porte la requête. Le test d'attribut `attribute(Age)` est utilisé dans cette expression. Comme les attributs ne sont pas triés, la requête utilise l'expression FLWOR pour récupérer tous les attributs et tester ensuite chaque attribut à l'aide de l'expression `instance of`. L’exemple crée d’abord une collection de schémas XML pour créer une variable **XML** typée.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<x:customer xmlns:x="myNS" CustomerID="1" Age="22" >  
<firstName>SomeFName</firstName>  
<lastName>SomeLName</lastName>  
</x:customer>'  
SELECT @var.query('declare namespace x="myNS";   
FOR $i in /x:customer/@*  
RETURN  
    IF ($i instance of attribute (Age)) THEN  
        "true"  
        ELSE  
        ()')     
GO  
  
```  
  
 Si vous supprimez l'attribut facultatif `Age` de l'instance, la requête précédente retournera False.  
  
 Vous pouvez spécifier le nom et le type d'attribut (`attribute(name,type)`) dans le test d'attribut.  
  
```  
SELECT @var.query('declare namespace x="myNS";   
FOR $i in /x:customer/@*  
RETURN  
    IF ($i instance of attribute (Age, xs:integer)) THEN  
        "true"  
        ELSE  
        ()')  
```  
  
 Vous pouvez également spécifier la syntaxe de `attribute(*, type)` type de séquence. Cela met en correspondance le nœud d'attribut si le type d'attribut correspond au type spécifié, quel que soit le nom.  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limites spécifiques sont répertoriées ci-dessous :  
  
-   Dans le test d’élément, le nom de type doit être suivi de l’indicateur d’occurrence (**?**).  
  
-   **Element (ElementName, TypeName)** n’est pas pris en charge.  
  
-   **Element ( \* , TypeName)** n’est pas pris en charge.  
  
-   **Schema-Element ()** n’est pas pris en charge.  
  
-   **Schema-Attribute (AttributeName)** n’est pas pris en charge.  
  
-   L’interrogation explicite de **xsi : type** ou **xsi : Nil** n’est pas prise en charge.  
  
## <a name="see-also"></a>Voir aussi  
 [Système de type &#40;XQuery&#41;](../xquery/type-system-xquery.md)  
  
  
