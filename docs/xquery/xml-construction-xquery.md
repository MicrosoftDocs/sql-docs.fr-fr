---
title: Construction XML (XQuery) | Microsoft Docs
description: Apprenez à construire des structures XML dans un XQuery à l’aide des constructeurs directs et calculés.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- white space [XQuery]
- computed constructor
- construct XML structures [XQuery]
- constructors [XQuery]
- prolog
- direct constructor [SQL Server]
- XML [SQL Server], construction
- XQuery, XML construction
ms.assetid: a6330b74-4e52-42a4-91ca-3f440b3223cf
author: rothja
ms.author: jroth
ms.openlocfilehash: 1c171dab70a909edb197f5a498d2207bcb2e555b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352322"
---
# <a name="xml-construction-xquery"></a>Construction XML (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Dans XQuery, vous pouvez utiliser les constructeurs **directs** et **calculés** pour construire des structures XML dans une requête.  
  
> [!NOTE]  
>  Il n’existe aucune différence entre les constructeurs **directs** et **calculés** .  
  
## <a name="using-direct-constructors"></a>Utilisation de constructeurs directs  
 Lorsque vous utilisez des constructeurs directs, vous spécifiez une syntaxe de type XML pour construire le document XML. Les exemples suivants illustrent la construction XML par les constructeurs directs.  
  
### <a name="constructing-elements"></a>Construction d'éléments  
 À l'aide de notations XML, vous pouvez construire des éléments. L’exemple suivant utilise l’expression de constructeur d’élément direct et crée un \<ProductModel> élément. L'élément construit possède trois éléments enfants :  
  
-   Un nœud de texte.  
  
-   Deux nœuds d’élément, \<Summary> et \<Features> .  
  
    -   L' \<Summary> élément possède un enfant de nœud de texte dont la valeur est « Some description ».  
  
    -   L' \<Features> élément a trois enfants de nœud d’élément,, \<Color> \<Weight> et \<Warranty> . Chacun de ces nœuds possède un nœud de texte enfant et les valeurs « Red », « 25 » et « 2 years parts and labor », respectivement.  
  
```sql
declare @x xml;  
set @x='';  
select @x.query('<ProductModel ProductModelID="111">;  
This is product model catalog description.  
<Summary>Some description</Summary>  
<Features>  
  <Color>Red</Color>  
  <Weight>25</Weight>  
  <Warranty>2 years parts and labor</Warranty>  
</Features></ProductModel>')  
  
```  
  
 Voici le document XML obtenu :  
  
```xml
<ProductModel ProductModelID="111">  
  This is product model catalog description.  
  <Summary>Some description</Summary>  
  <Features>  
    <Color>Red</Color>  
    <Weight>25</Weight>  
    <Warranty>2 years parts and labor</Warranty>  
  </Features>  
</ProductModel>  
```  
  
 Bien que la construction d'éléments à partir d'expressions constantes, comme dans cet exemple, soit utile, la véritable puissance de cette fonctionnalité du langage XQuery réside dans la possibilité de construire un document XML qui extrait dynamiquement des données d'une base de données. Vous pouvez utiliser des accolades pour spécifier les expressions de requête. Dans le document XML obtenu, l'expression est remplacée par sa valeur. Par exemple, la requête suivante construit un élément <`NewRoot`> avec un élément enfant (<`e`>). La valeur de l’élément <`e`> est calculée en spécifiant une expression de chemin d’accès à l’intérieur des accolades ("{...}").  
  
```sql
DECLARE @x xml;  
SET @x='<root>5</root>';  
SELECT @x.query('<NewRoot><e> { /root } </e></NewRoot>');  
```  
  
 Les accolades font office de jetons de commutation de contexte et basculent la requête de la construction du document XML vers sa propre évaluation. Dans ce cas, l'expression de chemin d'accès XQuery entre les accolades, `/root`, est évaluée et les résultats lui sont substitués.  
  
 Voici le résultat obtenu :  
  
```xml
<NewRoot>  
  <e>  
    <root>5</root>  
  </e>  
</NewRoot>  
```  
  
 La requête suivante est similaire à la précédente. Toutefois, l’expression entre accolades spécifie la fonction **Data ()** pour récupérer la valeur atomique de l' `root` élément <> et l’assigne à l’élément construit, <`e`>.  
  
