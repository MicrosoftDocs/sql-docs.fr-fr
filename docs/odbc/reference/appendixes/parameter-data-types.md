---
description: Types de données de paramètre
title: Types de données de paramètre | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], parameters
- parameter data type [ODBC]
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
ms.assetid: fd7e99d8-d26a-408c-9733-6ffccde99f75
author: David-Engel
ms.author: v-daenge
ms.reviewer: ''
ms.openlocfilehash: fbc01ee7f6c4e7cd3ca7dbe4deb6e78e5ff78449
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160827"
---
# <a name="parameter-data-types"></a>Types de données de paramètre
Même si chaque paramètre spécifié avec **SQLBindParameter** est défini à l’aide d’un type de données SQL, les paramètres d’une instruction SQL n’ont pas de type de données intrinsèque. Par conséquent, les marqueurs de paramètres peuvent être inclus dans une instruction SQL uniquement si leurs types de données peuvent être déduits à partir d’un autre opérande dans l’instruction. Par exemple, dans une expression arithmétique telle que ? + COLONNE1, le type de données du paramètre peut être déduit à partir du type de données de la colonne nommée représentée par COLONNE1. Une application ne peut pas utiliser un marqueur de paramètre si le type de données ne peut pas être déterminé.  
  
 Le tableau suivant décrit la façon dont un type de données est déterminé pour plusieurs types de paramètres, conformément à SQL-92. Pour obtenir une spécification plus complète sur la déduction du type de paramètre lorsque d’autres clauses SQL sont utilisées, consultez la spécification SQL-92.  
  
|Emplacement du paramètre|Type de données supposé|  
|---------------------------|-----------------------|  
|Un opérande d’un opérateur binaire arithmétique ou de comparaison|Identique à l’autre opérande|  
|Premier opérande dans une clause **between**|Identique au second opérande|  
|Deuxième ou troisième opérande dans une clause **between**|Identique au premier opérande|  
|Expression utilisée avec **dans**|Identique à la première valeur ou à la colonne de résultat de la sous-requête|  
|Valeur utilisée avec **dans**|Identique à l’expression ou à la première valeur s’il existe un marqueur de paramètre dans l’expression|  
|Valeur de modèle utilisée avec **Like**|VARCHAR|  
|Une valeur de mise à jour utilisée avec **Update**|Identique à la colonne mettre à jour|
