---
description: Construction d’instructions de recherche
title: Construction d’instructions recherchées | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- searched statements [ODBC]
- ODBC cursor library [ODBC], statement processing
- ODBC cursor library [ODBC], searched statements
- SQL statements [ODBC], cursor library
- cursor library [ODBC], statement processing
- cursor library [ODBC], searched statements
- SQL statements [ODBC], searched statements
ms.assetid: e429254c-c43f-4fbf-98b2-5f1ed53501ff
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a7928e3c730a7431d7f94a90a85073a3c818ae31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165399"
---
# <a name="constructing-searched-statements"></a>Construction d’instructions de recherche
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Pour prendre en charge les instructions Update et DELETE positionnées, la bibliothèque de curseurs construit une instruction **Update** ou **Delete** recherchée à partir de l’instruction positionnée. Pour prendre en charge les appels à **SQLGetData** dans un bloc de données, la bibliothèque de curseurs construit une instruction **Select** recherchée pour créer un jeu de résultats contenant la ligne de données actuelle. Dans chacune de ces instructions, la clause **Where** énumère les valeurs stockées dans le cache pour chaque colonne liée qui retourne SQL_PRED_SEARCHABLE ou SQL_PRED_BASIC pour l’identificateur de champ SQL_DESC_SEARCHABLE dans **SQLColAttribute**.  
  
> [!CAUTION]  
>  La clause **Where** construite par la bibliothèque de curseurs pour identifier la ligne actuelle peut ne pas pouvoir identifier de lignes, identifier une autre ligne ou identifier plusieurs lignes.  
  
 Si une instruction UPDATE ou DELETE positionnée affecte plus d’une ligne, la bibliothèque de curseurs met à jour le tableau d’état de ligne uniquement pour la ligne sur laquelle le curseur est positionné et retourne SQL_SUCCESS_WITH_INFO et SQLSTATE 01001 (conflit d’opération de curseur). Si l’instruction n’identifie aucune ligne, la bibliothèque de curseurs ne met pas à jour le tableau d’état de ligne et retourne SQL_SUCCESS_WITH_INFO et SQLSTATE 01001 (conflit d’opération de curseur). Une application peut appeler **SQLRowCount** pour déterminer le nombre de lignes qui ont été mises à jour ou supprimées.  
  
 Si la clause **Select** utilisée pour positionner le curseur pour un appel à **SQLGetData** identifie plusieurs lignes, **SQLGetData** n’a pas la garantie de renvoyer les données correctes. Si aucune ligne n’est identifiée, **SQLGetData** retourne SQL_NO_DATA.  
  
 Si une application est conforme aux indications suivantes, la clause **Where** construite par la bibliothèque de curseurs doit identifier de façon unique la ligne actuelle, sauf si cela est impossible, par exemple lorsque la source de données contient des lignes en double.  
  
-   **Liez les colonnes qui identifient la ligne de façon unique.** Si les colonnes dépendantes n’identifient pas la ligne de manière unique, la clause **Where** construite par la bibliothèque de curseurs peut identifier plusieurs lignes. Dans une instruction UPDATE ou DELETE positionnée, une telle clause peut entraîner la mise à jour ou la suppression de plusieurs lignes. Dans un appel à **SQLGetData**, une telle clause peut amener le pilote à retourner des données pour une ligne incorrecte. La liaison de toutes les colonnes dans une clé unique garantit que chaque ligne est identifiée de manière unique.  
  
-   **Allouez des tampons de données suffisamment grands pour qu’aucune troncation ne se produise.** Le cache de la bibliothèque de curseurs est une copie des valeurs dans les mémoires tampons d’ensemble de lignes liées au jeu de résultats avec **SQLBindCol**. Si les données sont tronquées lorsqu’elles sont placées dans ces mémoires tampons, elles seront également tronquées dans le cache. Une clause **Where** construite à partir de valeurs tronquées peut ne pas identifier correctement la ligne sous-jacente dans la source de données.  
  
-   **Spécifiez des mémoires tampons de longueur non null pour les données binaires C.** La bibliothèque de curseurs alloue des mémoires tampons de longueur dans son cache uniquement si l’argument *StrLen_or_IndPtr* dans **SQLBindCol** n’a pas la valeur null. Lorsque l’argument *TargetType* est SQL_C_BINARY, la bibliothèque de curseurs requiert la longueur des données binaires pour construire une clause **Where** à partir des données. S’il n’existe pas de mémoire tampon de longueur pour une colonne SQL_C_BINARY et si l’application appelle **SQLGetData** ou tente d’exécuter une instruction UPDATE ou DELETE positionnée, la bibliothèque de curseurs retourne SQL_ERROR et SQLSTATE SL014 (une requête positionnée a été émise et tous les champs de nombre de colonnes n’ont pas été mis en mémoire tampon).  
  
-   **Spécifiez des mémoires tampons de longueur non null pour les colonnes Nullable.** La bibliothèque de curseurs alloue des mémoires tampons de longueur dans son cache uniquement si l’argument *StrLen_or_IndPtr* dans **SQLBindCol** n’a pas la valeur null. Étant donné que SQL_NULL_DATA est stockée dans la mémoire tampon de longueur, la bibliothèque de curseurs suppose que toute colonne pour laquelle aucune mémoire tampon de longueur n’est spécifiée n’accepte pas les valeurs NULL. Si aucune colonne de longueur n’est spécifiée pour une colonne Nullable, la bibliothèque de curseurs construit une clause **Where** qui utilise la valeur de données pour la colonne. Cette clause n’identifie pas correctement la ligne.
