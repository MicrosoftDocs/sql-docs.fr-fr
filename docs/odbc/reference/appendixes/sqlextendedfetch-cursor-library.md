---
description: SQLExtendedFetch (bibliothèque de curseurs)
title: SQLExtendedFetch (bibliothèque de curseurs) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLExtendedFetch function [ODBC], Cursor Library
ms.assetid: 06fbf06f-127b-475c-b636-7b784918475d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 266d408b765ce5025a7f275c87cc6b548e6e49bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202868"
---
# <a name="sqlextendedfetch-cursor-library"></a>SQLExtendedFetch (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Cette rubrique traite de l’utilisation de la fonction **SQLExtendedFetch** dans la bibliothèque de curseurs. Pour obtenir des informations générales sur **SQLExtendedFetch**, consultez [fonction SQLExtendedFetch](../../../odbc/reference/syntax/sqlextendedfetch-function.md).  
  
 La bibliothèque de curseurs implémente **SQLExtendedFetch** en appelant de façon répétée **SQLFetch** dans le pilote.  
  
 La bibliothèque de curseurs prend en charge l’appel de **SQLExtendedFetch** avec un *FetchOrientation* de SQL_FETCH_BOOKMARK.  
  
 Lorsque la bibliothèque de curseurs est utilisée, les appels à **SQLExtendedFetch** ne peuvent pas être mélangés avec des appels à **SQLFetchScroll** ou **SQLFetch**.
