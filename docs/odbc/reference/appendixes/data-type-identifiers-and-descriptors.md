---
description: Identificateurs et descripteurs des types de données
title: Identificateurs et descripteurs des types de données | Microsoft Docs
ms.custom: ''
ms.date: 02/02/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- data types [ODBC], identifiers
- identifiers [ODBC], data types
- descriptors [ODBC], data types
- verbose data types [ODBC]
- data types [ODBC], descriptors
- concise data types [ODBC]
ms.assetid: f0077c9b-8eb2-4b5f-8c4c-7436fdef37ab
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dce52118099ff4be572231e7f44f28a4cfca5ea7
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88466219"
---
# <a name="data-type-identifiers-and-descriptors"></a>Identificateurs et descripteurs des types de données
Les types de données répertoriés dans les sections types de données [SQL](../../../odbc/reference/appendixes/sql-data-types.md) et [types de données C](../../../odbc/reference/appendixes/c-data-types.md) plus haut dans cette annexe sont des types de données « concis » : chaque identificateur fait référence à un type de données unique. Il existe une correspondance un-à-un entre l’identificateur et le type de données. Toutefois, dans tous les cas, les descripteurs n’utilisent pas une seule valeur pour identifier les types de données. Dans certains cas, ils utilisent un type de données « verbose » et un sous-code de type. Pour tous les types de données à l’exception des types de données DateTime et Interval, l’identificateur de type détaillé est identique à l’identificateur de type concis et la valeur de SQL_DESC_DATETIME_INTERVAL_CODE est égale à 0. Toutefois, pour les types de données DateTime et Interval, un type détaillé (SQL_DATETIME ou SQL_INTERVAL) est stocké dans SQL_DESC_TYPE, un type concis est stocké dans SQL_DESC_CONCISE_TYPE, et un sous-code pour chaque type concis est stocké dans SQL_DESC_DATETIME_INTERVAL_CODE. La définition de l’un de ces champs affecte les autres. Pour plus d’informations sur ces champs, consultez la description de la fonction [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) .  
  
 Lorsque le champ SQL_DESC_TYPE ou SQL_DESC_CONCISE_TYPE est défini pour certains types de données, les champs SQL_DESC_DATETIME_INTERVAL_PRECISION, SQL_DESC_LENGTH, SQL_DESC_PRECISION et SQL_DESC_SCALE sont automatiquement définis sur les valeurs par défaut, selon le cas pour le type de données. Pour plus d’informations, consultez la description du champ SQL_DESC_TYPE dans [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md). Si l’une des valeurs par défaut n’est pas appropriée, l’application doit définir explicitement le champ descripteur via un appel à **SQLSetDescField**.  
  
 Le tableau suivant indique l’identificateur de type concis, l’identificateur de type détaillé et le sous-code de type pour chaque identificateur de type DateTime et interval SQL et C. Comme le montre ce tableau, pour les types de données DateTime et Interval, les champs SQL_DESC_TYPE et SQL_DESC_DATETIME_INTERVAL_CODE ont les mêmes constantes de manifeste pour les types de données SQL (dans les descripteurs d’implémentation) et pour les types de données C (dans les descripteurs d’application).  
  
|Type SQL concis|Type C concis|Type détaillé|DATETIME_INTERVAL_CODE|  
|----------------------|--------------------|------------------|------------------------------|  
|SQL_TYPE_DATE|SQL_C_TYPE_DATE|SQL_DATETIME|SQL_CODE_DATE|  
|SQL_TYPE_TIME|SQL_C_TYPE_TIME|SQL_DATETIME|SQL_CODE_TIME|  
|SQL_TYPE_TIMESTAMP|SQL_C_TYPE_TIMESTAMP|SQL_DATETIME|SQL_CODE_TIMESTAMP|  
|SQL_INTERVAL_MONTH|SQL_C_INTERVAL_MONTH|SQL_INTERVAL|SQL_CODE_MONTH|  
|SQL_INTERVAL_YEAR|SQL_C_INTERVAL_YEAR|SQL_INTERVAL|SQL_CODE_YEAR|  
|SQL_INTERVAL_YEAR_TO_MONTH|SQL_C_INTERVAL_YEAR_TO_MONTH|SQL_INTERVAL|SQL_CODE_YEAR_TO_MONTH|  
|SQL_INTERVAL_DAY|SQL_C_INTERVAL_DAY|SQL_INTERVAL|SQL_CODE_DAY|  
|SQL_INTERVAL_HOUR|SQL_C_INTERVAL_HOUR|SQL_INTERVAL|SQL_CODE_HOUR|  
|SQL_INTERVAL_MINUTE|SQL_C_INTERVAL_MINUTE|SQL_INTERVAL|SQL_CODE_MINUTE|  
|SQL_INTERVAL_SECOND|SQL_C_INTERVAL_SECOND|SQL_INTERVAL|SQL_CODE_SECOND|  
|SQL_INTERVAL_DAY_TO_HOUR|SQL_C_INTERVAL_DAY_TO_HOUR|SQL_INTERVAL|SQL_CODE_DAY_TO_HOUR|  
|SQL_INTERVAL_DAY_TO_MINUTE|SQL_C_INTERVAL_DAY_TO_MINUTE|SQL_INTERVAL|SQL_CODE_DAY_TO_MINUTE|  
|SQL_INTERVAL_DAY_TO_SECOND|SQL_C_INTERVAL_DAY_TO_SECOND|SQL_INTERVAL|SQL_CODE_DAY_TO_SECOND|  
|SQL_INTERVAL_HOUR_TO_MINUTE|SQL_C_INTERVAL_HOUR_TO_MINUTE|SQL_INTERVAL|SQL_CODE_HOUR_TO_MINUTE|  
|SQL_INTERVAL_HOUR_TO_SECOND|SQL_C_INTERVAL_HOUR_TO_SECOND|SQL_INTERVAL|SQL_CODE_HOUR_TO_SECOND|  
|SQL_INTERVAL_MINUTE_TO_SECOND|SQL_C_INTERVAL_MINUTE_TO_SECOND|SQL_INTERVAL|SQL_CODE_MINUTE_TO_SECOND|
