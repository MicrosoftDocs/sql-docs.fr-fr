---
description: insert (XML DML)
title: insert (XML DML)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- XML DML [SQL Server]
- inserting nodes
- insert keyword [XML DML]
- insert XML DML statement
ms.assetid: 0c95c2b3-5cc2-4c38-9e25-86493096c442
author: MightyPen
ms.author: genemi
ms.openlocfilehash: d5eae7b06bb10c356bbb1c3975b05dd7cd267708
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199334"
---
# <a name="insert-xml-dml"></a>insert (XML DML)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Insère un ou plusieurs nœuds identifiés par *Expression1* en tant que nœuds enfants ou frères du nœud identifié par *Expression2*.  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql 
insert Expression1 (  
{AS first | AS last} INTO | AFTER | BEFORE  
Expression2  
)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *Expression1*  
 Identifie un ou plusieurs nœuds à insérer. Il peut s’agir d’une instance XML constante, d’une référence à une instance de type de données XML typée de la même collection de schémas XML à laquelle la méthode modify est appliquée, d’une instance de type de données XML non typée à l’aide d’une fonction **sql:column()** /**sql:variable()** autonome ou encore d’une expression de requête Xml. L'expression peut résulter en un nœud, un nœud de texte ou une séquence ordonnée de nœuds. Le résultat ne peut pas être le nœud racine (/). Si l'expression a pour résultat une valeur ou une séquence de valeurs, ces valeurs sont insérées en tant que nœud de texte unique avec un espace séparant chaque valeur dans la séquence. Si vous spécifiez plusieurs nœuds sous la forme d'une constante, les nœuds sont inclus entre parenthèses et sont séparés par des virgules. Vous ne pouvez pas insérer des séquences hétérogènes telles qu'une séquence d'éléments, d'attributs ou de valeurs. Si *Expression1* est une séquence vide, aucune insertion n’a lieu et aucune erreur n’est retournée.  
  
 en  
 Les nœuds identifiés par *Expression1* sont insérés en tant que descendants directs (nœuds enfants) du nœud identifié par *Expression2*. Si le nœud dans *Expression2* a déjà un ou plusieurs nœuds enfants, vous devez utiliser **as first** ou **as last** pour spécifier où le nouveau nœud doit être ajouté. Par exemple, respectivement au début et à la fin de la liste des enfants. Les mots clés **as first** et **as last** sont ignorés lorsque des attributs sont insérés.  
  
 after  
 Les nœuds identifiés par *Expression1* sont insérés en tant que frères directement après le nœud identifié par *Expression2*. Le mot clé **after** ne peut pas être utilisé pour insérer des attributs. Par exemple, il ne peut pas être utilisé pour insérer un constructeur d'attribut ou pour retourner un attribut à partir d'une requête XQuery.  
  
 before  
 Les nœuds identifiés par *Expression1* sont insérés en tant que frères directement avant le nœud identifié par *Expression2*. Le mot clé **before** ne peut pas être utilisé pour l’insertion d’attributs. Par exemple, il ne peut pas être utilisé pour insérer un constructeur d'attribut ou pour retourner un attribut à partir d'une requête XQuery.  
  
 *Expression2*  
 Identifie un nœud. Les nœuds identifiés dans *Expression1* sont insérés par rapport au nœud identifié par *Expression2*. Il peut s'agir d'une expression XQuery qui retourne une référence à un nœud qui existe dans le document actuellement référencé. Si plusieurs nœuds sont retournés, l'insertion échoue. Si *Expression2* retourne une séquence vide, aucune insertion n’a lieu et aucune erreur n’est retournée. Si *Expression2* n’est statiquement pas un singleton, une erreur statique est générée. *Expression2* ne peut pas être une instruction, un commentaire ni un attribut de traitement. Notez que *Expression2* doit être une référence à un nœud existant dans le document et pas un nœud construit.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-inserting-element-nodes-into-the-document"></a>R. Insertion de nœuds d'élément dans le document  
 L'exemple suivant illustre l'insertion d'éléments dans un document. En premier lieu, un document XML est affecté à une variable de type **xml**. Ensuite, par l’intermédiaire de plusieurs instructions DML XML **insert**, l’exemple illustre l’insertion des nœuds d’élément dans le document. Après chaque insertion, l'instruction SELECT affiche le résultat.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;         
SET @myDoc = '<Root>         
    <ProductDescription ProductID="1" ProductName="Road Bike">         
        <Features>         
        </Features>         
    </ProductDescription>         
</Root>'  ;       
SELECT @myDoc;     
-- insert first feature child (no need to specify as first or as last)         
SET @myDoc.modify('         
INSERT <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>   
INTO (/Root/ProductDescription/Features)[1]') ;  
SELECT @myDoc ;        
-- insert second feature. We want this to be the first in sequence so use 'as first'         
SET @myDoc.modify('         
INSERT <Warranty>1 year parts and labor</Warranty>          
AS first         
INTO (/Root/ProductDescription/Features)[1]         
')  ;       
SELECT @myDoc  ;       
-- insert third feature child. This one is the last child of <Features> so use 'as last'         
SELECT @myDoc         
SET @myDoc.modify('         
INSERT <Material>Aluminium</Material>          
AS last         
INTO (/Root/ProductDescription/Features)[1]         
')         
SELECT @myDoc ;        
-- Add fourth feature - this time as a sibling (and not a child)         
-- 'after' keyword is used (instead of as first or as last child)         
SELECT @myDoc  ;       
SET @myDoc.modify('         
INSERT <BikeFrame>Strong long lasting</BikeFrame>   
AFTER (/Root/ProductDescription/Features/Material)[1]         
')  ;       
SELECT @myDoc;  
GO  
```  
  
 Notez que des expressions de chemin d'accès différentes dans cet exemple spécifient "[1]" comme exigence par type statique. Cela garantit l'unicité du nœud cible.  
  
### <a name="b-inserting-multiple-elements-into-the-document"></a>B. Insertion de plusieurs éléments dans le document  
 Dans l’exemple suivant, un document est affecté au préalable à une variable de type **xml**. Ensuite, une séquence de deux éléments, représentant les caractéristiques du produit, est affectée à une deuxième variable de type **xml**. Cette séquence est alors insérée dans la première variable.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc = N'<Root>             
<ProductDescription ProductID="1" ProductName="Road Bike">             
    <Features> </Features>             
</ProductDescription>             
</Root>';  
DECLARE @newFeatures xml;  
SET @newFeatures = N'<Warranty>1 year parts and labor</Warranty>            
<Maintenance>3 year parts and labor extended maintenance is available</Maintenance>';           
-- insert new features from specified variable            
SET @myDoc.modify('             
insert sql:variable("@newFeatures")             
into (/Root/ProductDescription/Features)[1] ')             
SELECT @myDoc;  
GO  
```  
  
### <a name="c-inserting-attributes-into-a-document"></a>C. Insertion d'attributs dans un document  
 L’exemple suivant illustre l’insertion d’attributs dans un document. En premier lieu, un document est affecté à une variable de type **xml**. Ensuite, une série d’instructions DML XML **insert** est utilisée pour insérer les attributs dans le document. Après chaque insertion d'attribut, l'instruction SELECT affiche le résultat.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;            
SET @myDoc =   
'<Root>             
    <Location LocationID="10" >             
        <step>Manufacturing step 1 at this work center</step>             
        <step>Manufacturing step 2 at this work center</step>             
    </Location>             
</Root>' ;  
SELECT @myDoc;          
-- insert LaborHours attribute             
SET @myDoc.modify('             
insert attribute LaborHours {".5" }             
into (/Root/Location[@LocationID=10])[1] ');           
SELECT @myDoc;          
-- insert MachineHours attribute but its value is retrived from a sql variable @Hrs             
DECLARE @Hrs FLOAT;            
SET @Hrs =.2;          
SET @myDoc.modify('             
insert attribute MachineHours {sql:variable("@Hrs") }             
into   (/Root/Location[@LocationID=10])[1] ');            
SELECT @myDoc;             
-- insert sequence of attribute nodes (note the use of ',' and ()              
-- around the attributes.             
SET @myDoc.modify('             
insert (              
           attribute SetupHours {".5" },             
           attribute SomeOtherAtt {".2"}             
        )             
INTO (/Root/Location[@LocationID=10])[1] ');             
SELECT @myDoc;  
GO  
```  
  
### <a name="d-inserting-a-comment-node"></a>D. Insertion d'un nœud de commentaire  
 Dans cette requête, un document XML est d’abord affecté à une variable de type **xml**. Ensuite, XML DML est utilisé pour insérer un nœud de commentaire après le premier élément <`step`>.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;             
SET @myDoc =   
'<Root>             
    <Location LocationID="10" >             
        <step>Manufacturing step 1 at this work center</step>             
        <step>Manufacturing step 2 at this work center</step>             
    </Location>             
</Root>' ;           
SELECT @myDoc;             
SET @myDoc.modify('             
insert <!-- some comment -->             
after (/Root/Location[@LocationID=10]/step[1])[1] ');            
SELECT @myDoc;  
GO  
```  
  
### <a name="e-inserting-a-processing-instruction"></a>E. Insertion d'une instruction de traitement  
 Dans la requête suivante, un document XML est d’abord affecté à une variable de type **xml**. Ensuite, le mot clé XML DML est utilisé pour insérer une instruction de traitement au début du document.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc =   
'<Root>   
    <Location LocationID="10" >   
        <step>Manufacturing step 1 at this work center</step>   
        <step>Manufacturing step 2 at this work center</step>   
    </Location>   
</Root>' ;  
SELECT @myDoc ;  
SET @myDoc.modify('   
insert <?Program = "Instructions.exe" ?>   
before (/Root)[1] ') ;  
SELECT @myDoc ;  
GO  
```  
  
### <a name="f-inserting-data-using-a-cdata-section"></a>F. Insertion de données à l'aide d'une section CDATA  
 Quand vous insérez du texte qui inclut des caractères non valides en XML, tels que < ou >, vous pouvez utiliser des sections CDATA pour insérer les données comme cela est illustré dans la requête ci-dessous. La requête spécifie une section CDATA, mais elle est ajoutée comme nœud de texte avec les caractères non valides convertis en entités. Par exemple, `<` est enregistré sous la forme `&lt;`.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;             
SET @myDoc =   
'<Root>             
    <ProductDescription ProductID="1" ProductName="Road Bike">             
        <Features> </Features>             
    </ProductDescription>             
</Root>' ;            
SELECT @myDoc ;            
SET @myDoc.modify('             
insert <![CDATA[ <notxml> as text </notxml> or cdata ]]>   
into  (/Root/ProductDescription/Features)[1] ') ;   
SELECT @myDoc ;  
GO  
```  
  
 La requête insère un nœud de texte dans l’élément <`Features`> :  
  
```  
<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features> &lt;notxml@gt; as text &lt;/notxml&gt; or cdata </Features>  
</ProductDescription>  
</Root>       
```  
  
### <a name="g-inserting-text-node"></a>G. Insertion d'un nœud de texte  
 Dans cette requête, un document XML est d’abord affecté à une variable de type **xml**. Ensuite, XML DML est utilisé pour insérer un nœud de texte comme premier enfant de l’élément <`Root`>. Le constructeur de texte est utilisé pour spécifier le texte.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc = '<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  
</Features>  
</ProductDescription>  
</Root>'  
SELECT @myDoc;  
SET @myDoc.modify('  
 insert text{"Product Catalog Description"}   
 as first into (/Root)[1]  
');  
SELECT @myDoc;  
```  
  
### <a name="h-inserting-a-new-element-into-an-untyped-xml-column"></a>H. Insertion d'un nouvel élément dans une colonne xml non typée  
 L’exemple suivant applique une instruction DML XML pour mettre à jour une instance XML stockée dans une colonne de type **xml** :  
  
```sql
USE AdventureWorks;  
GO  
CREATE TABLE T (i INT, x XML);  
GO  
INSERT INTO T VALUES(1,'<Root>  
    <ProductDescription ProductID="1" ProductName="Road Bike">  
        <Features>  
            <Warranty>1 year parts and labor</Warranty>  
            <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
        </Features>  
    </ProductDescription>  
</Root>');  
GO  
-- insert a new element  
UPDATE T  
SET x.modify('insert <Material>Aluminium</Material> as first  
  into   (/Root/ProductDescription/Features)[1]  
');  
GO  
```  
  
 De nouveau, quand le nœud d’élément <`Material`> est inséré, l’expression de chemin d’accès doit retourner une seule cible. Cela est spécifié explicitement par l'ajout de [1] à la fin de l'expression.  
  
```sql
-- check the update  
SELECT x.query(' //ProductDescription/Features')  
FROM T;  
GO  
```  
  
### <a name="i-inserting-based-on-an-if-condition-statement"></a>I. Insertion basée sur une instruction de condition if  
 Dans l’exemple suivant, une condition IF est spécifiée dans le cadre d’Expression1, dans l’instruction DML XML **insert**. Si la condition a la valeur True, un attribut est ajouté à l’élément <`WorkCenter`>.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc =   
'<Root>  
    <Location LocationID="10" LaborHours="1.2" >  
        <step>Manufacturing step 1 at this work center</step>  
    <step>Manufacturing step 2 at this work center</step>  
    </Location>  
</Root>';  
SELECT @myDoc  
SET @myDoc.modify('  
insert  
if (/Root/Location[@LocationID=10])  
then attribute MachineHours {".5"}  
else ()  
    as first into   (/Root/Location[@LocationID=10])[1] ');  
SELECT @myDoc;  
GO  
```  
  
 L’exemple suivant est similaire, si ce n’est que l’instruction DML XML **insert** insère un élément dans le document si la condition a la valeur True. C'est-à-dire, si l’élément <`WorkCenter`> possède deux ou moins de deux éléments enfants <`step`>.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc =   
'<Root>  
    <Location LocationID="10" LaborHours="1.2" >  
        <step>Manufacturing step 1 at this work center</step>  
        <step>Manufacturing step 2 at this work center</step>  
    </Location>  
</Root>';  
SELECT @myDoc;  
SET @myDoc.modify('  
insert  
if (count(/Root/Location/step) <= 2)  
then element step { "This is a new step" }  
else ()  
    as last into   (/Root/Location[@LocationID=10])[1] ');  
SELECT @myDoc;  
GO  
```  
  
 Voici le résultat obtenu :  
  
```  
<Root>  
 <WorkCenter WorkCenterID="10" LaborHours="1.2">  
  <step>Manufacturing step 1 at this work center</step>  
  <step>Manufacturing step 2 at this work center</step>  
  <step>This is a new step</step>  
 </WorkCenter>  
```  
  
### <a name="j-inserting-nodes-in-a-typed-xml-column"></a>J. Insertion de nœuds dans une colonne xml typée  
 Dans cet exemple, un élément et un attribut sont insérés dans un document XML d’instructions de fabrication, stocké dans une colonne **xml** typée.  
  
 Vous créez d’abord une table (T) avec une colonne **xml** typée dans la base données AdventureWorks. Ensuite, vous copiez une instance XML des instructions de fabrication de la colonne Instructions dans la table ProductModel vers la table T. Les insertions sont ensuite appliquées à l'instance XML dans la table T.  
  
```sql
USE AdventureWorks;  
GO            
DROP TABLE T;  
GO             
CREATE TABLE T(
  ProductModelID INT PRIMARY KEY,    
  Instructions XML (Production.ManuInstructionsSchemaCollection));  
GO  
INSERT T              
    SELECT ProductModelID, Instructions             
    FROM Production.ProductModel             
    WHERE ProductModelID=7;  
GO             
SELECT Instructions             
FROM T;  
-- now insertion begins             
--1) insert a new manu. Location. The <Root> specified as              
-- expression 2 in the insert() must be singleton.      
UPDATE T   
SET Instructions.modify('   
declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
insert <MI:Location LocationID="1000" >   
           <MI:step>New instructions go here</MI:step>   
         </MI:Location>   
as first   
into   (/MI:root)[1]   
') ;  
  
SELECT Instructions             
FROM T ;  
-- 2) insert attributes in the new <Location>             
UPDATE T             
SET Instructions.modify('             
declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";             
insert attribute LaborHours { "1000" }             
into (/MI:root/MI:Location[@LocationID=1000])[1] ');   
GO             
SELECT Instructions             
FROM T ;  
GO             
--cleanup             
DROP TABLE T ;  
GO             
```  
  
## <a name="see-also"></a>Voir aussi  
 [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Créer des instances de données XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [méthodes de type de données xml](../../t-sql/xml/xml-data-type-methods.md)   
 [Langage de manipulation de données XML &#40;DML XML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
