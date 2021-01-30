---
description: Traitement des instructions de mise à jour et de suppression positionnées
title: Traitement des instructions Update et DELETE positionnées | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- positioned deletes [ODBC]
- ODBC cursor library [ODBC], statement processing
- cursor library [ODBC], positioned update or delete
- positioned updates [ODBC]
- SQL statements [ODBC], cursor library
- ODBC cursor library [ODBC], positioned update or delete
- cursor library [ODBC], statement processing
ms.assetid: 2975dd97-48e6-4d0a-a9c7-40759a7d94c8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f2d7283524a516b81b6489b543fd3169f7f9aeb4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159536"
---
# <a name="processing-positioned-update-and-delete-statements"></a>Traitement des instructions de mise à jour et de suppression positionnées
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 La bibliothèque de curseurs prend en charge les instructions Update et DELETE positionnées en remplaçant la clause **Where Current of** dans ces instructions par une clause **Where** qui énumère les valeurs stockées dans son cache pour chaque colonne liée. La bibliothèque de curseurs transmet les instructions **Update** et **Delete** nouvellement construites au pilote en vue de leur exécution. Pour les instructions Update positionnées, la bibliothèque de curseurs met à jour son cache à partir des valeurs dans les mémoires tampons d’ensemble de lignes et définit la valeur correspondante dans le tableau d’état de ligne sur SQL_ROW_UPDATED. Pour les instructions DELETE positionnées, elle définit la valeur correspondante dans le tableau d’état de ligne sur SQL_ROW_DELETED.  
  
> [!CAUTION]  
>  La clause **Where** construite par la bibliothèque de curseurs pour identifier la ligne actuelle peut ne pas pouvoir identifier de lignes, identifier une autre ligne ou identifier plusieurs lignes. Pour plus d’informations, consultez [construction d’instructions de recherche](../../../odbc/reference/appendixes/constructing-searched-statements.md), plus loin dans cette annexe.  
  
 Les instructions Update et DELETE positionnées sont soumises aux restrictions suivantes :  
  
-   Les instructions Update et DELETE positionnées ne peuvent être utilisées que dans les cas suivants : lorsqu’une instruction **Select** a généré le jeu de résultats ; Lorsque l’instruction **Select** ne contenait pas de jointure, de clause **Union** ou de clause **Group by** ; et lorsque des colonnes qui utilisaient un alias ou une expression dans la liste de sélection n’étaient pas liées à **SQLBindCol**.  
  
-   Si une application prépare une instruction UPDATE ou DELETE positionnée, elle doit le faire après avoir appelé **SQLFetch** ou **SQLFetchScroll**. Bien que la bibliothèque de curseurs soumette l’instruction au pilote pour la préparation, elle ferme l’instruction et l’exécute directement lorsque l’application appelle **SQLExecute**.  
  
-   Si le pilote ne prend en charge qu’une seule instruction active, la bibliothèque de curseurs extrait le reste du jeu de résultats et récupère ensuite l’ensemble de lignes actuel à partir de son cache avant d’exécuter une instruction UPDATE ou DELETE positionnée. Si l’application appelle ensuite une fonction qui retourne des métadonnées dans un jeu de résultats (par exemple, **SQLNumResultCols** ou **SQLDescribeCol**), la bibliothèque de curseurs retourne une erreur.  
  
-   Si une instruction UPDATE ou DELETE positionnée est exécutée sur une colonne d’une table qui comprend une colonne timestamp qui est automatiquement mise à jour chaque fois qu’une mise à jour est effectuée, toutes les instructions Update ou DELETE positionnées ultérieurement échouent si la colonne timestamp est liée. Cela est dû au fait que l’instruction UPDATE ou DELETE recherchée créée par la bibliothèque de curseurs n’identifie pas correctement la ligne à mettre à jour. La valeur de l’instruction recherchée pour la colonne timestamp ne correspondra pas à la valeur mise à jour automatiquement de la colonne timestamp.
