---
description: Données de la colonne
title: Données de la colonne | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- column data [ODBC]
- ODBC cursor library [ODBC], cache
- cursor library [ODBC], cache
- cache [ODBC]
ms.assetid: 0425818c-9469-493f-9e3c-fc03d9411c5c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1287a4deadc6f35bf83f6c42dcebc633f6557fed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212349"
---
# <a name="column-data"></a>Données de la colonne
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 La bibliothèque de curseurs crée une mémoire tampon dans le cache pour chaque tampon de données lié au jeu de résultats avec **SQLBindCol**. Elle utilise les valeurs de ces mémoires tampons pour construire une clause **Where** lorsqu’elle émule une instruction UPDATE ou DELETE positionnée. Il met à jour ces mémoires tampons à partir des tampons de l’ensemble de lignes lorsqu’il extrait des données de la source de données et lorsqu’il exécute des instructions de mise à jour positionnées.  
  
 Lorsque la bibliothèque de curseurs met à jour son cache à partir des mémoires tampons d’ensemble de lignes, elle transfère les données en fonction du type de données C spécifié dans **SQLBindCol**. Par exemple, si le type de données C d’une mémoire tampon d’ensemble de lignes est SQL_C_SLONG, la bibliothèque de curseurs transfère quatre octets de données ; s’il est SQL_C_CHAR et que *BufferLength* est 10, la bibliothèque de curseurs transfère 10 octets de données. La bibliothèque de curseurs n’effectue aucune vérification de type ou conversions sur les données qu’elle transfère.  
  
> [!NOTE]  
>  La bibliothèque de curseurs ne met pas à jour son cache pour une colonne si **StrLen_or_IndPtr* dans la mémoire tampon de l’ensemble de lignes correspondante est SQL_DATA_AT_EXEC ou le résultat de la macro SQL_LEN_DATA_AT_EXEC.  
  
 Lorsqu’il met à jour une colonne, une source de données vide remplit les données de caractères de longueur fixe et remplit les données binaires de longueur fixe de zéro en fonction des besoins. Par exemple, une source de données stocke « Smith » dans une colonne CHAR (10) sous la forme « Smith ». La bibliothèque de curseurs ne remplit pas les tampons ou les données de remplissage à zéro dans les tampons d’ensembles de lignes lorsqu’elle copie ces données dans son cache après l’exécution d’une instruction Update positionnée. Par conséquent, si une application exige que les valeurs du cache de la bibliothèque de curseurs soient complétées par des blancs ou complétées par des zéros, elles doivent remplir les valeurs des tampons de l’ensemble de lignes avant d’exécuter une instruction Update positionnée.
