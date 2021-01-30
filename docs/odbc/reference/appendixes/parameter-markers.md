---
description: Marqueurs de paramètres
title: Marqueurs de paramètres | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
- parameter markers [ODBC]
ms.assetid: 07213d04-cd31-45fd-a8c8-2e16e09eeaf4
author: David-Engel
ms.author: v-daenge
ms.reviewer: ''
ms.openlocfilehash: 7ab9b0cd8682ecc722f97b563285bdd31665c1fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205339"
---
# <a name="parameter-markers"></a>Marqueurs de paramètres
Conformément à la spécification SQL-92, une application ne peut pas placer de marqueurs de paramètres aux emplacements suivants. Pour obtenir une liste plus complète, consultez la spécification SQL-92.  
  
-   Dans une liste de **sélection**  
  
-   À la fois en tant qu' *expressions* dans un *prédicat de comparaison*  
  
-   Comme opérandes d’un opérateur binaire  
  
-   En tant que premier et deuxième opérandes d’une opération **between**  
  
-   Comme les premier et troisième opérandes d’une opération **between**  
  
-   À la fois l’expression et la première valeur d’une opération **in**  
  
-   Comme opérande d’une opération + or unaire  
  
-   En tant qu’argument d’une *fonction Set-Function-Reference*  
  
 Pour plus d’informations sur les marqueurs de paramètres, consultez la spécification SQL-92. Pour plus d’informations sur les paramètres, consultez [paramètres d’instruction](../../../odbc/reference/develop-app/statement-parameters.md).
