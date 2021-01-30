---
description: Utilisation de la bibliothèque de curseurs ODBC
title: Utilisation de la bibliothèque de curseurs ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], using cursor library
- cursor library [ODBC], using cursor library
ms.assetid: 9653f2f8-ccfc-4220-99ef-601dc0fa641c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1658d3c628506f1b5c53a5c9271a10f96fa2ef44
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158184"
---
# <a name="using-the-odbc-cursor-library"></a>Utilisation de la bibliothèque de curseurs ODBC
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Pour utiliser la bibliothèque de curseurs ODBC, une application :  
  
1.  Appelle **SQLSetConnectAttr** avec un *attribut* de SQL_ATTR_ODBC_CURSORS pour spécifier comment la bibliothèque de curseurs doit être utilisée avec une connexion particulière. La bibliothèque de curseurs peut être toujours utilisée (SQL_CUR_USE_ODBC), utilisée uniquement si le pilote ne prend pas en charge les curseurs à défilement (SQL_CUR_USE_IF_NEEDED) ou n’est jamais utilisé (SQL_CUR_USE_DRIVER).  
  
2.  Appelle **SQLConnect**, **SQLDriverConnect** ou **SQLBrowseConnect** pour se connecter à la source de données.  
  
3.  Appelle **SQLSetStmtAttr** pour spécifier le type de curseur (SQL_ATTR_CURSOR_TYPE), la concurrence (SQL_ATTR_CONCURRENCY) et la taille de l’ensemble de lignes (SQL_ATTR_ROW_ARRAY_SIZE). La bibliothèque de curseurs prend en charge les curseurs avant uniquement et statiques. Les curseurs avant uniquement doivent être en lecture seule, tandis que les curseurs statiques peuvent être en lecture seule ou peuvent utiliser le contrôle d’accès concurrentiel optimiste en comparant les valeurs.  
  
4.  Alloue une ou plusieurs mémoires tampons d’ensemble de lignes et appelle **SQLBindCol** une ou plusieurs fois pour lier ces mémoires tampons aux colonnes du jeu de résultats.  
  
5.  Génère un jeu de résultats en exécutant une instruction **Select** ou une procédure, ou en appelant une fonction de catalogue. Si l’application exécute des instructions Update positionnées, elle doit exécuter une instruction **Select for Update** pour générer le jeu de résultats.  
  
6.  Appelle **SQLFetch** ou **SQLFetchScroll** une ou plusieurs fois pour faire défiler le jeu de résultats.  
  
 L’application peut modifier les valeurs des données dans les mémoires tampons de l’ensemble de lignes. Pour actualiser les tampons de l’ensemble de lignes avec les données du cache de la bibliothèque de curseurs, une application appelle **SQLFetchScroll** avec l’argument *FetchOrientation* défini sur SQL_FETCH_RELATIVE et l’argument *FetchOffset* défini sur 0.  
  
 Pour extraire des données d’une colonne indépendante, l’application appelle **SQLSetPos** pour positionner le curseur sur la ligne souhaitée. Il appelle ensuite **SQLGetData** pour récupérer les données.  
  
 Pour déterminer le nombre de lignes qui ont été extraites de la source de données, l’application appelle **SQLRowCount**.
