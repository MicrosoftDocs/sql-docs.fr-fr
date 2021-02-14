---
title: Fonction de chaîne (XQuery) | Microsoft Docs
description: Découvrez la chaîne de fonction XQuery () qui retourne la valeur de son argument représenté sous la forme d’une chaîne.
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
- string function
- fn:string function
ms.assetid: 7baa2959-9340-429b-ad53-3df03d8e13fc
author: rothja
ms.author: jroth
ms.openlocfilehash: 9c334f2f78ae79b9a1af3a268728d6fcc5454f05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351086"
---
# <a name="data-accessor-functions---string-xquery"></a>Fonctions d’accesseur de données : string (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Retourne la valeur de *$arg* représentée sous forme de chaîne.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:string() as xs:string  
fn:string($arg as item()?) as xs:string  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Nœud ou valeur atomique.  
  
## <a name="remarks"></a>Notes  
  
-   Si *$arg* est la séquence vide, la chaîne de longueur nulle est retournée.  
  
-   Si *$arg* est un nœud, la fonction retourne la valeur de chaîne du nœud obtenu à l’aide de l’accesseur de valeur de chaîne. Celui-ci est défini dans les spécifications W3C XQuery 1.0 et XPath 2.0 Data Model.  
  
-   Si *$arg* est une valeur atomique, la fonction retourne la même chaîne que celle retournée par la conversion de l’expression en tant que **XS : String**, *$arg*, sauf indication contraire.  
  
-   Si le type de *$arg* est **XS : anyURI**, l’URI est converti en une chaîne sans caractères spéciaux d’échappement.  
  
-   Cette implémentation, **FN : String ()** sans argument ne peut être utilisée que dans le contexte d’un prédicat dépendant du contexte. Autrement dit, elle ne peut être utilisée qu'à l'intérieur de crochets ([ ]).  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la base de données AdventureWorks.  
  
### <a name="a-using-the-string-function"></a>R. Utilisation de la fonction string  
 La requête suivante récupère le <`Features`> nœud d’élément enfant de l' `ProductDescription` élément <>.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
 /PD:ProductDescription/PD:Features  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Voici le résultat partiel :  
  
```  
<PD:Features xmlns:PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
   These are the product highlights.   
   <p1:Warranty xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <p1:WarrantyPeriod>3 years</p1:WarrantyPeriod>  
    <p1:Description>parts and labor</p1:Description>  
   </p1:Warranty>  
       ...  
</PD:Features>  
```  
  
 Si vous spécifiez la fonction **String ()** , vous recevez la valeur de chaîne du nœud spécifié.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
 string(/PD:ProductDescription[1]/PD:Features[1])  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Le résultat partiel est le suivant.  
  
```  
These are the product highlights.   
3 yearsparts and labor...    
```  
  
### <a name="b-using-the-string-function-on-various-nodes"></a>B. Utilisation de la fonction string sur différents nœuds  
 Dans l'exemple suivant, une instance XML est affectée à une variable de type xml. Les requêtes sont spécifiées pour illustrer le résultat de l’application de **String ()** à différents nœuds.  
  
```  
declare @x xml  
set @x = '<?xml version="1.0" encoding="UTF-8" ?>  
<!--  This is a comment -->  
<root>  
  <a>10</a>  
just text  
  <b attr="x">20</b>  
</root>  
'  
```  
  
 La requête suivante extrait la valeur de chaîne du nœud de document. Cette valeur est le fruit de la concaténation de la valeur de chaîne de tous ses nœuds de texte descendants.  
  
```  
select @x.query('string(/)')  
```  
  
 Voici le résultat obtenu :  
  
```  
This is a comment 10  
just text  
 20  
```  
  
 La requête suivante essaie d'extraire la valeur de chaîne d'un nœud d'instruction de traitement. Le résultat est une séquence vide, car il ne contient pas de nœud de texte.  
  
```  
select @x.query('string(/processing-instruction()[1])')  
```  
  
 La requête suivante extrait la valeur de chaîne du nœud de commentaire et renvoie le nœud de texte.  
  
```  
select @x.query('string(/comment()[1])')  
```  
  
 Voici le résultat obtenu :  
  
```  
This is a comment   
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
