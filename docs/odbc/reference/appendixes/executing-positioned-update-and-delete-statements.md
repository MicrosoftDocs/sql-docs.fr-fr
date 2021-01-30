---
description: Exécution d’instructions de mise à jour et de suppression positionnées
title: Exécution d’instructions Update et DELETE positionnées | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- positioned deletes [ODBC]
- cursor library [ODBC], positioned update or delete
- positioned updates [ODBC]
- ODBC cursor library [ODBC], positioned update or delete
ms.assetid: 1d64f309-2a6e-4ad1-a6b5-e81145549c56
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d10b2e5c625a222e8e09a5e783291ce613a492b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194823"
---
# <a name="executing-positioned-update-and-delete-statements"></a>Exécution d’instructions de mise à jour et de suppression positionnées
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Une fois qu’une application a extrait un bloc de données avec **SQLFetchScroll**, elle peut mettre à jour ou supprimer les données du bloc. Pour exécuter une mise à jour ou une suppression positionnée, l’application :  
  
1.  Appelle **SQLSetPos** pour positionner le curseur sur la ligne à mettre à jour ou à supprimer.  
  
2.  Construit une instruction UPDATE ou DELETE positionnée avec la syntaxe suivante :  
  
     **Mettre à jour** *le nom de la table*  
  
     **Set** *Column-identifier* **=** {*expression* &#124; **null**}  
  
     [**,** *Column-identifier* **=** {*expression* &#124; **null**}]  
  
     **Où Current de** *Cursor-Name*  
  
     **Supprimer de** la *table-nom* **où Current de** *Cursor-Name*  
  
     Le moyen le plus simple de construire la clause **Set** dans une instruction de mise à jour positionnée consiste à utiliser des marqueurs de paramètres pour chaque colonne à mettre à jour et à utiliser **SQLBindParameter** pour les lier aux mémoires tampons de l’ensemble de lignes à mettre à jour. Dans ce cas, le type de données C du paramètre sera le même que le type de données C de la mémoire tampon de l’ensemble de lignes.  
  
3.  Met à jour les mémoires tampons d’ensemble de lignes pour la ligne actuelle si elle exécute une instruction Update positionnée. Après l’exécution réussie d’une instruction Update positionnée, la bibliothèque de curseurs copie les valeurs de chaque colonne de la ligne actuelle dans son cache.  
  
    > [!CAUTION]  
    >  Si l’application ne met pas correctement à jour les mémoires tampons d’ensembles de lignes avant d’exécuter une instruction de mise à jour positionnée, les données du cache sont incorrectes après l’exécution de l’instruction.  
  
4.  Exécute l’instruction UPDATE ou DELETE positionnée à l’aide d’une instruction différente de celle de l’instruction associée au curseur.  
  
    > [!CAUTION]  
    >  La clause **Where** construite par la bibliothèque de curseurs pour identifier la ligne actuelle peut ne pas pouvoir identifier de lignes, identifier une autre ligne ou identifier plusieurs lignes. Pour plus d’informations, consultez [construction d’instructions recherchées](../../../odbc/reference/appendixes/constructing-searched-statements.md).  
  
 Toutes les instructions Update et DELETE positionnées requièrent un nom de curseur. Pour spécifier le nom du curseur, une application appelle **SQLSetCursorName** avant l’ouverture du curseur. Pour utiliser le nom de curseur généré par le pilote, une application appelle **SQLGetCursorName** après l’ouverture du curseur.  
  
 Une fois que la bibliothèque de curseurs a exécuté une instruction UPDATE ou DELETE positionnée, le tableau d’État, les mémoires tampons d’ensemble de lignes et le cache gérés par la bibliothèque de curseurs contiennent les valeurs indiquées dans le tableau suivant.  
  
|Instruction utilisée|Valeur dans le tableau d’état de ligne|Valeurs dans<br /><br /> mémoires tampons d’ensemble de lignes|Valeurs dans<br /><br /> mémoires tampons de cache|  
|--------------------|-------------------------------|----------------------------------|---------------------------------|  
|Mise à jour positionnée|SQL_ROW_UPDATED|Nouvelles valeurs [1]|Nouvelles valeurs [1]|  
|Suppression positionnée|SQL_ROW_DELETED|Anciennes valeurs|Anciennes valeurs|  
  
 [1] l’application doit mettre à jour les valeurs dans les mémoires tampons de l’ensemble de lignes avant d’exécuter l’instruction de mise à jour positionnée. après l’exécution de l’instruction Update positionnée, la bibliothèque de curseurs copie les valeurs des tampons de l’ensemble de lignes dans son cache.
