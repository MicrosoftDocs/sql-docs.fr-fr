---
description: 'Annexe F : Bibliothèque de curseurs ODBC'
title: 'Annexe F : bibliothèque de curseurs ODBC | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], about cursor library
- ODBC cursor library [ODBC]
- cursor library [ODBC], about cursor library
- cursor library [ODBC]
ms.assetid: a03084df-4e48-48ef-917d-4a3fae48a605
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 44fcba1e4c4b2b7f82c99c82399884d7c9bdad85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212531"
---
# <a name="appendix-f-odbc-cursor-library"></a>Annexe F : Bibliothèque de curseurs ODBC
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 La bibliothèque de curseurs ODBC (Odbccr32.dll) prend en charge les curseurs avec défilement de bloc pour tout pilote conforme au niveau de conformité de l’API de niveau 1 et peut être redistribué par les développeurs avec leurs applications ou pilotes. La bibliothèque de curseurs prend également en charge les instructions Update et DELETE positionnées pour les jeux de résultats générés par les instructions **Select** . Bien qu’il prenne en charge uniquement les curseurs statiques et les curseurs avant uniquement, la bibliothèque de curseurs répond aux besoins de nombreuses applications. En outre, elle peut fournir de bonnes performances, en particulier pour les jeux de résultats de petite taille et de taille moyenne, et pour les applications qui n’ont pas une bonne prise en charge de curseur.  
  
 La bibliothèque de curseurs est une bibliothèque de liens dynamiques (DLL) qui se trouve entre le gestionnaire de pilotes et le pilote. Quand une application appelle une fonction, le gestionnaire de pilotes appelle la fonction dans la bibliothèque de curseurs, qui exécute la fonction ou l’appelle dans le pilote spécifié. Pour une connexion donnée, une application spécifie si la bibliothèque de curseurs est toujours utilisée, utilisée si le pilote ne prend pas en charge les curseurs de défilement ou n’est jamais utilisée.  
  
 La bibliothèque de curseurs apparaît en tant que pilote pour le gestionnaire de pilotes. Si la bibliothèque de curseurs réside entre le gestionnaire de pilotes et un pilote ODBC *2. x* , la bibliothèque de curseurs apparaît en tant que pilote ODBC *2. x* . Si la bibliothèque de curseurs réside entre le gestionnaire de pilotes et un pilote ODBC *3. x* , la bibliothèque de curseurs apparaît en tant que pilote ODBC *3. x* . Le comportement présenté par la bibliothèque de curseurs dépend de la version du pilote utilisé, à l’exception des décalages de liaison, qui sont pris en charge pour les pilotes ODBC *2. x* et ODBC *3. x* .  
  
 Pour implémenter des curseurs de bloc dans **SQLFetch** et **SQLFetchScroll**, la bibliothèque de curseurs appelle **SQLFetch** à plusieurs reprises dans le pilote. Pour implémenter le défilement, il met en cache les données qu’il a récupérées en mémoire et dans les fichiers sur disque. Quand une application demande un nouvel ensemble de lignes, la bibliothèque de curseurs la récupère, le cas échéant, à partir du pilote ou du cache.  
  
 Pour implémenter des instructions Update et DELETE positionnées, la bibliothèque de curseurs construit une instruction **Update** ou **Delete** avec une clause **Where** qui spécifie la valeur mise en cache de chaque colonne liée dans la ligne. Lorsqu’il exécute une instruction Update positionnée, la bibliothèque de curseurs met à jour son cache à partir des valeurs des mémoires tampons de l’ensemble de lignes.  
  
 Pour plus d’informations sur la bibliothèque de curseurs ODBC, reportez-vous aux sections suivantes de cette annexe :  
  
-   [Utilisation de la bibliothèque de curseurs ODBC](../../../odbc/reference/appendixes/using-the-odbc-cursor-library.md)  
  
-   [Exécution d’instructions de mise à jour et de suppression positionnées](../../../odbc/reference/appendixes/executing-positioned-update-and-delete-statements.md)  
  
-   [Exemple de code de la bibliothèque de curseurs](../../../odbc/reference/appendixes/cursor-library-code-example.md)  
  
-   [Notes d’implémentation](../../../odbc/reference/appendixes/implementation-notes.md)  
  
-   [Codes d’erreur de la bibliothèque de curseurs ODBC](../../../odbc/reference/appendixes/odbc-cursor-library-error-codes.md)
