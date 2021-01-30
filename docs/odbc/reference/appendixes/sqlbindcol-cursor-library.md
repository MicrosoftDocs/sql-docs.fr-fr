---
description: SQLBindCol (bibliothèque de curseurs)
title: SQLBindCol (bibliothèque de curseurs) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLAllocStmt function [ODBC], Cursor Library
ms.assetid: f4dd546a-0a6c-4397-8ee7-fafa6b9da543
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 64b51f58fd51b9bcad1abd86750b024be131a8a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202949"
---
# <a name="sqlbindcol-cursor-library"></a>SQLBindCol (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Cette rubrique traite de l’utilisation de la fonction **SQLBindCol** dans la bibliothèque de curseurs. Pour obtenir des informations générales sur **SQLBindCol**, consultez la [fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
 Une application alloue une ou plusieurs mémoires tampons pour que la bibliothèque de curseurs retourne l’ensemble de lignes actuel dans. Elle appelle **SQLBindCol** une ou plusieurs fois pour lier ces mémoires tampons au jeu de résultats.  
  
 Une application peut appeler **SQLBindCol** pour relier les colonnes du jeu de résultats après avoir appelé **SQLExtendedFetch**, **SQLFetch** ou **SQLFetchScroll**, tant que le type de données C, la taille de colonne et les chiffres décimaux de la colonne liée restent identiques. L’application n’a pas besoin de fermer le curseur pour relier les colonnes à différentes adresses.  
  
 La bibliothèque de curseurs prend en charge la définition de l’attribut d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR pour utiliser des décalages de liaison. (**SQLBindCol** n’a pas besoin d’être appelé pour que cette reliaison se produise.) Si la bibliothèque de curseurs est utilisée avec un pilote ODBC *3. x* , l’offset de liaison n’est pas utilisé lorsque **SQLFetch** est appelé. L’offset de liaison est utilisé si **SQLFetch** est appelé lorsque la bibliothèque de curseurs est utilisée avec un pilote ODBC *2. x* , car **SQLFetch** est ensuite mappée à **SQLExtendedFetch**.  
  
 La bibliothèque de curseurs prend en charge l’appel à **SQLBindCol** pour lier la colonne de signets.  
  
 Lors de l’utilisation d’un pilote ODBC *2. x* , la bibliothèque de curseurs retourne SQLState HY090 (chaîne ou longueur de mémoire tampon non valide) lorsque **SQLBindCol** est appelé pour définir la longueur de la mémoire tampon d’une colonne de signet sur une valeur différente de 4. Lorsque vous utilisez un pilote ODBC *3. x* , la bibliothèque de curseurs permet à la mémoire tampon d’être de n’importe quelle taille.
