---
title: Fonction Data (XQuery) | Microsoft Docs
description: Apprenez à utiliser la fonction XQuery Data () pour retourner la valeur typée de chaque élément dans une séquence d’éléments spécifiée.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:data function
- data function [XQuery]
ms.assetid: 511b5d7d-c679-4cb2-a3dd-170cc126f49d
author: rothja
ms.author: jroth
ms.openlocfilehash: 4e20c00d93082994cd5dc230bfe1b4356b3ed031
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338498"
---
# <a name="data-accessor-functions---data-xquery"></a>Fonctions d’accesseur de données : data (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Retourne la valeur typée pour chaque élément spécifié par *$arg*.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:data ($arg as item()*) as xdt:untypedAtomic*  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Séquence d'éléments dont les valeurs typées seront renvoyées.  
  
## <a name="remarks"></a>Notes  
 Les points suivants s'appliquent aux valeurs typées :  
  
-   La valeur typée d'une valeur atomique est la valeur atomique.  
  
-   La valeur typée d'un nœud de texte est la valeur chaîne du nœud de texte.  
  
-   La valeur typée d'un commentaire est la valeur de chaîne du commentaire.  
  
-   La valeur typée d'une instruction de traitement est le contenu de l'instruction de traitement sans le nom cible de l'instruction de traitement.  
  
-   La valeur typée d'un nœud de document est sa valeur de chaîne.  
  
 Les points suivants s'appliquent aux nœuds d'attribut et d'élément :  
  
-   Si un nœud d'attribut est typé avec un type de schéma XML, sa valeur typée est la valeur typée correspondante.  
  
-   Si le nœud d’attribut n’est pas typé, sa valeur typée est égale à sa valeur de chaîne qui est retournée en tant qu’instance de **xdt : untypedAtomic**.  
  
-   Si le nœud d’élément n’a pas été typé, sa valeur typée est égale à sa valeur de chaîne qui est retournée en tant qu’instance de **xdt : untypedAtomic**.  
  
 Les points suivants s'appliquent aux nœuds d'élément typés :  
  
-   Si l’élément a un type de contenu simple, **Data ()** retourne la valeur typée de l’élément.  
  
-   Si le nœud est de type complexe, y compris XS : anyType, **Data ()** renvoie une erreur statique.  
  
 Bien que l’utilisation de la fonction **Data ()** soit souvent facultative, comme indiqué dans les exemples suivants, la spécification de la fonction **Data ()** augmente explicitement la lisibilité des requêtes. Pour plus d’informations, consultez [principes de base de XQuery](../xquery/xquery-basics.md).  
  
 Vous ne pouvez pas spécifier de **données ()** sur du code XML construit, comme illustré ci-dessous :  
  
```  
declare @x xml  
set @x = ''  
select @x.query('data(<SomeNode>value</SomeNode>)')  
```  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la base de données AdventureWorks.  
  
### <a name="a-using-the-data-xquery-function-to-extract-typed-value-of-a-node"></a>R. Utilisation de la fonction XQuery data() pour extraire la valeur typée d'un nœud  
 La requête suivante illustre l’utilisation de la fonction **Data ()** pour extraire les valeurs d’un attribut, d’un élément et d’un nœud de texte :  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS p1,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query(N'  
 for $pd in //p1:ProductDescription  
 return   
    <Root   
      ProductID = "{ data( ($pd//@ProductModelID)[1] ) }"   
      Feature =   "{ data( ($pd/p1:Features/wm:Warranty/wm:Description)[1] ) }" >  
    </Root>  
 ') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Voici le résultat obtenu :  
  
```  
<Root ProductID="19" Feature="parts and labor"/>  
```  
  
 Comme mentionné, la fonction **Data ()** est facultative lorsque vous construisez des attributs. Si vous ne spécifiez pas la fonction **Data ()** , elle est implicitement utilisée. La requête suivante produit les mêmes résultats que la précédente :  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS p1,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
      for $pd in //p1:ProductDescription  
         return   
          <Root    
                ProductID = "{ ($pd/@ProductModelID)[1] }"    
                Feature =   "{ ($pd/p1:Features/wm:Warranty/wm:Description)[1] }" >  
           </Root>  
 ') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Les exemples suivants illustrent des instances dans lesquelles la fonction **Data ()** est requise.  
  
 Dans la requête suivante, **$PD/P1 : Specifications/Material** retourne l' `Material` élément <>. En outre, les **données ($PD/P1 : Specifications/Material)** retournent des données de type caractère sous la forme xdt : untypedAtomic, car <`Material`> n’est pas typée. Lorsque l’entrée n’est pas typée, le résultat de **Data ()** est de type **xdt : untypedAtomic**.  
  
```  
SELECT CatalogDescription.query('  
declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
      for $pd in //p1:ProductDescription  
         return   
          <Root>  
             { $pd/p1:Specifications/Material }  
             { data($pd/p1:Specifications/Material) }  
           </Root>  
 ') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Voici le résultat obtenu :  
  
```  
<Root>  
  <Material>Almuminum Alloy</Material>Almuminum Alloy  
</Root>  
```  
  
 Dans la requête suivante, les **données ($PD/P1 : features/WM : Warranty)** renvoient une erreur statique, car <`Warranty`> est un élément de type complexe.  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS p1,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
 <Root>  
   {     /p1:ProductDescription/p1:Features/wm:Warranty }  
   { data(/p1:ProductDescription/p1:Features/wm:Warranty) }  
 </Root>  
 ') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID = 23  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
