---
description: Curseurs avant uniquement
title: Curseurs de Forward-Only | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], forward-only
- forward-only cursors [ADO]
ms.assetid: 2b1e062f-3294-4a6f-8241-a17045c4df18
author: rothja
ms.author: jroth
ms.openlocfilehash: 0ac51891368a5551c29190de959f8a9559d03b4e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033139"
---
# <a name="forward-only-cursors"></a>Curseurs avant uniquement
Le type de curseur par défaut classique, appelé curseur avant uniquement (ou non défilant), ne peut se déplacer que vers l’avant dans le jeu de résultats. Un curseur avant uniquement ne prend pas en charge le défilement (la possibilité de se déplacer vers l’avant et vers l’arrière dans le jeu de résultats); Il prend uniquement en charge l’extraction de lignes du début jusqu’à la fin du jeu de résultats. Avec certains curseurs avant uniquement (par exemple, avec la bibliothèque de curseurs SQL Server), toutes les instructions d’insertion, de mise à jour et de suppression effectuées par l’utilisateur actuel (ou validées par d’autres utilisateurs) qui affectent les lignes du jeu de résultats sont visibles lors de l’extraction des lignes. Cependant, étant donné que le curseur ne permet pas le défilement arrière, les modifications apportées aux lignes de la base de données après l’extraction d’une ligne ne sont pas visibles par le biais du curseur.  
  
 Une fois les données de la ligne actuelle traitées, le curseur avant uniquement libère les ressources qui ont été utilisées pour stocker ces données. Les curseurs avant uniquement sont dynamiques par défaut, ce qui signifie que toutes les modifications sont détectées lors du traitement de la ligne active. Cela accélère l’ouverture du curseur et permet au jeu de résultats d’afficher les mises à jour apportées aux tables sous-jacentes.  
  
 Alors que les curseurs avant uniquement ne prennent pas en charge le défilement arrière, votre application peut revenir au début du jeu de résultats en fermant et en réouvrant le curseur. Il s’agit d’un moyen efficace de travailler avec de petites quantités de données. En guise d’alternative, votre application peut lire le jeu de résultats une fois, mettre en cache les données localement, puis parcourir le cache de données local.  
  
 Si votre application n’a pas besoin de faire défiler le jeu de résultats, le curseur avant uniquement est la meilleure façon de récupérer des données rapidement avec la surcharge la plus faible. Utilisez **AdOpenForwardOnly CursorTypeEnum** pour indiquer que vous souhaitez utiliser un curseur avant uniquement dans ADO.  
  
## <a name="see-also"></a>Voir aussi  
 [Curseurs statiques](./static-cursors.md)   
 [Curseurs de jeu de clés](./keyset-cursors.md)   
 [Curseurs dynamiques](./dynamic-cursors.md)