```sql
DECLARE @x xml;  
SET @x='<root>5</root>';  
DECLARE @y xml;  
SET @y = (SELECT @x.query('  
                           <NewRoot>  
                             <e> { data(/root) } </e>  
                           </NewRoot>' ));  
SELECT @y;  
```  
  
 Voici le résultat obtenu :  
  
```xml
<NewRoot>  
  <e>5</e>  
</NewRoot>  
```  
  
 Si vous souhaitez utiliser les accolades dans le texte au lieu de jetons de commutation de contexte, vous pouvez recourir au caractère d'échappement « }} » ou « {{ », comme le montre l'exemple suivant :  
  
```sql
DECLARE @x xml;  
SET @x='<root>5</root>';  
DECLARE @y xml;  
SET @y = (SELECT @x.query('  
<NewRoot> Hello, I can use {{ and  }} as part of my text</NewRoot>'));  
SELECT @y;  
```  
  
 Voici le résultat obtenu :  
  
```xml
<NewRoot> Hello, I can use { and  } as part of my text</NewRoot>  
```  
  
 La requête suivante est un autre exemple de construction d'éléments à l'aide du constructeur d'élément direct. En outre, la valeur de l' `FirstLocation` élément <> est obtenue en exécutant l’expression entre accolades. L'expression de requête renvoie les étapes de fabrication suivies sur le premier site de production à partir de la colonne Instructions de la table Production.ProductModel.  
  
```sql
SELECT Instructions.query('  
    declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        <FirstLocation>  
           { /AWMI:root/AWMI:Location[1]/AWMI:step }  
        </FirstLocation>   
') as Result   
FROM Production.ProductModel  
WHERE ProductModelID=7;  
```  
  
 Voici le résultat obtenu :  
  
```xml
<FirstLocation>  
  <AWMI:step xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">  
      Insert <AWMI:material>aluminum sheet MS-2341</AWMI:material> into the <AWMI:tool>T-85A framing tool</AWMI:tool>.   
  </AWMI:step>  
  <AWMI:step xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">  
      Attach <AWMI:tool>Trim Jig TJ-26</AWMI:tool> to the upper and lower right corners of the aluminum sheet.   
  </AWMI:step>  
   ...  
</FirstLocation>  
```  
  
#### <a name="element-content-in-xml-construction"></a>Contenu des éléments dans la construction XML  
 L'exemple suivant illustre le comportement des expressions dans la construction du contenu des éléments à l'aide du constructeur d'élément direct. Dans l'exemple suivant, le constructeur d'élément direct spécifie une expression. Pour cette expression, un nœud de texte est créé dans le document XML obtenu.  
  
```sql
declare @x xml;  
set @x='  
<root>  
  <step>This is step 1</step>  
  <step>This is step 2</step>  
  <step>This is step 3</step>  
</root>';  
select @x.query('  
<result>  
 { for $i in /root[1]/step  
    return string($i)  
 }  
</result>');  
  
```  
  
 La séquence de valeurs atomiques obtenue à partir de l'évaluation de l'expression est ajoutée au nœud de texte avec un espace entre les valeurs atomiques adjacentes, comme le montre le résultat. L'élément construit possède un enfant. Il s'agit d'un nœud de texte qui contient la valeur affichée dans le résultat.  
  
```xml
<result>This is step 1 This is step 2 This is step 3</result>  
```  
  
 Au lieu d'une expression, si vous spécifiez trois expressions distinctes générant trois nœuds de texte, les nœuds de texte adjacents sont concaténés en un seul nœud de texte dans le document XML obtenu.  
  
```sql
declare @x xml;  
set @x='  
<root>  
  <step>This is step 1</step>  
  <step>This is step 2</step>  
  <step>This is step 3</step>  
</root>';  
select @x.query('  
<result>  
 { string(/root[1]/step[1]) }  
 { string(/root[1]/step[2]) }  
 { string(/root[1]/step[3]) }  
</result>');  
```  
  
 Le nœud d'élément construit possède un enfant. Il s'agit d'un nœud de texte qui contient la valeur affichée dans le résultat.  
  
```xml
<result>This is step 1This is step 2This is step 3</result>  
```  
  
### <a name="constructing-attributes"></a>Construction d'attributs  
 Lorsque vous construisez des éléments à l'aide du constructeur d'élément direct, vous pouvez également spécifier les attributs de l'élément à l'aide d'une syntaxe de type XML, comme le montre l'exemple suivant :  
  
