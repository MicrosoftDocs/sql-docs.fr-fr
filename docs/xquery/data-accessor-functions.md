---
title: Fonctions d’accesseur de données | Microsoft Docs
description: 'Découvrez comment utiliser les fonctions d’accesseur de données XQuery FN : Data (), fn : String () et Text ().'
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
- data-accessor functions [XQuery]
ms.assetid: 31bad04f-7c74-4773-9f83-612704fdd21c
author: rothja
ms.author: jroth
ms.openlocfilehash: 53ce941c88e2ab551ac0787126e2cf981d353819
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351076"
---
# <a name="data-accessor-functions"></a>Fonctions d'accesseurs de données
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Les rubriques de cette section présentent les fonctions d'accès aux données et les illustrent avec des exemples de code.  
  
## <a name="understanding-fndata-fnstring-and-text"></a>Fonctionnement de fn:data(), fn:string () et text()  
 XQuery a une fonction **FN : Data ()** pour extraire des valeurs scalaires, typées à partir de nœuds, un texte de test de nœud **()** pour retourner des nœuds de texte et la fonction **FN : String ()** qui retourne la valeur de chaîne d’un nœud. Leur utilisation peut prêter à confusion. Vous trouverez ci-après des instructions relatives à leur bonne utilisation dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. L’instance XML \<age> 12 \</age> est utilisée à des fins d’illustration.  
  
-   XML non typé : l'expression de chemin d'accès /age/text() retourne le nœud de texte "12". Les fonctions fn:data(/age) et fn:string(/age) retournent la valeur de chaîne "12".  
  
-   XML typé : l’expression/Age/Text () renvoie une erreur statique pour tout élément typé simple \<age> . En revanche, la fonction fn:data (/age) retourne l'entier 12. La fonction fn:string(/age) produit la chaîne "12".  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Fonction de chaîne &#40;XQuery&#41;](../xquery/data-accessor-functions-string-xquery.md)  
  
-   [Fonction de données &#40;XQuery&#41;](../xquery/data-accessor-functions-data-xquery.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions de chemin d’accès &#40;XQuery&#41;](../xquery/path-expressions-xquery.md)  
  
  
