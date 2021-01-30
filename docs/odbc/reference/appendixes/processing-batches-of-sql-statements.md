---
description: Traitement des lots d’instructions SQL
title: Traitement des lots d’instructions SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], batches
- ODBC cursor library [ODBC], statement processing
- cursor library [ODBC], batches
- SQL statements [ODBC], cursor library
- cursor library [ODBC], statement processing
- SQL statements [ODBC], batches
- batches [ODBC], processing batches of SQL statements
ms.assetid: 04b93ef9-11de-47a3-8bd8-ba963c42f182
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d2c70dc742b170f608246eff1bbb12234a26414b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209484"
---
# <a name="processing-batches-of-sql-statements"></a>Traitement des lots d’instructions SQL
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 La bibliothèque de curseurs ne prend pas en charge les lots d’instructions SQL, y compris les instructions SQL pour lesquelles l’attribut d’instruction SQL_ATTR_PARAMSET_SIZE est supérieur à 1. Si une application soumet un lot d’instructions SQL à la bibliothèque de curseurs, les résultats ne sont pas définis.
