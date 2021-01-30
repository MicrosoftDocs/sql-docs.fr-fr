---
description: État des lignes
title: État de la ligne | Microsoft Docs
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
- row status [ODBC]
- cache [ODBC]
ms.assetid: 0f0b1fb6-f697-4ced-811c-2908e210bc71
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 491e0b420e31f1fe8e66c73f9ab8d89cb1e7e1cd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187154"
---
# <a name="row-status"></a>État des lignes
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 La bibliothèque de curseurs crée une mémoire tampon dans le cache pour l’état de la ligne. La bibliothèque de curseurs récupère des valeurs pour le tableau d’état de ligne (spécifié avec l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR) à partir de cette mémoire tampon. Pour chaque ligne, la bibliothèque de curseurs définit ce tampon sur :  
  
-   SQL_ROW_DELETED lorsqu’il exécute une instruction DELETE positionnée sur la ligne.  
  
-   SQL_ROW_ERROR lorsqu’il rencontre une erreur lors de l’extraction de la ligne de la source de données avec **SQLFetch**.  
  
-   SQL_ROW_SUCCESS lorsqu’il extrait avec succès la ligne de la source de données avec **SQLFetch**.  
  
-   SQL_ROW_UPDATED lorsqu’il exécute une instruction Update positionnée sur la ligne.
