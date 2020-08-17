---
description: Commentaires (syntaxe MDX)
title: Commentaires (syntaxe MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 8fa1312e361bf0ef673254c0eb804ed7b8b7ca7b
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88387674"
---
# <a name="comments-mdx-syntax"></a>Commentaires (syntaxe MDX)


  Les commentaires sont des chaînes de texte non exécutables figurant dans un code de programmation (les commentaires sont également appelés remarques). Vous pouvez utiliser des commentaires pour documenter du code ou désactiver temporairement des parties d'instructions et de scripts MDX (Multidimensional Expressions) en cours d'analyse. En utilisant des commentaires pour documenter le code, vous pouvez faciliter la maintenance future du code de programme. Les commentaires servent souvent à enregistrer le nom du programme, le nom de l'auteur et les dates des principales mises à jour. Vous pouvez également utiliser des commentaires pour décrire des calculs compliqués ou expliquer une méthode de programmation.  
  
 Les commentaires de la syntaxe MDX respectent les principes suivants :  
  
-   Tous les caractères alphanumériques ou les symboles peuvent être utilisés dans un commentaire.  Tous les caractères d’un commentaire sont ignorés.  
  
-   La longueur d'un commentaire dans une instruction ou un script n'est pas limitée. Un commentaire peut être constitué d’une ou de plusieurs lignes.  
  
 La syntaxe MDX prend en charge trois types de caractères de commentaire :  
  
 // (doubles barres obliques)  
 Ces caractères de commentaire peuvent être utilisés directement dans une ligne de code ou en début de ligne. Tout ce qui se trouve entre les doubles barres obliques et la fin de ligne fait partie du commentaire. Pour un commentaire de plusieurs lignes, les doubles barres obliques doivent apparaître au début de chaque ligne. Pour plus d’informations, consultez [&#40;Comment&#41; &#40;&#41;MDX ](../mdx/comment-mdx-double-slash.md).  
  
 -- (doubles tirets)  
 Ces caractères de commentaire peuvent être utilisés directement dans une ligne de code ou en début de ligne. Tout ce qui se trouve entre les doubles tirets et la fin de ligne fait partie du commentaire. Pour un commentaire de plusieurs lignes, les doubles tirets doivent apparaître au début de chaque ligne. Pour plus d’informations, consultez [--&#40;Comment&#41; &#40;&#41;MDX ](../mdx/comment-mdx-operator-reference.md).  
  
 /* ... \* /(paires de caractères barre oblique-astérisque)  
 Ces caractères de commentaire peuvent être utilisés directement dans une ligne de code devant être exécuté, en début de ligne ou même dans du code exécutable. Tout, de la paire de commentaires ouverte (/ \* ) à la paire de commentaires de fermeture ( \* /), est considéré comme faisant partie du commentaire. Pour un commentaire de plusieurs lignes, la paire de caractères de commentaire ouvert (/ \* ) doit démarrer le commentaire, et la paire de caractères de fermeture de commentaire ( \* /) doit terminer le commentaire. Aucun autre caractère de commentaire ne peut apparaître dans les lignes de commentaire. Pour plus d’informations, consultez [/*. \* .. /(Commentaire)](../mdx/comment-mdx.md).  
  
## <a name="example"></a>Exemple  
 La requête suivante offre des exemples des trois types de commentaire :  
  
 `//An example of a comment using the double-forward slash`  
  
 `--An example of a comment using the double-hypen`  
  
 `/*An example of a`  
  
 `multi-line`  
  
 `comment*/`  
  
 `SELECT`  
  
 `{[Measures].[Internet Sales Amount]}`  
  
 `ON Columns,`  
  
 `[Date].[Calendar].MEMBERS`  
  
 `ON Rows`  
  
 `FROM [Adventure Works]`  
  
## <a name="see-also"></a>Voir aussi  
 [Éléments de la syntaxe MDX &#40;MDX&#41;](../mdx/mdx-syntax-elements-mdx.md)  
  
  
