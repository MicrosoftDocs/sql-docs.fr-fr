---
description: Types de curseurs (ADO)
title: Types de curseurs (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], types
ms.assetid: 7cc01544-e814-403b-bbfe-a2750bf921bd
author: rothja
ms.author: jroth
ms.openlocfilehash: ea996827565f0cc6d593078e7772c336699260bf
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88452711"
---
# <a name="types-of-cursors-ado"></a>Types de curseurs (ADO)
En règle générale, votre application doit utiliser le curseur le plus simple qui fournit l’accès aux données requis. Chaque caractéristique de curseur supplémentaire au-delà des principes de base (avant uniquement, en lecture seule, statique, de défilement, sans mise en mémoire tampon) a un prix en mémoire client, de charge réseau ou de performances. Dans de nombreux cas, les options de curseur par défaut génèrent un curseur plus complexe que celui dont votre application a réellement besoin.  
  
 Le type de curseur choisi dépend de la manière dont votre application utilise le jeu de résultats, ainsi que de plusieurs considérations relatives à la conception, notamment la taille du jeu de résultats, le pourcentage de données susceptibles d’être utilisées, la sensibilité aux modifications de données et les exigences en matière de performances des applications.  
  
 Au plus basique, votre choix de curseur dépend de la nécessité ou non de modifier ou d’afficher simplement les données :  
  
-   Si vous avez simplement besoin de faire défiler un jeu de résultats, mais pas de modifier les données, utilisez un curseur [avant uniquement](../../../ado/guide/data/forward-only-cursors.md) ou [statique](../../../ado/guide/data/static-cursors.md) .  
  
-   Si vous avez un jeu de résultats volumineux et que vous devez sélectionner uniquement quelques lignes, utilisez un curseur de [jeu de clés](../../../ado/guide/data/keyset-cursors.md) .  
  
-   Si vous souhaitez synchroniser un jeu de résultats avec des ajouts, des modifications et des suppressions récents par tous les utilisateurs simultanés, utilisez un curseur [dynamique](../../../ado/guide/data/dynamic-cursors.md) .  
  
 Bien que chaque type de curseur semble être distinct, gardez à l’esprit que ces types de curseur ne sont pas tellement différents que le résultat du chevauchement des caractéristiques et des options.  
  
 Cette section contient les rubriques suivantes :  
  
-   [Curseurs avant uniquement](../../../ado/guide/data/forward-only-cursors.md)  
  
-   [Curseurs statiques](../../../ado/guide/data/static-cursors.md)  
  
-   [Curseurs de jeu de clés](../../../ado/guide/data/keyset-cursors.md)  
  
-   [Curseurs dynamiques](../../../ado/guide/data/dynamic-cursors.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Curseurs avant uniquement](../../../ado/guide/data/forward-only-cursors.md)   
 [Curseurs statiques](../../../ado/guide/data/static-cursors.md)   
 [Curseurs de jeu de clés](../../../ado/guide/data/keyset-cursors.md)   
 [Curseurs dynamiques](../../../ado/guide/data/dynamic-cursors.md)
