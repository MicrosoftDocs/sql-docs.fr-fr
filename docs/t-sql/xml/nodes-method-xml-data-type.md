---
description: Méthode nodes() (type de données xml)
title: Méthode nodes() (type de données xml)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- nodes() method
- nodes method
ms.assetid: 7267fe1b-2e34-4213-8bbf-1c953822446c
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 71b636999583da9a957e72c0d8f3a5e0b9fe98bf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207514"
---
# <a name="nodes-method-xml-data-type"></a>Méthode nodes() (type de données xml)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

La méthode **nodes()** s’avère utile pour éclater une instance de type de données **xml** en données relationnelles. Elle vous permet d'identifier les nœuds à mapper dans une nouvelle ligne.  
  
Chaque instance de type de données **xml** possède un nœud de contexte fourni implicitement. Pour l’instance XML stockée dans une colonne ou variable, ce nœud est le nœud du document. Le nœud de document est le nœud implicite situé en haut de chaque instance de type de données **xml**.  
  
Le résultat de la méthode **nodes()** est un ensemble de lignes qui contient des copies logiques des instances XML d’origine. Dans ces copies logiques, le nœud de contexte de chaque instance de ligne correspond à l’un des nœuds identifiés avec l’expression de requête. Ainsi, les requêtes ultérieures peuvent naviguer par rapport à ces nœuds de contexte.  
  
Vous pouvez extraire plusieurs valeurs de l'ensemble de lignes. Par exemple, vous pouvez appliquer la méthode **value()** à l’ensemble de lignes renvoyé par **nodes()** , puis extraire plusieurs valeurs de l’instance XML d’origine. Appliquée à l’instance XML, la méthode **value()** ne retourne qu’une seule valeur.  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
nodes (XQuery) as Table(Column)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*XQuery*  
Littéral de chaîne, représentant une expression XQuery. Si l'expression de requête construit des nœuds, ceux-ci sont exposés dans l'ensemble de lignes obtenu. Si l’expression de requête aboutit à une séquence vide, l’ensemble de lignes est également vide. Si l'expression de requête aboutit de façon statique à une séquence qui contient des valeurs atomiques au lieu de nœuds, une erreur statique est déclenchée.  
  
*Table*(*Column*)  
Nom de table et nom de colonne de l'ensemble de lignes obtenu.  
  
## <a name="remarks"></a>Notes  
Prenons par exemple la table suivante :  
  
```sql
T (ProductModelID INT, Instructions XML)  
```  
  
Le document des instructions de fabrication suivant est stocké dans la table. Seule une partie de celui-ci est montrée. Le document mentionne trois sites de fabrication.  
  
```
<root>  
  <Location LocationID="10"...>  
     <step>...</step>  
     <step>...</step>  
      ...  
  </Location>  
  <Location LocationID="20" ...>  
       ...  
  </Location>  
  <Location LocationID="30" ...>  
       ...  
  </Location>  
</root>  
```  
  
Un appel de la méthode `nodes()` avec l'expression de requête `/root/Location` renverrait un ensemble de lignes composé de trois lignes, contenant chacune une copie logique du document XML d'origine et associant l'élément de contexte à l'un des nœuds `<Location>` :  
  
```
Product  
ModelID      Instructions  
----------------------------------  
1      <root><Location LocationID="10" ... />  
             <Location LocationID="20" ... />  
             <Location LocationID="30" .../></root>  
1      <root><Location LocationID="10" ... />  
             <Location LocationID="20" ... />  
             <Location LocationID="30" .../></root>  
1      <root><Location LocationID="10" ... />  
             <Location LocationID="20" ... />  
             <Location LocationID="30" .../></root>  
```  
  
Vous pouvez alors interroger cet ensemble de lignes à l’aide de méthodes de type de données **xml**. La requête suivante extrait la sous-arborescence de l'élément de contexte pour chaque ligne générée :  
  
```sql
SELECT T2.Loc.query('.')  
FROM T  
CROSS APPLY Instructions.nodes('/root/Location') AS T2(Loc)   
```  
  
Voici le résultat :  
  
```
ProductModelID  Instructions  
----------------------------------  
1        <Location LocationID="10" ... />  
1        <Location LocationID="20" ... />  
1        <Location LocationID="30" .../>  
```  
  
L’ensemble de lignes retourné a conservé les informations de type. Vous pouvez appliquer des méthodes de type de données **xml**, comme **query()** , **value()** , **exist()** et **nodes()** , au résultat d’une méthode **nodes()** . Toutefois, vous ne pouvez pas appliquer la méthode **modify()** pour modifier l’instance XML.  
  
En outre, le nœud de contexte figurant dans l’ensemble de lignes ne peut pas être matérialisé. Vous ne pouvez donc pas l’utiliser dans une instruction SELECT. Toutefois, vous pouvez l'utiliser dans IS NULL et COUNT(*).  
  
