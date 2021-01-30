---
description: 'SQL à C : Intervalles de jours-heures'
title: 'SQL en C : intervalles de Day-Time | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], day-time intervals
- day-time intervals [ODBC]
- data conversions from SQL to C types [ODBC], day-time intervals
- intervals [ODBC], converting
ms.assetid: 8ea84d69-2292-4128-89a0-f184f68abb98
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ae81fdf91c59457a6453958cb1c906abdd9d53fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203035"
---
# <a name="sql-to-c-day-time-intervals"></a>SQL à C : Intervalles de jours-heures

Les identificateurs pour les types de données SQL ODBC d’intervalle de jour sont les suivants :

- SQL_INTERVAL_DAY
- SQL_INTERVAL_DAY_TO_MINUTE
- SQL_INTERVAL_HOUR
- SQL_INTERVAL_DAY_TO_SECOND
- SQL_INTERVAL_MINUTE
- SQL_INTERVAL_HOUR_TO_MINUTE
- SQL_INTERVAL_SECOND
- SQL_INTERVAL_HOUR_TO_SECOND
- SQL_INTERVAL_DAY_TO_HOUR
- SQL_INTERVAL_MINUTE_TO_SECOND

Le tableau suivant répertorie les types de données ODBC C dans lesquels les données SQL de l’intervalle de temps peuvent être converties. Pour obtenir une explication des colonnes et des termes du tableau, consultez [conversion de données SQL en types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).

|Identificateur de type C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|Tous les types d’intervalle de temps C|La partie des champs de fin n’est pas tronquée<br /><br /> Partie des champs de fin tronquée<br /><br /> La précision de pointe de la cible n’est pas assez grande pour contenir les données de la source|Données<br /><br /> Données tronquées<br /><br /> Indéfini|Longueur des données<br /><br /> Longueur des données<br /><br /> Indéfini|n/a<br /><br /> 01S07<br /><br /> 22015|  
|SQL_C_STINYINT [b] SQL_C_UTINYINT [b] SQL_C_USHORT [b] SQL_C_SHORT [b] SQL_C_SLONG [b] SQL_C_ULONG [b] SQL_C_NUMERIC [b] SQL_C_BIGINT [b]|La précision de l’intervalle était un champ unique et les données ont été converties sans troncation<br /><br /> La précision de l’intervalle était un champ unique et une fraction tronquée<br /><br /> La précision de l’intervalle était un champ unique et tronqué dans son ensemble<br /><br /> La précision de l’intervalle n’est pas un champ unique|Données<br /><br /> Données tronquées<br /><br /> Données tronquées<br /><br /> Indéfini|Taille du type de données C<br /><br /> Longueur des données<br /><br /> Longueur des données<br /><br /> Taille du type de données C|n/a<br /><br /> 01S07<br /><br /> 22003<br /><br /> 07006|  
|SQL_C_BINARY|Longueur en octets des données <= *BufferLength*<br /><br /> Longueur en octets des données > *BufferLength*|Données<br /><br /> Indéfini|Longueur des données<br /><br /> Indéfini|n/a<br /><br /> 22003|  
|SQL_C_CHAR|Longueur en octets < *BufferLength*<br /><br /> Nombre de chiffres entiers (par opposition à fractionnaires) < *BufferLength*<br /><br /> Nombre de chiffres entiers (par opposition à fractionnaires) >= *BufferLength*|Données<br /><br /> Données tronquées<br /><br /> Indéfini|Taille du type de données C<br /><br /> Taille du type de données C<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|Longueur de caractère < *BufferLength*<br /><br /> Nombre de chiffres entiers (par opposition à fractionnaires) < *BufferLength*<br /><br /> Nombre de chiffres entiers (par opposition à fractionnaires) >= *BufferLength*|Données<br /><br /> Données tronquées<br /><br /> Indéfini|Taille du type de données C<br /><br /> Taille du type de données C<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
  
 [a] un type SQL d’intervalle de jours peut être converti en un type C d’intervalle de jour/heure.  
  
 [b] si la précision de l’intervalle est un champ unique (l’un des jours, heures, minutes ou secondes), le type SQL de l’intervalle peut être converti en n’importe quel nombre exact (SQL_C_STINYINT, SQL_C_UTINYINT, SQL_C_USHORT, SQL_C_SHORT, SQL_C_SLONG, SQL_C_ULONG ou SQL_C_NUMERIC).  
  
La conversion par défaut d’un type SQL interval est vers le type de données de l’intervalle C correspondant. L’application lie ensuite la colonne ou le paramètre (ou définit le champ SQL_DESC_DATA_PTR dans l’enregistrement approprié du ARD) pour pointer vers la structure de SQL_INTERVAL_STRUCT initialisée (ou passe un pointeur vers la structure de INTERVAL_STRUCT SQL_ en tant qu’argument *TargetValuePtr* dans un appel à **SQLGetData**).  
  
L’exemple suivant montre comment transférer des données à partir d’une colonne de type SQL_INTERVAL_DAY_TO_MINUTE dans la structure SQL_INTERVAL_STRUCT, de sorte qu’elle revient sous la forme d’un intervalle de DAY_TO_HOUR.  

```cpp
SQL_INTERVAL_STRUCT is;  
SQLINTEGER    cbValue;  
SQLUINTEGER   days, hours;  
  
// Execute a select statement; "interval_column" is a column  
// whose data type is SQL_INTERVAL_DAY_TO_MINUTE.  
SQLExecDirect(hstmt, "SELECT interval_column FROM table", SQL_NTS);  
  
// Bind  
SQLBindCol(hstmt, 1, SQL_C_INTERVAL_DAY_TO_MINUTE, &is, sizeof(SQL_INTERVAL_STRUCT), &cbValue);  
  
// Fetch  
SQLFetch(hstmt);  
  
// Process data  
days = is.intval.day_second.day;  
hours = is.intval.day_second.hour;  
```
