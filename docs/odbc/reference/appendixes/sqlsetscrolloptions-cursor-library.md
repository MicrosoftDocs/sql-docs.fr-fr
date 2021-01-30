---
description: SQLSetScrollOptions (bibliothèque de curseurs)
title: SQLSetScrollOptions (bibliothèque de curseurs) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetScrollOptions function [ODBC], Cursor Library
ms.assetid: c5c0ac6d-a6c1-4077-8186-1644df1944f8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: eaba3884e90883cded418acc46d73cc1c0dfba88
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202606"
---
# <a name="sqlsetscrolloptions-cursor-library"></a>SQLSetScrollOptions (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Cette rubrique traite de l’utilisation de la fonction **SQLSetScrollOptions** dans la bibliothèque de curseurs. Pour obtenir des informations générales sur **SQLSetScrollOptions**, consultez [fonction SQLSetScrollOptions](../../../odbc/reference/syntax/sqlsetscrolloptions-function.md).  
  
 La bibliothèque de curseurs prend en charge **SQLSetScrollOptions** uniquement à des fins de compatibilité descendante. les applications doivent utiliser les attributs d’instruction SQL_ATTR_CONCURRENCY, SQL_ATTR_CURSOR_TYPE et SQL_ATTR_ROW_ARRAY_SIZE à la place.