```sql
declare @x xml;  
set @x='';  
select @x.query('<ProductModel ProductModelID="111">;  
This is product model catalog description.  
<Summary>Some description</Summary>  
</ProductModel>')  
```  
  
 Voici le document XML obtenu :  
  
```xml
<ProductModel ProductModelID="111">  
  This is product model catalog description.  
  <Summary>Some description</Summary>  
</ProductModel>  
```  
  
 L’élément construit <`ProductModel`> a un attribut ProductModelID et les nœuds enfants suivants :  
  
-   Un nœud de texte (`This is product model catalog description.`).  
  
-   Nœud d’élément, <`Summary`>. Ce nœud possède un nœud de texte enfant (`Some description`).  
  
 Lorsque vous construisez un attribut, vous pouvez spécifier sa valeur avec une expression entre accolades. Dans ce cas, le résultat de l'expression est renvoyé en tant que valeur de l'attribut.  
  
 Dans l’exemple suivant, la fonction **Data ()** n’est pas strictement obligatoire. Étant donné que vous affectez la valeur d’expression à un attribut, **Data ()** est appliqué implicitement pour récupérer la valeur typée de l’expression spécifiée.  
  
```sql
DECLARE @x xml;  
SET @x='<root>5</root>';  
DECLARE @y xml;  
SET @y = (SELECT @x.query('<NewRoot attr="{ data(/root) }" ></NewRoot>'));  
SELECT @y;  
```  
  
 Voici le résultat obtenu :  
  
```xml
<NewRoot attr="5" />  
```  
  
 Dans l'exemple suivant, des expressions sont spécifiées pour la construction des attributs LocationID et SetupHrs. Ces expressions sont évaluées par rapport au document XML de la colonne Instruction. La valeur typée de l'expression est affectée aux attributs.  
  
```sql
SELECT Instructions.query('  
    declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        <FirstLocation   
         LocationID="{ (/AWMI:root/AWMI:Location[1]/@LocationID)[1] }"  
         SetupHrs = "{ (/AWMI:root/AWMI:Location[1]/@SetupHours)[1] }" >  
           { /AWMI:root/AWMI:Location[1]/AWMI:step }  
        </FirstLocation>   
') as Result   
FROM  Production.ProductModel  
where ProductModelID=7;  
```  
  
 Voici le résultat partiel :  
  
```xml
<FirstLocation LocationID="10" SetupHours="0.5" >  
  <AWMI:step ...   
  </AWMI:step>  
  ...  
</FirstLocation>  
```  
  
#### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   Les expressions d'attributs multiples ou mixtes (expression de chaîne et XQuery) ne sont pas prises en charge. Par exemple, comme le montre la requête suivante, vous construisez le document XML où `Item` est une constante et la valeur `5` est obtenue par évaluation d'une expression de requête :  
  
    ```xml
    <a attr="Item 5" />  
    ```  
  
     La requête suivante renvoie une erreur car la combinaison d'une chaîne constante avec une expression ({/x}) n'est pas prise en charge :  
  
    ```sql
    DECLARE @x xml  
    SET @x ='<x>5</x>'  
    SELECT @x.query( '<a attr="Item {/x}"/>' )   
    ```  
  
     Dans ce cas, les possibilités suivantes s'offrent à vous :  
  
    -   Former la valeur de l'attribut en concaténant deux valeurs atomiques. Ces valeurs atomiques sont sérialisées pour former la valeur de l'attribut et séparées par un espace :  
  
        ```sql
        SELECT @x.query( '<a attr="{''Item'', data(/x)}"/>' )   
        ```  
  
         Voici le résultat obtenu :  
  
        ```xml
        <a attr="Item 5" />  
        ```  
  
    -   Utilisez la [fonction concat](../xquery/functions-on-string-values-concat.md) pour concaténer les deux arguments de chaîne dans la valeur d’attribut obtenue :  
  
        ```sql
        SELECT @x.query( '<a attr="{concat(''Item'', /x[1])}"/>' )   
        ```  
  
         Dans ce cas, aucun espace n'est ajouté entre les deux valeurs de chaîne. Si vous souhaitez un espace entre les deux valeurs, vous devez le fournir explicitement.  
  
         Voici le résultat obtenu :  
  
        ```xml
        <a attr="Item5" />  
        ```  
  
-   L'utilisation de plusieurs expressions comme valeur d'attribut n'est pas prise en charge. Par exemple, la requête suivante renvoie une erreur :  
  
    ```sql
    DECLARE @x xml  
    SET @x ='<x>5</x>'  
    SELECT @x.query( '<a attr="{/x}{/x}"/>' )  
    ```  
  
