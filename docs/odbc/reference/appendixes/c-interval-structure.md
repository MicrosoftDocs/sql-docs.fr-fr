---
description: Structure d’intervalle C
title: Structure d’intervalle C | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], interval data types
- interval data type [ODBC], structure
- C data types [ODBC], interval
ms.assetid: 52b42b56-50aa-4ce6-8d79-0963c7a71437
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e470d0e30864c953fa3937725bcd5597bf542f41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212436"
---
# <a name="c-interval-structure"></a>Structure d’intervalle C
Chacun des types de données d’intervalle C listés dans la section [types de données c](../../../odbc/reference/appendixes/c-data-types.md) utilise la même structure pour contenir les données d’intervalle. Quand **SQLFetch**, **SQLFetchScroll** ou **SQLGetData** est appelé, le pilote retourne des données dans la structure SQL_INTERVAL_STRUCT, utilise la valeur spécifiée par l’application pour les types de données C (dans l’appel à **SQLBindCol**, **SQLGetData** ou **SQLBindParameter**) pour interpréter le contenu de SQL_INTERVAL_STRUCT et remplit le champ *Interval_type* de la structure avec la valeur d' *énumération* correspondant au type c. Notez que les pilotes ne lisent pas le champ *interval_type* pour déterminer le type de l’intervalle. ils récupèrent la valeur du champ descripteur SQL_DESC_CONCISE_TYPE. Lorsque la structure est utilisée pour les données de paramètre, le pilote utilise la valeur spécifiée par l’application dans le champ SQL_DESC_CONCISE_TYPE de l’APD pour interpréter le contenu de SQL_INTERVAL_STRUCT, même si l’application définit la valeur du champ *interval_type* sur une autre valeur.  
  
 Cette structure est définie comme suit :  
  
```  
typedef struct tagSQL_INTERVAL_STRUCT  
{  
   SQLINTERVAL interval_type;   
   SQLSMALLINT interval_sign;  
   union {  
         SQL_YEAR_MONTH_STRUCT   year_month;  
         SQL_DAY_SECOND_STRUCT   day_second;  
         } intval;  
} SQL_INTERVAL_STRUCT;  
typedef enum   
{  
   SQL_IS_YEAR = 1,  
   SQL_IS_MONTH = 2,  
   SQL_IS_DAY = 3,  
   SQL_IS_HOUR = 4,  
   SQL_IS_MINUTE = 5,  
   SQL_IS_SECOND = 6,  
   SQL_IS_YEAR_TO_MONTH = 7,  
   SQL_IS_DAY_TO_HOUR = 8,  
   SQL_IS_DAY_TO_MINUTE = 9,  
   SQL_IS_DAY_TO_SECOND = 10,  
   SQL_IS_HOUR_TO_MINUTE = 11,  
   SQL_IS_HOUR_TO_SECOND = 12,  
   SQL_IS_MINUTE_TO_SECOND = 13  
} SQLINTERVAL;  
  
typedef struct tagSQL_YEAR_MONTH  
{  
   SQLUINTEGER year;  
   SQLUINTEGER month;   
} SQL_YEAR_MONTH_STRUCT;  
  
typedef struct tagSQL_DAY_SECOND  
{  
   SQLUINTEGER day;  
   SQLUINTEGER hour;  
   SQLUINTEGER minute;  
   SQLUINTEGER second;  
   SQLUINTEGER fraction;  
} SQL_DAY_SECOND_STRUCT;  
```  
  
 Le champ *interval_type* de la SQL_INTERVAL_STRUCT indique à l’application la structure qui est conservée dans l’Union, ainsi que les membres de la structure qui sont pertinents. La valeur du champ *interval_sign* est SQL_FALSE si le champ de début de l’intervalle n’est pas signé ; s’il est SQL_TRUE, le champ de début est négatif. La valeur du champ de début est toujours non signée, quelle que soit la valeur de *interval_sign*. Le champ *interval_sign* agit comme un bit de signe.
