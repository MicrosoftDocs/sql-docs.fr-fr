---
description: Mots clés réservés (syntaxe MDX)
title: Mots clés réservés (syntaxe MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 630f004a230d473ef5aab6ea230dc2d3903ce0e7
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88341345"
---
# <a name="reserved-keywords-mdx-syntax"></a>Mots clés réservés (syntaxe MDX)


  Analysis Services réserve certains mots clés pour son usage exclusif. Pour obtenir la liste des mots clés réservés, consultez [Mots réservés MDX](../mdx/mdx-reserved-words.md).  
  
 Les mots clés respectent les principes suivants :  
  
-   Vous ne pouvez inclure des mots clés réservés dans une instruction MDX (Multidimensional Expressions) qu'à l'emplacement défini par [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].  
  
-   Aucun objet de la base de données ne peut recevoir un nom qui correspond à un mot clé réservé. Si le cas se présente, il faut toujours faire référence à l'objet en délimitant les identificateurs. Même si cette méthode n'autorise pas les noms d'objets en tant que mots réservés, l'utilisation de mots clés pour nommer des objets doit être évitée.  
  
-   Utilisez une convention d'attribution de noms qui permet d'éviter l'utilisation de mots clés réservés. Les consonnes ou les voyelles peuvent être supprimées si un nom d’objet doit ressembler à un mot clé réservé.  
  
## <a name="see-also"></a>Voir aussi  
 [Éléments de la syntaxe MDX &#40;MDX&#41;](../mdx/mdx-syntax-elements-mdx.md)  
  
  
