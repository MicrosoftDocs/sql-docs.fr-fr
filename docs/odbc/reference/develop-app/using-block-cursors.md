---
description: Utilisation de curseurs de bloc
title: Utilisation des curseurs de bloc | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- cursors [ODBC], block
- block cursors [ODBC]
- result sets [ODBC], block cursors
ms.assetid: 2aad7d6b-216e-47e7-b3cb-f95ad096f21a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 67cad7220641e9c3800e89675825cf2c0b1e334d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88482862"
---
# <a name="using-block-cursors"></a>Utilisation de curseurs de bloc
La prise en charge des curseurs de bloc est intégrée à ODBC 3. *x*. **SQLFetch** peut uniquement être utilisée pour les extractions de lignes multiples quand elle est appelée dans ODBC 3. *x*; Si ODBC 2. *x* appelle **SQLFetch**, il n’ouvre qu’un curseur à une seule ligne et avant uniquement. Quand ODBC 3. *x* appelle **SQLFetch** dans une application ODBC 2. *x* , elle retourne une seule ligne, sauf si le pilote prend en charge **SQLExtendedFetch**. Pour plus d’informations, consultez [curseurs de bloc, curseurs avec défilement et compatibilité descendante](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) dans l’annexe G : instructions relatives aux pilotes pour la compatibilité descendante.  
  
 Pour utiliser des curseurs de bloc, l’application définit la taille de l’ensemble de lignes, lie les mémoires tampons d’ensemble de lignes (comme décrit dans la section précédente), définit éventuellement les attributs d’instruction SQL_ATTR_ROWS_FETCHED_PTR et SQL_ATTR_ROW_STATUS_PTR et appelle **SQLFetch** ou **SQLFetchScroll** pour extraire un bloc de lignes. L’application peut modifier la taille de l’ensemble de lignes et lier de nouvelles mémoires tampons d’ensemble de lignes (en appelant **SQLBindCol** ou en spécifiant un décalage de liaison) même après l’extraction des lignes.  
  
 Cette section contient les rubriques suivantes :  
  
-   [Taille des ensembles de lignes](../../../odbc/reference/develop-app/rowset-size.md)  
  
-   [Nombre de lignes extraites et état](../../../odbc/reference/develop-app/number-of-rows-fetched-and-status.md)  
  
-   [Les curseurs de bloc et SQLGetData ; bloquer le bloc](../../../odbc/reference/develop-app/sqlgetdata-and-block-cursors.md)
