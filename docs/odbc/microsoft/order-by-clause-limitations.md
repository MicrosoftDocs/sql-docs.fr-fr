---
description: ORDER BY, clause - limitations
title: Limitations des clauses ORDER BY | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, ORDER BY clause limitations
- ORDER BY clause limitations [ODBC]
ms.assetid: fd4ddc7c-9c7e-4a0c-a781-e5427dfb2e18
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 51ec06b42e3aca1c48ef243c7032608d28b21a08
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158199"
---
# <a name="order-by-clause-limitations"></a>ORDER BY, clause - limitations
Si une instruction SELECT contient une clause GROUP BY et une clause ORDER BY, la clause ORDER BY ne peut contenir qu’une colonne du jeu de résultats ou une expression dans la clause GROUP BY.
