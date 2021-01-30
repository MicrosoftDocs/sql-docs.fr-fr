---
description: SQLSetStmtAttr (bibliothèque de curseurs)
title: SQLSetStmtAttr (bibliothèque de curseurs) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetStmtAttr function [ODBC], Cursor Library
ms.assetid: 6018a733-c2c8-4047-92ec-92cf85031767
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8a395052df9eb5623e63463e3e79722769583721
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202588"
---
# <a name="sqlsetstmtattr-cursor-library"></a>SQLSetStmtAttr (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Cette rubrique traite de l’utilisation de la fonction **SQLSetStmtAttr** dans la bibliothèque de curseurs. Pour obtenir des informations générales sur **SQLSetStmtAttr**, consultez la [fonction SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md).  
  
 La bibliothèque de curseurs prend en charge les attributs d’instruction suivants avec **SQLSetStmtAttr**:  

:::row:::
    :::column:::
        SQL_ATTR_CONCURRENCY  
        SQL_ATTR_CURSOR_TYPE  
        SQL_ATTR_FETCH_BOOKMARK_PTR  
        SQL_ATTR_PARAM_BIND_OFFSET_PTR  
        SQL_ATTR_PARAM_BIND_TYPE  
    :::column-end:::
    :::column:::
        SQL_ATTR_ROW_BIND_OFFSET_PTR  
        SQL_ATTR_ROW_BIND_TYPE  
        SQL_ATTR_ROWSET_ARRAY_SIZE  
        SQL_ATTR_SIMULATE_CURSOR  
        SQL_ATTR_USE_BOOKMARKS  
    :::column-end:::
:::row-end:::

 La bibliothèque de curseurs prend en charge uniquement les valeurs SQL_CURSOR_FORWARD_ONLY et SQL_CURSOR_STATIC de l’attribut d’instruction SQL_ATTR_CURSOR_TYPE.  
  
 Pour les curseurs avant uniquement, la bibliothèque de curseurs prend en charge la SQL_CONCUR_READ_ONLY valeur de l’attribut d’instruction SQL_ATTR_CONCURRENCY. Pour les curseurs statiques, la bibliothèque de curseurs prend en charge les valeurs SQL_CONCUR_READ_ONLY et SQL_CONCUR_VALUES de l’attribut d’instruction SQL_ATTR_CONCURRENCY.  
  
 La bibliothèque de curseurs prend en charge uniquement la valeur SQL_SC_NON_UNIQUE de l’attribut d’instruction SQL_ATTR_SIMULATE_CURSOR.  
  
 Bien que la spécification ODBC prenne en charge les appels à **SQLSetStmtAttr** avec les attributs SQL_ATTR_PARAM_BIND_TYPE ou SQL_ATTR_ROW_BIND_TYPE après l’appel de **SQLFetch** ou **SQLFetchScroll** , la bibliothèque de curseurs ne le fait pas. Avant de pouvoir modifier le type de liaison dans la bibliothèque de curseurs, l’application doit fermer le curseur. La bibliothèque de curseurs prend en charge la modification des attributs d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR, SQL_ATTR_PARAM_BIND_OFFSET_PTR, SQL_ATTR_ROWS_FETCHED_PTR et SQL_ATTR_PARAMS_PROCESSED_PTR lorsqu’un curseur est ouvert.  
  
 Une application peut appeler **SQLSetStmtAttr** avec un **attribut** de SQL_ATTR_ROW_ARRAY_SIZE pour modifier la taille de l’ensemble de lignes lorsqu’un curseur est ouvert. La nouvelle taille de l’ensemble de lignes prendra effet lors du prochain appel de **SQLFetchScroll** ou **SQLFetch** .  
  
 La bibliothèque de curseurs prend en charge la définition de l’attribut d’instruction SQL_ATTR_PARAM_BIND_OFFSET_PTR ou SQL_ATTR_ROW_BIND_OFFSET_PTR pour activer les décalages de liaison. L’offset de liaison n’est pas utilisé pour les appels à **SQLFetch** lorsque la bibliothèque de curseurs est utilisée avec ODBC 2. pilote *x* .  
  
 La bibliothèque de curseurs prend en charge la définition de l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS sur SQL_UB_VARIABLE.
