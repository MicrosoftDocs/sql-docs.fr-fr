---
description: SQLSetEnvAttr et la bibliothèque de curseurs
title: SQLSetEnvAttr et la bibliothèque de curseurs | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetEnvAttr function [ODBC], Cursor Library
ms.assetid: 59cc8eae-09ae-4796-869a-c5806488ae83
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 791933e7c4edd263e282437b06fb79fca65fca65
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202639"
---
# <a name="sqlsetenvattr-and-the-cursor-library"></a>SQLSetEnvAttr et la bibliothèque de curseurs
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Cette rubrique traite de l’utilisation de la fonction **SQLSetEnvAttr** avec la bibliothèque de curseurs. Pour obtenir des informations générales sur **SQLSetEnvAttr**, consultez [fonction SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md).  
  
 La bibliothèque de curseurs n’est pas affectée par le paramètre de l’attribut d’environnement SQL_ATTR_ODBC_VERSION, quelle que soit la version de l’application ou du pilote.