-   Les séquences hétérogènes ne sont pas prises en charge. Toute tentative d'affectation d'une séquence hétérogène comme valeur d'attribut renvoie une erreur, comme le montre l'exemple suivant. Dans cet exemple, une séquence hétérogène, une chaîne « Item » et un élément <`x`>, est spécifiée en tant que valeur d’attribut :  
  
    ```sql
    DECLARE @x xml  
    SET @x ='<x>5</x>'  
    select @x.query( '<a attr="{''Item'', /x }" />')  
    ```  
  
     Si vous appliquez la fonction **Data ()** , la requête fonctionne, car elle récupère la valeur atomique de l’expression, `/x` , qui est concaténée avec la chaîne. Voici une séquence de valeurs atomiques :  
  
    ```sql
    SELECT @x.query( '<a attr="{''Item'', data(/x)}"/>' )   
    ```  
  
     Voici le résultat obtenu :  
  
    ```xml
    <a attr="Item 5" />  
    ```  
  
-   L'ordre des nœuds d'attribut est imposé au cours de la sérialisation plutôt que pendant la vérification des types statiques. Par exemple, la requête ci-dessous échoue car elle tente d'ajouter un attribut après un nœud qui n'est pas un nœud d'attribut.  
  
    ```sql
    select convert(xml, '').query('  
    element x { attribute att { "pass" }, element y { "Element text" }, attribute att2 { "fail" } }  
    ')  
    go  
    ```  
  
     La requête ci-dessus retourne l'erreur suivante :  
  
    ```  
    XML well-formedness check: Attribute cannot appear outside of element declaration. Rewrite your XQuery so it returns well-formed XML.  
    ```  
  
### <a name="adding-namespaces"></a>Ajout d'espaces de noms  
 Lorsque vous construisez un document XML à l'aide des constructeurs directs, vous pouvez qualifier les noms des attributs et des éléments construits à l'aide d'un préfixe d'espace de noms. Vous pouvez lier le préfixe à l'espace de noms des manières suivantes :  
  
-   À l'aide d'un attribut de déclaration d'espace de noms.  
  
-   À l'aide de la clause WITH XMLNAMESPACES.  
  
-   Dans le prologue XQuery.  
  
#### <a name="using-a-namespace-declaration-attribute-to-add-namespaces"></a>Utilisation d'un attribut de déclaration d'espace de noms pour ajouter des espaces de noms  
 L’exemple suivant utilise un attribut de déclaration d’espace de noms dans la construction d’un élément <`a`> pour déclarer un espace de noms par défaut. La construction de l’élément enfant <`b`> annule la déclaration de l’espace de noms par défaut déclaré dans l’élément parent.  
  
```sql
declare @x xml  
set @x ='<x>5</x>'  
select @x.query( '  
  <a xmlns="a">  
    <b xmlns=""/>  
  </a>' )   
```  
  
 Voici le résultat obtenu :  
  
```xml
<a xmlns="a">  
  <b xmlns="" />  
</a>  
```  
  
 Vous pouvez affecter un préfixe à l'espace de noms. Le préfixe est spécifié dans la construction de l’élément <`a`>.  
  
```sql
declare @x xml  
set @x ='<x>5</x>'  
select @x.query( '  
  <x:a xmlns:x="a">  
    <b/>  
  </x:a>' )  
```  
  
 Voici le résultat obtenu :  
  
```xml
<x:a xmlns:x="a">  
  <b />  
</x:a>  
```  
  
 Vous pouvez annuler la déclaration d'un espace de noms par défaut dans la construction XML, mais pas celle d'un préfixe d'espace de noms. La requête suivante retourne une erreur, car vous ne pouvez pas annuler la déclaration d’un préfixe tel que spécifié dans la construction de l’élément <`b`>.  
  
```sql
declare @x xml  
set @x ='<x>5</x>'  
select @x.query( '  
  <x:a xmlns:x="a">  
    <b xmlns:x=""/>  
  </x:a>' )  
```  
  
 L'espace de noms nouvellement construit peut être utilisé dans la requête. Par exemple, la requête suivante déclare un espace de noms dans la construction de l’élément, <`FirstLocation`> et spécifie le préfixe dans les expressions pour les valeurs d’attribut LocationID et SetupHrs.  
  
