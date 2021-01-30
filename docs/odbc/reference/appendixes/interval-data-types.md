---
description: Types de données d’intervalle
title: Types de données Interval | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- second intervals [ODBC]
- data types [ODBC], interval data types
- interval data type [ODBC]
- day-time intervals [ODBC]
- intervals [ODBC], about intervals
- minute intervals [ODBC]
- day intervals [ODBC]
- year intervals [ODBC]
- month intervals [ODBC]
- interval data type [ODBC], about interval data types
- SQL data types [ODBC], interval
- year-month intervals [ODBC]
- C data types [ODBC], interval
- interval fields [ODBC]
ms.assetid: fba93f65-c1db-44f4-91ba-532f87241cf7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 41418b59d61b184717c4a3654491154221b9a791
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207134"
---
# <a name="interval-data-types"></a>Types de données d’intervalle
Un intervalle est défini comme la différence entre deux dates et heures. Les intervalles sont exprimés de deux façons différentes. L’un est un intervalle d' *année-mois* qui exprime les intervalles en termes d’années et un nombre entier de mois. L’autre est un intervalle de *jour-heure* qui exprime les intervalles en termes de jours, de minutes et de secondes. Ces deux types d’intervalles sont distincts et ne peuvent pas être mélangés, car les mois peuvent avoir différents nombres de jours.  
  
 Un intervalle se compose d’un ensemble de champs. Il existe un classement implicite entre les champs. Par exemple, dans un intervalle d’une année à l’autre, l’année se produit en premier, suivi du mois. De même, dans un intervalle de jour à minute, les champs sont dans l’ordre jour, heure et minute. Le premier champ d’un type d’intervalle est appelé champ de *début* ou champ de *poids fort* . Le dernier champ est appelé « champ de *fin* ».  
  
 Dans tous les intervalles, le champ de début n’est pas imposé par les règles du calendrier grégorien. Par exemple, dans un intervalle d’une heure et d’une minute, le champ d’heure n’est pas restreint pour être compris entre 0 et 23 (inclus), comme c’est normalement le cas. Les champs de fin postérieurs au champ de début suivent les contraintes habituelles du calendrier grégorien. Pour plus d’informations, consultez [contraintes du calendrier grégorien](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md), plus loin dans cette annexe.  
  
 Il existe 13 types de données SQL Interval et treize types de données de l’intervalle C. Chacun des types de données Interval C utilise la même structure, SQL_INTERVAL_STRUCT, pour contenir les données d’intervalle. (Pour plus d’informations, consultez la section suivante, [structure d’intervalles C](../../../odbc/reference/appendixes/c-interval-structure.md).) Pour plus d’informations sur les types de données SQL, consultez [types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md); Pour plus d’informations sur les types de données C, consultez [types de données c](../../../odbc/reference/appendixes/c-data-types.md).  
  
|Identificateur de type|Class|Description|  
|---------------------|-----------|-----------------|  
|MONTH|Year-Month|Nombre de mois entre deux dates.|  
|YEAR|Year-Month|Nombre d’années entre deux dates.|  
|YEAR_TO_MONTH|Year-Month|Nombre d’années et de mois entre deux dates.|  
|DAY|Day-Time|Nombre de jours entre deux dates.|  
|HOUR|Day-Time|Nombre d’heures entre deux dates/heures.|  
|MINUTE|Day-Time|Nombre de minutes entre deux dates/heures.|  
|SECOND|Day-Time|Nombre de secondes entre deux dates/heures.|  
|DAY_TO_HOUR|Day-Time|Nombre de jours/heures entre deux dates/heures.|  
|DAY_TO_MINUTE|Day-Time|Nombre de jours/heures/minutes entre deux dates/heures.|  
|DAY_TO_SECOND|Day-Time|Nombre de jours/heures/minutes/secondes entre deux dates/heures.|  
|HOUR_TO_MINUTE|Day-Time|Nombre d’heures/minutes entre deux dates/heures.|  
|HOUR_TO_SECOND|Day-Time|Nombre d’heures/minutes/secondes entre deux dates/heures.|  
|MINUTE_TO_SECOND|Day-Time|Nombre de minutes/secondes entre deux dates/heures.|  
  
 Cette section contient les rubriques suivantes :  
  
-   [Structure d’intervalle C](../../../odbc/reference/appendixes/c-interval-structure.md)  
  
-   [Précision des types de données d’intervalle](../../../odbc/reference/appendixes/interval-data-type-precision.md)  
  
-   [Longueur des types de données d’intervalle](../../../odbc/reference/appendixes/interval-data-type-length.md)  
  
-   [Littéraux d’intervalle](../../../odbc/reference/appendixes/interval-literals.md)  
  
-   [Remplacement de la précision du début et de la fin par défaut pour les types de données d’intervalle](../../../odbc/reference/appendixes/overriding-default-leading-and-seconds-precision-for-interval-data-types.md)
