---
description: WHERE, clause - limitations
title: Limitations des clauses WHERE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, WHERE clause limitations
- WHERE clause limitations [ODBC]
ms.assetid: 46b54f74-e4a3-4318-87cf-8a97c38a2718
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 80a0d29b57e5498c6d14e5d1e79600751d6f85c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176508"
---
# <a name="where-clause-limitations"></a>WHERE, clause - limitations
Le nombre maximal de clauses dans une clause WHERE est 40.  
  
 Les colonnes LONGVARBINARY et LONGVARCHAR peuvent être comparées à des littéraux d’une longueur maximale de 255 caractères, mais elles ne peuvent pas être comparées à l’aide de paramètres.