```sql
SELECT Instructions.query('  
        <FirstLocation xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"  
         LocationID="{ (/AWMI:root/AWMI:Location[1]/@LocationID)[1] }"  
         SetupHrs = "{ (/AWMI:root/AWMI:Location[1]/@SetupHours)[1] }" >  
           { /AWMI:root/AWMI:Location[1]/AWMI:step }  
        </FirstLocation>   
') as Result   
FROM  Production.ProductModel  
where ProductModelID=7  
```  
  
 La création d'un nouveau préfixe d'espace de noms de cette façon écrase toute déclaration d'espace de noms déjà existante pour ce préfixe. Par exemple, la déclaration d’espace de noms, `AWMI="https://someURI"` , dans le prologue de la requête est remplacée par la déclaration d’espace de noms dans l' `FirstLocation` élément <>.  
  
```sql
SELECT Instructions.query('  
declare namespace AWMI="https://someURI";  
        <FirstLocation xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"  
         LocationID="{ (/AWMI:root/AWMI:Location[1]/@LocationID)[1] }"  
         SetupHrs = "{ (/AWMI:root/AWMI:Location[1]/@SetupHours)[1] }" >  
           { /AWMI:root/AWMI:Location[1]/AWMI:step }  
        </FirstLocation>   
') as Result   
FROM  Production.ProductModel  
where ProductModelID=7  
```  
  
#### <a name="using-a-prolog-to-add-namespaces"></a>Utilisation d'un prologue pour ajouter des espaces de noms  
 Cet exemple illustre l'ajout d'espaces de noms au document XML construit. Un espace de noms par défaut est déclaré dans le prologue de la requête.  
  
```sql
declare @x xml  
set @x ='<x>5</x>'  
select @x.query( '  
           declare default element namespace "a";  
            <a><b xmlns=""/></a>' )  
```  
  
 Notez que dans la construction de l’élément <`b`>, l’attribut de déclaration d’espace de noms est spécifié avec une chaîne vide comme valeur. Cette opération annule la déclaration de l'espace de noms par défaut déclaré dans le parent.  
  

Voici le résultat obtenu :  

```xml
<a xmlns="a">  
  <b xmlns="" />  
</a>  
```  
  
### <a name="xml-construction-and-white-space-handling"></a>Construction XML et gestion des espaces blancs  
 Le contenu des éléments dans la construction XML peut comprendre des espaces blancs. Ces caractères sont traités des manières suivantes :  
  
-   Les espaces blancs dans les URI d’espace de noms sont traités comme le type XSD **anyURI**. Plus particulièrement, voici comment ils sont gérés :  
  
    -   Tous les espaces blancs de début et de fin sont tronqués.  
  
    -   Les valeurs d'espaces blancs internes sont réduites à un seul espace.  
  
-   Les caractères de saut de ligne dans le contenu des attributs sont remplacés par des espaces. Tous les autres espaces blancs demeurent tels quels.  
  
-   L'espace blanc dans les éléments est conservé.  
  
 L'exemple suivant illustre la gestion des espaces blancs dans la construction XML :  
  
```sql
-- line feed is repaced by space.  
declare @x xml  
set @x=''  
select @x.query('  
  
declare namespace myNS="   https://       
 abc/  
xyz  
  
";  
<test attr="    my   
test   attr   
value    " >  
  
<a>  
  
This     is  a  
  
test  
  
</a>  
</test>  
') as XML_Result  
  
```  
  
 Voici le résultat obtenu :  
  
```xml
-- result  
<test attr="<test attr="    my test   attr  value    "><a>  
  
This     is  a  
  
test  
  
</a></test>  
"><a>  
  
This     is  a  
  
test  
  
</a></test>  
```  
  
