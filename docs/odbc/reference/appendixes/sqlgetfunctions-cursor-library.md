---
description: SQLGetFunctions (bibliothèque de curseurs)
title: SQLGetFunctions (bibliothèque de curseurs) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLGetFunctions function [ODBC], Cursor Library
ms.assetid: 931acd12-4eb6-4a78-9a77-157a18a9a2d0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9414aa4d2be551ab3b6a44193f6b6c17e2deb145
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202770"
---
# <a name="sqlgetfunctions-cursor-library"></a>SQLGetFunctions (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Cette rubrique traite de l’utilisation de la fonction **SQLGetFunctions** dans la bibliothèque de curseurs. Pour obtenir des informations générales sur **SQLGetFunctions**, consultez [fonction SQLGetFunctions](../../../odbc/reference/syntax/sqlgetfunctions-function.md).  
  
 Lorsque vous appelez **SQLGetFunctions**, la bibliothèque de curseurs retourne la prise en charge de **SQLExtendedFetch**, **SQLFetchScroll**, **SQLSetPos** et **SQLSetScrollOptions**, en plus des fonctions prises en charge par le pilote.
