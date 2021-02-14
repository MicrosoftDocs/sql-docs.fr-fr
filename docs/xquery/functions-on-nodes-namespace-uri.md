---
title: Fonction namespace-URI (XQuery) | Microsoft Docs
description: Découvrez comment utiliser la fonction namespace-URI dans un XQuery pour retourner l’URI d’espace de noms d’un QName spécifié.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:namespace-uri function
- namespace-uri function
ms.assetid: 9b48d216-26c8-431d-9ab4-20ab187917f4
author: rothja
ms.author: jroth
ms.openlocfilehash: 4a9734dd4efd2f4dcedd539218a0a7d989211749
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353283"
---
# <a name="functions-on-nodes---namespace-uri"></a>Fonctions sur les nœuds : namespace-uri
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Retourne l’URI d’espace de noms du QName spécifié dans *$arg* en tant que XS : String.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
fn:namespace-uri() as xs:string  
fn:namespace-uri($arg as node()?) as xs:string  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Nom de nœud dont la partie URI d'espace de noms doit être extraite.  
  
## <a name="remarks"></a>Notes  
  
-   Si l'argument est omis, la valeur par défaut est le nœud de contexte.  
  
-   Dans SQL Server, **FN : namespace-URI ()** sans argument ne peut être utilisé que dans le contexte d’un prédicat dépendant du contexte. Autrement dit, elle ne peut être utilisée qu'à l'intérieur de crochets ([ ]).  
  
-   Si *$arg* est la séquence vide, la chaîne de longueur nulle est retournée.  
  
-   Si *$arg* est un nœud d’élément ou d’attribut dont expanded-QName n’est pas dans un espace de noms, la fonction retourne la chaîne de longueur nulle.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la base de données AdventureWorks.  
  
### <a name="a-retrieve-namespace-uri-of-a-specific-node"></a>R. Extraction de l'URI d'espace de noms d'un nœud spécifique  
 La requête suivante est spécifiée sur une instance XML non typée. L'expression de requête, `namespace-uri(/ROOT[1])`, extrait la partie URI d'espace de noms du nœud spécifié.  
  
```  
set @x='<ROOT><a>111</a></ROOT>'  
SELECT @x.query('namespace-uri(/ROOT[1])')  
```  
  
 Étant donné que le QName spécifié ne possède pas la partie URI d'espace de noms mais uniquement la partie nom local, le résultat est une chaîne nulle.  
  
 La requête suivante est spécifiée par rapport à la colonne **XML** typée d’instructions. L’expression, `namespace-uri(/AWMI:root[1]/AWMI:Location[1])` , retourne l’URI de l’espace de noms de la première <`Location`> élément enfant de l' `root` élément> <.  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     namespace-uri(/AWMI:root[1]/AWMI:Location[1])') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Voici le résultat obtenu :  
  
```  
https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions  
```  
  
### <a name="b-using-namespace-uri-without-argument-in-a-predicate"></a>B. Utilisation de namespace-uri() sans argument dans un prédicat  
 La requête suivante porte sur la colonne xml typée CatalogDescription. L'expression renvoie tous les nœuds d'élément dont l'URI d'espace de noms est `https://www.adventure-works.com/schemas/OtherFeatures`. La fonction namespace-**URI ()** est spécifiée sans argument et utilise le nœud de contexte.  
  
```  
SELECT CatalogDescription.query('  
declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
   /p1:ProductDescription//*[namespace-uri() = "https://www.adventure-works.com/schemas/OtherFeatures"]  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Résultat partiel :  
  
```  
<p1:wheel xmlns:p1="https://www.adventure-works.com/schemas/OtherFeatures">High performance wheels.</p1:wheel>  
<p2:saddle xmlns:p2="https://www.adventure-works.com/schemas/OtherFeatures">  
  <p3:i xmlns:p3="http://www.w3.org/1999/xhtml">Anatomic design</p3:i> and made from durable leather for a full-day of riding in comfort.</p2:saddle>  
...  
```  
  
 Vous pouvez remplacer l'URI d'espace de noms dans la requête précédente par `https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain`. Vous recevrez ensuite tous les enfants de nœud d’élément de l' `ProductDescription` élément <> dont la partie URI d’espace de noms du QName développé est `https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain` .  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   La fonction **namespace-URI ()** retourne des instances de type xs : String au lieu de XS : anyURI.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions sur les nœuds](./xquery-functions-against-the-xml-data-type.md)   
 [Fonction de nom local &#40;XQuery&#41;](../xquery/functions-on-nodes-local-name.md)  
  