Les scénarios d’utilisation de la méthode **nodes()** sont les mêmes que ceux de la méthode [OPENXML &#40;Transact-SQL&#41;](../../t-sql/functions/openxml-transact-sql.md) qui affiche une vue XML de l’ensemble de lignes. Toutefois, vous n’avez pas besoin de recourir à des curseurs lorsque vous utilisez la méthode **nodes()** sur une table qui contient plusieurs lignes de documents XML.  
  
L’ensemble de lignes retourné par la méthode **nodes()** est un ensemble de lignes sans nom. Il doit donc être nommé explicitement à l’aide d’un alias.  
  
La fonction **nodes()** ne peut pas s’appliquer directement aux résultats d’une fonction définie par l’utilisateur. Pour utiliser la fonction **nodes()** avec le résultat d’une fonction scalaire définie par l’utilisateur, vous pouvez :
 
- Affecter le résultat de la fonction définie par l’utilisateur à une variable
- Utiliser une table dérivée pour affecter un alias de colonne à la valeur de retour d’une fonction définie par l’utilisateur, puis utiliser `CROSS APPLY` pour effectuer une sélection à partir de l’alias.  
  
L'exemple suivant illustre une utilisation de `CROSS APPLY` permettant d'opérer une sélection à partir du résultat d'une fonction définie par l'utilisateur.  
  
```sql
USE AdventureWorks;  
GO  
  
CREATE FUNCTION XTest()  
RETURNS XML  
AS  
BEGIN  
RETURN '<document/>';  
END;  
GO  
  
SELECT A2.B.query('.')  
FROM  
(SELECT dbo.XTest()) AS A1(X)   
CROSS APPLY X.nodes('.') A2(B);  
GO  
  
DROP FUNCTION XTest;  
GO  
```  
  
## <a name="examples"></a>Exemples  
  
### <a name="using-nodes-method-against-a-variable-of-xml-type"></a>Utilisation de la méthode nodes() par rapport à une variable de type xml  
L’exemple suivant montre un document XML qui possède un élément de niveau supérieur <`Root`> et trois éléments enfants <`row`>. La requête utilise la méthode `nodes()` pour définir un nœud de contexte distinct par élément <`row`>. La méthode `nodes()` renvoie un ensemble de lignes composé de trois lignes. Chaque ligne possède une copie logique du document XML d’origine et chaque nœud de contexte identifie un élément <`row`> distinct de ce document.  
  
La requête renvoie ensuite le nœud de contexte depuis chaque ligne :  
  
```sql
DECLARE @x XML   
SET @x='<Root>  
    <row id="1"><name>Larry</name><oflw>some text</oflw></row>  
    <row id="2"><name>moe</name></row>  
    <row id="3" />  
</Root>'  
SELECT T.c.query('.') AS result  
FROM   @x.nodes('/Root/row') T(c)  
GO  
```  
  
Dans l’exemple suivant, la méthode de requête retourne l’élément de contexte et son contenu :  
  
```
<row id="1"><name>Larry</name><oflw>some text</oflw></row>  
<row id="2"><name>moe</name></row>  
<row id="3"/>  
```  
  
L’application de l’accesseur parent aux nœuds de contexte renvoie l’élément <`Root`> pour les trois lignes :  
  
```sql
SELECT T.c.query('..') AS result  
FROM   @x.nodes('/Root/row') T(c)  
GO  
```  
  
Voici le résultat :  
  
```
<Root>  
    <row id="1"><name>Larry</name><oflw>some text</oflw></row>  
    <row id="2"><name>moe</name></row>  
    <row id="3" />  
</Root>  
<Root>  
    <row id="1"><name>Larry</name><oflw>some text</oflw></row>  
    <row id="2"><name>moe</name></row>  
    <row id="3" />  
</Root>  
<Root>  
    <row id="1"><name>Larry</name><oflw>some text</oflw></row>  
    <row id="2"><name>moe</name></row>  
    <row id="3" />  
</Root>  
```  
  
### <a name="specifying-the-nodes-method-against-a-column-of-xml-type"></a>Spécification de la méthode nodes() par rapport à une colonne de type xml  
Cet exemple utilise les instructions de fabrication de vélo, stockées dans la colonne Instructions de type **xml** de la table **ProductModel**.  
  
Dans l’exemple suivant, la méthode `nodes()` est spécifiée par rapport à la colonne `Instructions` de type **xml** de la table `ProductModel`.  
  
La méthode `nodes()` définit les éléments <`Location`> en tant que nœuds de contexte en spécifiant le chemin d’accès `/MI:root/MI:Location`. L’ensemble de lignes obtenu comprend une copie logique du document d’origine par nœud <`Location`> du document et le nœud de contexte a pour valeur l’élément <`Location`>. Par conséquent, la fonction `nodes()` fournit un ensemble de nœuds de contexte <`Location`>.  
  
La méthode `query()` appliquée à cet ensemble de lignes demande `self::node` et renvoie l’élément `<Location>` de chaque ligne.  
  
Dans cet exemple, la requête définit chaque élément <`Location`> en tant que nœud de contexte du document d’instructions de fabrication d’un modèle de produit spécifique. Ces nœuds de contexte vous permettent d’effectuer les opérations d’extraction suivantes :  
  
- Rechercher les ID d’emplacement dans chaque élément <`Location`>  
  
- Récupérer les étapes de fabrication (éléments enfants <`step`>) dans chaque <`Location`>  
  
Cette requête renvoie l'élément de contexte, dans lequel est spécifiée la syntaxe abrégée `'.'` de `self::node()`, dans la méthode `query()`.  
  
Notez les points suivants :
  
- La méthode `nodes()` est appliquée à la colonne Instructions et renvoie l'ensemble de lignes `T (C)`. Cet ensemble de lignes contient des copies logiques du document d'origine des instructions de fabrication et indique `/root/Location` en guise d'élément de contexte.  
  
- CROSS APPLY applique `nodes()` à chaque ligne de la table `Instructions` et renvoie uniquement les lignes qui génèrent un ensemble de résultats.  
  
    ```sql  
    SELECT C.query('.') as result  
    FROM Production.ProductModel  
    CROSS APPLY Instructions.nodes('  
    declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
    /MI:root/MI:Location') as T(C)  
    WHERE ProductModelID=7  
    ```  
  
  Voici le résultat partiel :  
  
    ```
    <MI:Location LocationID="10"  ...>  
       <MI:step ... />  
          ...  
    </MI:Location>  
    <MI:Location LocationID="20"  ... >  
        <MI:step ... />  
          ...  
    </MI:Location>  
    ...  
    ```  
  
### <a name="applying-nodes-to-the-rowset-returned-by-another-nodes-method"></a>Application de la méthode nodes() à l'ensemble de lignes renvoyé par une autre méthode nodes()  
Le code suivant interroge les documents XML des instructions de fabrication stockés dans la colonne `Instructions` de la table `ProductModel`. La requête renvoie un ensemble de lignes qui contient l'ID du modèle de produit, ainsi que les sites et les étapes de fabrication.  
  
Notez les points suivants :  
  
- La méthode `nodes()` est appliquée à la colonne `Instructions` et renvoie l'ensemble de lignes `T1 (Locations)`. Cet ensemble de lignes contient des copies logiques du document d'origine des instructions de fabrication et indique `/root/Location` en guise de contexte d'élément.  
  
- `nodes()` est appliqué à l'ensemble de lignes `T1 (Locations)` et renvoie l'ensemble de lignes `T2 (steps)`. Cet ensemble de lignes contient des copies logiques du document d'origine des instructions de fabrication et indique `/root/Location/step` en guise de contexte d'élément.  
  
```sql
SELECT ProductModelID, Locations.value('./@LocationID','int') AS LocID,  
steps.query('.') AS Step         
FROM Production.ProductModel         
CROSS APPLY Instructions.nodes('         
declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";         
/MI:root/MI:Location') AS T1(Locations)         
CROSS APPLY T1.Locations.nodes('         
declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";         
./MI:step ') AS T2(steps)         
WHERE ProductModelID=7         
GO         
```  
  
Voici le résultat :  
  
```
ProductModelID LocID Step         
----------------------------         
7      10   <step ... />         
7      10   <step ... />         
...         
7      20   <step ... />         
7      20   <step ... />         
7      20   <step ... />         
...         
```  
  
La requête déclare le préfixe `MI` deux fois. À la place, vous pouvez recourir à `WITH XMLNAMESPACES` pour déclarer le préfixe une fois et l'utiliser dans la requête :  
  
```sql
WITH XMLNAMESPACES (  
   'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions' AS MI)  
  
SELECT ProductModelID, Locations.value('./@LocationID','int') AS LocID,  
steps.query('.') AS Step         
FROM Production.ProductModel         
CROSS APPLY Instructions.nodes('         
/MI:root/MI:Location') AS T1(Locations)         
CROSS APPLY T1.Locations.nodes('         
./MI:step ') as T2(steps)         
WHERE ProductModelID=7         
GO    
```  
  
## <a name="see-also"></a>Voir aussi  
[Ajouter des espaces de noms aux requêtes avec WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
[Créer des instances de données XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
[Méthodes de type de données xml](../../t-sql/xml/xml-data-type-methods.md)  
  
  
