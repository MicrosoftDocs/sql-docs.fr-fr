---
description: SQLBindParam, mappage
title: Mappage SQLBindParam | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- SQLBindparam function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLBindParam
ms.assetid: 375f8f24-36de-4946-916e-c75abc6f070d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f998fd30716e479cb4dd0650af53c5a24483f2f5
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88456460"
---
# <a name="sqlbindparam-mapping"></a>SQLBindParam, mappage
**SQLBindParam** ne peut véritablement pas être appelé comme déconseillé, car il n’existait jamais dans ODBC ; Toutefois, il représente toujours des fonctionnalités dupliquées. le gestionnaire de pilotes doit l’exporter, car les applications ISO et Open Group sont en cours d’utilisation. Comme **SQLBindParameter** contient toutes les fonctionnalités de **SQLBindParam**, **SQLBindParam** sera mappé sur **SQLBindParameter** (lorsque le pilote sous-jacent est un pilote ODBC *3. x* ). Un pilote ODBC *3. x* n’a pas besoin d’implémenter **SQLBindParam**.  
  
## <a name="remarks"></a>Notes  
 Lorsque l’appel suivant à **SQLBindParam** est effectué :  
  
```  
SQLBindParam(   StatementHandle,    ParameterNumber,    ValueType,    ParameterType,    ColumnSize,    DecimalDigits,    ParameterValuePtr,    StrLen_or_IndPtr)  
```  
  
 le gestionnaire de pilotes appelle **SQLBindParameter** dans le pilote comme suit :  
  
```  
SQLBindParameter(   StatementHandle,    ParameterNumber,    SQL_PARAM_INPUT,    ValueType,    ParameterType,    ColumnSize,    DecimalDigits,    ParameterValuePtr,    BufferLength,    StrLen_or_IndPtr)  
```  
  
 Consultez [ODBC 64-informations sur ODBC](../../../odbc/reference/odbc-64-bit-information.md), si votre application s’exécute sur un système d’exploitation 64 bits.  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage des fonctions dépréciées](../../../odbc/reference/appendixes/mapping-deprecated-functions.md)
