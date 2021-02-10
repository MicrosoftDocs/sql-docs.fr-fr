---
description: Curseurs de jeu de clés
title: Curseurs de jeu de clés | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Keyset cursors [ADO]
- cursors [ADO], Keyset
ms.assetid: 14b51b17-6fd9-4146-af45-ca4b0fe6d48a
author: rothja
ms.author: jroth
ms.openlocfilehash: 4071772212226b2b09886476c6577cac73cd3f79
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037269"
---
# <a name="keyset-cursors"></a>Curseurs de jeu de clés
Le curseur de jeu de clés fournit les fonctionnalités entre un curseur statique et un curseur dynamique dans sa capacité à détecter les modifications. Comme un curseur statique, il ne détecte pas toujours les modifications apportées à l’appartenance et à l’ordre du jeu de résultats. Comme un curseur dynamique, il détecte les modifications apportées aux valeurs des lignes dans le jeu de résultats.  
  
 Les curseurs de jeux de clés sont gérés par un ensemble d’identificateurs uniques (clés) appelé jeu de clés. Les clés sont créées à partir d'un ensemble de colonnes qui identifient uniquement les lignes de l'ensemble de résultats. Le jeu de clés est l’ensemble des valeurs de clés de toutes les lignes retournées par l’instruction de requête.  
  
 Avec les curseurs de jeux de clés, une clé est générée et enregistrée pour chaque ligne du curseur et stockée sur la station de travail cliente ou sur le serveur. Quand vous accédez à chaque ligne, la clé stockée est utilisée pour extraire les valeurs de données actuelles de la source de données. Dans un curseur de jeux de clés, l’appartenance au jeu de résultats est figée quand le jeu de clés est plein. Par la suite, les ajouts ou mises à jour affectant l’appartenance ne font partie du jeu de résultats qu’une fois celui-ci rouvert.  
  
 Les modifications apportées aux valeurs de données (effectuées par le propriétaire du jeu de clés ou par d’autres processus) sont visibles lorsque l’utilisateur fait défiler le jeu de résultats. Les insertions effectuées en dehors du curseur (par d’autres processus) sont visibles uniquement si le curseur est fermé puis rouvert. Les insertions effectuées à partir de l’intérieur du curseur sont visibles à la fin du jeu de résultats.  
  
 Lorsqu’un curseur de jeu de clés tente d’extraire une ligne qui a été supprimée, la ligne apparaît sous la forme d’un « trou » dans le jeu de résultats. La clé pour la ligne existe dans le jeu de clés, mais la ligne n’existe plus dans le jeu de résultats. Si les valeurs de clé d’une ligne sont mises à jour, la ligne est considérée comme supprimée puis insérée, de sorte que ces lignes apparaissent également sous forme de trous dans le jeu de résultats. Bien qu’un curseur piloté par jeu de clés puisse toujours détecter les lignes supprimées par d’autres processus, il peut éventuellement supprimer les clés pour les lignes qu’il supprime lui-même. Les curseurs de jeu de clés qui effectuent cette opération ne peuvent pas détecter leurs propres suppressions, car la preuve a été supprimée.  
  
 Une mise à jour d’une colonne clé fonctionne comme une suppression de l’ancienne clé suivie d’une insertion de la nouvelle clé. La nouvelle valeur de clé n’est pas visible si la mise à jour n’a pas été effectuée par le biais du curseur. Si la mise à jour a été effectuée par le biais du curseur, la nouvelle valeur de clé est visible à la fin du jeu de résultats.  
  
 Il existe une variante des curseurs pilotés par jeu de clés appelés curseurs standard pilotés par jeu de clés. Dans un curseur standard piloté par jeu de clés, l’appartenance des lignes dans le jeu de résultats et l’ordre des lignes sont fixes au moment de l’ouverture du curseur, mais les modifications apportées aux valeurs par le propriétaire du curseur et les modifications validées effectuées par d’autres processus sont visibles. Si une modification ne remplit pas une ligne pour l’appartenance ou affecte l’ordre d’une ligne, la ligne ne disparaît ou ne se déplace pas, sauf si le curseur est fermé et rouvert. Les données insérées n’apparaissent pas, mais les modifications apportées aux données existantes s’affichent au fur et à mesure que les lignes sont extraites.  
  
 Le curseur de jeu de clés est difficile à utiliser correctement, car la sensibilité aux modifications de données dépend de nombreuses circonstances différentes, comme décrit ci-dessus. Toutefois, si votre application n’est pas concernée par les mises à jour simultanées, peut gérer par programmation les clés erronées et doit accéder directement à certaines lignes de clé, le curseur de jeu de clés peut fonctionner pour vous. Utilisez **AdOpenKeyset CursorTypeEnum** pour indiquer que vous souhaitez utiliser un curseur de jeu de clés dans ADO.  
  
## <a name="see-also"></a>Voir aussi  
 [Curseurs avant uniquement](./forward-only-cursors.md)   
 [Curseurs statiques](./static-cursors.md)   
 [Curseurs dynamiques](./dynamic-cursors.md)