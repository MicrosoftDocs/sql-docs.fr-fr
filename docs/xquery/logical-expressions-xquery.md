---
title: Expressions logiques (XQuery) | Microsoft Docs
description: En savoir plus sur les expressions logiques prises en charge dans XQuery.
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
- logical operators [SQL Server], XQuery
- effective Boolean value [XQuery]
- logical expressions [XQuery]
- EBV
- expressions [XQuery], logical
ms.assetid: de94cd2e-2d48-49fb-9ebd-a2d90c79bf62
author: rothja
ms.author: jroth
ms.openlocfilehash: 44f05374320dc2d5c329ad234e62dd0c420bf22c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341849"
---
# <a name="logical-expressions-xquery"></a>Expressions logiques (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  XQuery prend en charge les **opérateurs and et** **or** logiques.  
  
```  
expression1 and expression2  
expression1 or expression2  
```  
  
 Les expressions de test, `expression1,``expression2` , dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peuvent entraîner une séquence vide, une séquence d’un ou de plusieurs nœuds, ou une valeur booléenne unique. D'après le résultat, la valeur booléenne effective est déterminée de la façon suivante :  
  
-   Si les résultats d'une expression de test est une séquence vide, le résultat de l'expression est False.  
  
-   Si les résultats de l'expression de test est une seule valeur booléenne, cette valeur est le résultat de l'expression.  
  
-   Si l'expression de test génère une séquence d'un ou plusieurs nœuds, le résultat de l'expression est True.  
  
-   Dans le cas contraire, une erreur statique est générée.  
  
 L’opérateur **and** et **or** logique est ensuite appliqué aux valeurs booléennes résultantes des expressions avec la sémantique logique standard.  
  
 La requête suivante extrait du catalogue de produits les petites images de l’angle avant, le <`Picture`> élément, pour un modèle de produit spécifique. Notez que pour chaque document de description de produit, le catalogue peut stocker une ou plusieurs illustrations avec différents attributs tels que taille et angle de prise de vue.  
  
```  
SELECT CatalogDescription.query('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     for $F in /PD:ProductDescription/PD:Picture[PD:Size="small"   
                                                 and PD:Angle="front"]  
     return   
         $F   
    ') as Result  
FROM  Production.ProductModel  
where ProductModelID=19  
```  
  
 Voici le résultat obtenu :  
  
```  
<PD:Picture   
  xmlns:PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
  <PD:Angle>front</PD:Angle>  
  <PD:Size>small</PD:Size>  
  <PD:ProductPhotoID>31</PD:ProductPhotoID>  
</PD:Picture>  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions XQuery](../xquery/xquery-expressions.md)  
  
  
