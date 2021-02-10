---
description: Curseurs statiques
title: Curseurs statiques | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], static
- static cursors [ADO]
ms.assetid: cce93ace-c4ed-4c6c-940c-28a50ff2fd12
author: rothja
ms.author: jroth
ms.openlocfilehash: a6b6ef12f2da2416e5f2c3c6e2e6c0c94e2c5327
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036979"
---
# <a name="static-cursors"></a>Curseurs statiques
Le curseur statique affiche toujours l’ensemble de résultats tel qu’il était au moment où le curseur a été ouvert pour la première fois. En fonction de l’implémentation, les curseurs statiques sont en lecture seule ou en lecture/écriture, et permettent de faire défiler vers l’avant et vers l’arrière. Le curseur statique ne détecte généralement pas les modifications apportées à l’appartenance, à l’ordre ou aux valeurs du jeu de résultats après l’ouverture du curseur. Les curseurs statiques peuvent détecter leurs propres mises à jour, suppressions et insertions, bien qu’ils ne soient pas obligés de le faire.  
  
 Les curseurs statiques ne détectent jamais d’autres mises à jour, suppressions et insertions. Par exemple, supposez qu’un curseur statique extrait une ligne et qu’une autre application met ensuite à jour cette ligne. Si l’application réextrait la ligne à partir du curseur statique, les valeurs qu’elle voit sont inchangées, malgré les modifications apportées par l’autre application. Tous les types de défilement sont pris en charge, mais les fournisseurs peuvent ou non prendre en charge les signets.  
  
 Si votre application n’a pas besoin de détecter les modifications de données et nécessite un défilement, le curseur statique est le meilleur choix. Utilisez **AdOpenStatic CursorTypeEnum** pour indiquer que vous souhaitez utiliser un curseur statique dans ADO.  
  
## <a name="see-also"></a>Voir aussi  
 [Curseurs avant uniquement](../../../ado/guide/data/forward-only-cursors.md)   
 [Curseurs de jeu de clés](../../../ado/guide/data/keyset-cursors.md)   
 [Curseurs dynamiques](../../../ado/guide/data/dynamic-cursors.md)
