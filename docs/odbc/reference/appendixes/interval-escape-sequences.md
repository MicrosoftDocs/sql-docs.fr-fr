---
description: Séquences d’échappement des intervalles
title: Séquences d’échappement d’intervalle | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- interval literals [ODBC]
- escape sequences [ODBC], interval
- ODBC escape sequences [ODBC], interval
ms.assetid: 303e8dab-8f13-4fa5-857f-15cc1f75bdd6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0badac832ee4cf4e29f148dbd39a7989536b9c29
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176495"
---
# <a name="interval-escape-sequences"></a>Séquences d’échappement des intervalles
ODBC utilise des séquences d’échappement pour les littéraux d’intervalle. La syntaxe de cette séquence d’échappement est la suivante :  
  
 {*Interval-Literal*}  
  
 Pour la syntaxe BNF du *littéral Interval*, consultez la section [syntaxe du littéral d’intervalle](../../../odbc/reference/appendixes/interval-literal-syntax.md) plus loin dans cette annexe.  
  
 La séquence d’échappement littérale d’intervalle est prise en charge si les types de données Interval sont pris en charge par la source de données. Une application doit appeler **SQLGetTypeInfo** pour déterminer si ces types de données sont pris en charge.
