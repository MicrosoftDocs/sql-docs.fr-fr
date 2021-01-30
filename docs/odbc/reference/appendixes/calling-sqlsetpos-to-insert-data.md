---
description: Appel de SQLSetPos pour insérer des données
title: Appel de SQLSetPos pour l’insertion de données | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- compatibility [ODBC], SQLSetPos
- SQLSetPos function [ODBC], inserting data
- backward compatibility [ODBC], SqlSetPos
ms.assetid: 03e5c4d0-2bb3-4649-9781-89cab73f78eb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6c9ddd58e9f463d7878bb4b9b8710ce0e6dfecc4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99110904"
---
# <a name="calling-sqlsetpos-to-insert-data"></a>Appel de SQLSetPos pour insérer des données
Quand une application ODBC *2. x* qui utilise un pilote *ODBC 3. x* appelle **SQLSetPos** avec un argument *operation* de SQL_ADD, le gestionnaire de pilotes ne mappe pas cet appel à **SQLBulkOperations**. Si un pilote ODBC *3. x* doit fonctionner avec une application qui appelle **SQLSetPos** avec SQL_ADD, le pilote doit prendre en charge cette opération.  
  
 Une différence majeure dans le comportement lorsque **SQLSetPos** est appelé avec SQL_ADD se produit lorsqu’il est appelé dans l’État S6. Dans ODBC *2. x*, le pilote a retourné S1010 quand **SQLSetPos** a été appelé avec SQL_ADD dans état S6 (une fois que le curseur a été positionné avec **SQLFetch**). Dans ODBC *3. x*, **SQLBulkOperations** avec une *opération* de SQL_ADD peut être appelée dans State S6. Une deuxième différence majeure dans le comportement est que **SQLBulkOperations** avec une *opération* de SQL_ADD peut être appelée dans l’État S5, alors que **SQLSetPos** avec une **opération** de SQL_ADD ne peut pas. Pour les transitions d’instruction qui peuvent se produire pour le même appel dans ODBC *3. x*, consultez [annexe B : tables de transition d’État ODBC](../../../odbc/reference/appendixes/appendix-b-odbc-state-transition-tables.md).
