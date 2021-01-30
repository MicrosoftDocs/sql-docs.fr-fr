---
description: SQLRemoveDefaultDataSource, fonction
title: SQLRemoveDefaultDataSource fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRemoveDefaultDataSource
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveDefaultDataSource
helpviewer_keywords:
- SQLRemoveDefaultDataSource function [ODBC]
ms.assetid: db803266-57df-4864-a41b-901247549c1f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 326522986785d7e76bc781fa4cc912401f2c237e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192515"
---
# <a name="sqlremovedefaultdatasource-function"></a>SQLRemoveDefaultDataSource, fonction
**Conformité**  
 Version introduite : ODBC 1,0, déconseillé  
  
 **Résumé**  
 Dans ODBC 3,0, la fonction **SQLRemoveDefaultDataSource** a été remplacée par un appel à [SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md) avec un argument *fRequest* de ODBC_REMOVE_DEFAULT_DSN. Si un programme d’installation ODBC 2 *. x* appelle cette fonction, le programme d’installation ODBC le mappe à l’appel de **SQLConfigDataSource** suivant :  
  
```cpp  
SQLConfigDataSource (NULL, ODBC_REMOVE_DEFAULT_DSN, NULL, NULL)  
```
