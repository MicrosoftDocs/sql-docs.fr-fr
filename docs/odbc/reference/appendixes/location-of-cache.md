---
description: Emplacement du cache
title: Emplacement du cache | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], cache
- cursor library [ODBC], cache
- cache [ODBC]
ms.assetid: 240d6162-4da6-4b1f-96c7-f379f4ecb16f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b500aa9b7955c91c6ca39b3f7f830b2022dc4a57
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184359"
---
# <a name="location-of-cache"></a>Emplacement du cache
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 La bibliothèque de curseurs met en cache les données en mémoire et dans les fichiers temporaires Windows®. Cela limite la taille du jeu de résultats que la bibliothèque de curseurs peut gérer uniquement par l’espace disque disponible. Un fichier temporaire est utilisé lorsque les données à mettre en cache franchissent les limites du segment si elles sont insérées à la fin du cache de la bibliothèque de curseurs. Au lieu de cela, les données à mettre en cache sont ajoutées à la place du dernier bloc de données enregistré dans le cache. Le dernier bloc de données enregistré est enregistré dans un fichier temporaire. Si la bibliothèque de curseurs se termine anormalement, par exemple en cas de panne de courant, elle peut conserver les fichiers temporaires Windows sur le disque. Celles-ci sont nommées ~ CTT *nnnn*. tmp et sont créées dans le répertoire actif.  
  
> [!NOTE]  
>  Si la bibliothèque de curseurs dans Microsoft® Windows NT®/Windows2000 tente de mettre en cache des données dans un fichier temporaire sur le répertoire actif pendant que l’application s’exécute à partir d’un partage en lecture seule ou d’un disque compact (tel qu’un exemple bibliothèque MFC (Microsoft Foundation Class)), SQLSTATE HY000 (général Error-Unable pour créer une mémoire tampon de fichier) est retourné.
