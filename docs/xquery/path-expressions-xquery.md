---
title: Expressions de chemin d’accès (XQuery) | Microsoft Docs
description: Découvrez comment les expressions de chemin d’accès XQuery localisent les nœuds, tels que les nœuds d’élément, d’attribut et de texte, dans un document.
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
- path expressions [XQuery]
- expressions [XQuery], path
ms.assetid: b93fa36c-bf69-46b9-b137-f597d66fd0c0
author: rothja
ms.author: jroth
ms.openlocfilehash: 6d69a916d19c519e1b313a7244a6a07feba36a67
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339996"
---
# <a name="path-expressions-xquery"></a>Expressions de chemin d'accès (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Les expressions de chemin d'accès recherchent des nœuds, tels que des nœuds d'élément, d'attribut et de texte, dans un document. Le résultat d'une expression de chemin d'accès suit toujours l'ordre des documents et la séquence du résultat est dépourvue de nœuds dupliqués. Lorsque vous spécifiez un chemin d'accès, vous pouvez utiliser une syntaxe non abrégée ou une syntaxe abrégée. Les informations suivantes portent sur la syntaxe non abrégée. La syntaxe abrégée est décrite plus loin dans cette rubrique.  
  
> [!NOTE]  
>  Étant donné que les exemples de requêtes de cette rubrique sont spécifiés par rapport aux colonnes de type **XML** **CatalogDescription** et **instructions**, dans la table **ProductModel** , vous devez vous familiariser avec le contenu et la structure des documents XML stockés dans ces colonnes.  
  
 Une expression de chemin d'accès peut être relative ou absolue. Les paragraphes suivants décrivent ces deux types d'expression :  
  
-   Une expression de chemin d'accès relative est composée d'une étape, ou de plusieurs étapes séparées par une ou deux barres obliques (/ ou //). Par exemple, `child::Features` est une expression de chemin d'accès relative, où `Child` fait uniquement référence aux nœuds enfants du nœud de contexte. Il s'agit du nœud en cours de traitement. L’expression récupère les \<Features> enfants du nœud d’élément du nœud de contexte.  
  
-   Une expression de chemin d'accès absolue commence par une ou deux barres obliques (/ ou //), suivies d'un chemin d'accès relatif facultatif. Par exemple, la barre oblique initiale de l'expression `/child::ProductDescription` indique qu'il s'agit d'une expression de chemin d'accès absolue. Comme une marque de barre oblique au début d’une expression retourne le nœud racine du document du nœud de contexte, l’expression retourne tous les \<ProductDescription> enfants du nœud d’élément de la racine du document.  
  
     Si un chemin d'accès absolu commence par une barre oblique unique, il peut être, ou ne pas être, suivi d'un chemin d'accès relatif. Si vous spécifiez une seule barre oblique, l'expression renvoie le nœud racine du nœud de contexte. Dans le cas d'un type de données XML, il s'agit de son nœud de document.  
  
 En règle générale, une expression de chemin d'accès est composée d'étapes. Par exemple, l’expression de chemin d’accès absolu, `/child::ProductDescription/child::Summary` , contient deux étapes séparées par une barre oblique.  
  
-   La première étape récupère les \<ProductDescription> enfants du nœud d’élément de la racine du document.  
  
-   La deuxième étape récupère les \<Summary> enfants de nœud d’élément pour chaque \<ProductDescription> nœud d’élément récupéré, qui à son tour devient le nœud de contexte.  
  
 Une étape dans une expression de chemin d'accès peut être une étape d'axe ou une étape générale.  
  
## <a name="axis-step"></a>Étape d'axe  
 Une étape d'axe dans une expression de chemin d'accès est composée des parties ci-après.  
  
 [zone](../xquery/path-expressions-specifying-axis.md)  
 Définit le sens du déplacement. Étape d'axe dans une expression de chemin d'accès qui commence au nœud de contexte et navigue jusqu'aux nœuds accessibles dans le sens spécifié par l'axe.  
  
 [test de nœud](../xquery/path-expressions-specifying-node-test.md)  
 Spécifie les noms de nœud ou de type de nœud à sélectionner.  
  
 Zéro ou plusieurs prédicats facultatifs  
 Filtre les nœuds en sélectionnant certains d'entre eux et en rejetant les autres.  
  
 Les exemples suivants utilisent un **axisstep** dans les expressions de chemin d’accès :  
  
-   L'expression de chemin d'accès absolue `/child::ProductDescription` contient une seule étape. Elle spécifie un axe (`child`) et un test de nœud (`ProductDescription`).  
  
-   L'expression de chemin d'accès relative `child::ProductDescription/child::Features` contient deux étapes séparées par une barre oblique. Les deux étapes spécifient un axe enfant. ProductDescription et Features sont des tests de nœud.  
  
-   L’expression de chemin d’accès relative, `child::root/child::Location[attribute::LocationID=10]` , contient deux étapes séparées par une barre oblique. La première étape spécifie un axe (`child`) et un test de nœud (`root`). La seconde étape spécifie l'ensemble des trois composants d'une étape d'axe : un axe (enfant), un test de nœud (`Location`) et un prédicat (`[attribute::LocationID=10]`).  
  
 Pour plus d’informations sur les composants d’une étape d’axe, consultez Spécification de l' [axe dans une étape d’expression de chemin d'](../xquery/path-expressions-specifying-axis.md)accès, [spécification d’un test de nœud dans une étape d’expression de chemin d'](../xquery/path-expressions-specifying-node-test.md)accès et [spécification de prédicats dans une étape d’expression de chemin d’accès](../xquery/path-expressions-specifying-predicates.md).  
  
## <a name="general-step"></a>Étape générale  
 Une étape générale est simplement une expression dont la valeur doit correspondre à une séquence de nœuds.  
  
 Dans SQL Server, l'implémentation de XQuery prend en charge une étape générale en tant que première étape d'une expression de chemin d'accès. Les exemples ci-après illustrent l'utilisation d'étapes générales dans les expressions de chemin d'accès.  
  
```  
(/a, /b)/c  
id(/a/b)  
```  
  
 Pour plus d’informations sur la fonction ID, consultez la [fonction id &#40;XQuery&#41;](../xquery/functions-on-sequences-id.md).  
  
## <a name="in-this-section"></a>Dans cette section  
 [Définition d'une étape d'axe dans une expression de chemin d'accès](../xquery/path-expressions-specifying-axis.md)  
 Décrit l'utilisation de l'étape d'axe dans une expression de chemin d'accès.  
  
 [Spécification d'un test de nœud dans une étape d'expression du chemin d'accès](../xquery/path-expressions-specifying-node-test.md)  
 Décrit l'utilisation de tests de nœud dans une expression de chemin d'accès.  
  
 [Spécification de prédicats dans une étape d'expression de chemin d'accès](../xquery/path-expressions-specifying-predicates.md)  
 Décrit l'utilisation de prédicats dans une expression de chemin d'accès.  
  
 [Utilisation de la syntaxe abrégée dans une expression de chemin d'accès](../xquery/path-expressions-using-abbreviated-syntax.md)  
 Décrit l'utilisation de la syntaxe abrégée dans une expression de chemin d'accès.  
  
  