### <a name="other-direct-xml-constructors"></a>Autres constructeurs XML directs  
 Les constructeurs des instructions de traitement et des commentaires XML utilisent la même syntaxe que la construction XML correspondante. En outre, les constructeurs calculés pour les nœuds de texte sont pris en charge, mais sont essentiellement utilisés dans le langage DML XML pour construire des nœuds de texte.  
  
 **Remarque** Pour obtenir un exemple d’utilisation d’un constructeur de nœud de texte explicite, consultez l’exemple spécifique dans [insert &#40;XML DML&#41;](../t-sql/xml/insert-xml-dml.md).  
  
 Dans la requête suivante, le document XML construit comprend un élément, deux attributs, un commentaire et une instruction de traitement. Notez qu’une virgule est utilisée avant la `FirstLocation`> <, car une séquence est en cours de construction.  
  
```sql
SELECT Instructions.query('  
  declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   <?myProcessingInstr abc="value" ?>,   
   <FirstLocation   
        WorkCtrID = "{ (/AWMI:root/AWMI:Location[1]/@LocationID)[1] }"  
        SetupHrs = "{ (/AWMI:root/AWMI:Location[1]/@SetupHours)[1] }" >  
       <!-- some comment -->  
       <?myPI some processing instructions ?>  
       { (/AWMI:root/AWMI:Location[1]/AWMI:step) }  
    </FirstLocation>   
') as Result   
FROM Production.ProductModel  
where ProductModelID=7;  
  
```  
  
 Voici le résultat partiel :  
  
```xml
<?myProcessingInstr abc="value" ?>  
<FirstLocation WorkCtrID="10" SetupHrs="0.5">  
  <!-- some comment -->  
  <?myPI some processing instructions ?>  
  <AWMI:step xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">I  
  nsert <AWMI:material>aluminum sheet MS-2341</AWMI:material> into the <AWMI:tool>T-85A framing tool</AWMI:tool>.   
  </AWMI:step>  
    ...  
</FirstLocation>  
  
```  
  
## <a name="using-computed-constructors"></a>Utilisation de constructeurs calculés  
 . Dans ce cas, vous spécifiez les mots clés qui identifient le type de nœud à construire. Seuls les mots clés suivants sont pris en charge :  
  
-   element  
  
-   attribut  
  
-   text  
  
 Dans le cas des nœuds d'élément et d'attribut, ces mots clés sont suivis du nom du nœud ainsi que de l'expression, entre accolades, qui génère le contenu du nœud. Dans l'exemple suivant, vous construisez ce document XML :  
  
```xml
<root>  
  <ProductModel PID="5">Some text <summary>Some Summary</summary></ProductModel>  
</root>  
```  
  
 Voici la requête qui utilise des constructeurs calculés pour générer le document XML :  
  
```sql
declare @x xml  
set @x=''  
select @x.query('element root   
               {   
                  element ProductModel  
     {  
attribute PID { 5 },  
text{"Some text "},  
    element summary { "Some Summary" }  
 }  
               } ')  
  
```  
  
 L'expression qui génère le contenu du nœud peut spécifier une expression de requête.  
  
```sql
declare @x xml  
set @x='<a attr="5"><b>some summary</b></a>'  
select @x.query('element root   
               {   
                  element ProductModel  
     {  
attribute PID { /a/@attr },  
text{"Some text "},  
    element summary { /a/b }  
 }  
               } ')  
```  
  
 Les constructeurs d'éléments et d'attributs calculés, tels que définis dans la spécification XQuery, vous permettent de calculer les noms des nœuds. Lorsque vous utilisez des constructeurs directs dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], vous devez spécifier les noms des nœuds, tels que element et attribute, en tant que littéraux de constante. Par conséquent, il n'existe aucune différence entre les constructeurs directs et les constructeurs calculés pour les éléments et les attributs.  
  
 Dans l’exemple suivant, le contenu des nœuds construits est obtenu à partir des instructions de fabrication XML stockées dans la colonne instructions du type de données **XML** dans la table ProductModel.  
  
```sql
SELECT Instructions.query('  
  declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   element FirstLocation   
     {  
        attribute LocationID { (/AWMI:root/AWMI:Location[1]/@LocationID)[1] },  
        element   AllTheSteps { /AWMI:root/AWMI:Location[1]/AWMI:step }  
     }  
') as Result   
FROM  Production.ProductModel  
where ProductModelID=7  
```  
  
 Voici le résultat partiel :  
  
```xml
<FirstLocation LocationID="10">  
  <AllTheSteps>  
    <AWMI:step> ... </AWMI:step>  
    <AWMI:step> ... </AWMI:step>  
    ...  
  </AllTheSteps>  
</FirstLocation>    
```  
  
## <a name="additional-implementation-limitations"></a>Autres limites relatives à la mise en œuvre  
 Les constructeurs d'attributs calculés ne permettent pas de déclarer un nouvel espace de noms. En outre, les constructeurs calculés suivants ne sont pas pris en charge dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] :  
  
-   Constructeurs de nœud de document calculés  
  
-   Constructeurs d'instruction de traitement calculés  
  
-   Constructeurs de commentaire calculés  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions XQuery](../xquery/xquery-expressions.md)  
  
  
