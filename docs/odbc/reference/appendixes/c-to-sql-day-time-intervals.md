---
description: 'C en SQL : Intervalles de jours-heures'
title: 'C en SQL : intervalles de Day-Time | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- day-time intervals [ODBC]
- data conversions from C to SQL types [ODBC], day-time intervals
- converting data from c to SQL types [ODBC], day-time intervals
- intervals [ODBC], converting
ms.assetid: f9ee1ddb-dec7-4f78-b6e2-5ba34e7d6f59
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e207a7533a0f09360925f6123e8797bdabecc21b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207824"
---
# <a name="c-to-sql-day-time-intervals"></a>C en SQL : Intervalles de jours-heures
Les identificateurs pour les types de données ODBC C d’intervalle de jour sont les suivants :  
  
 SQL_C_INTERVAL_DAY  
  
 SQL_C_INTERVAL_HOUR  
  
 SQL_C_INTERVAL_MINUTE  
  
 SQL_C_INTERVAL_SECOND  
  
 SQL_C_INTERVAL_DAY_TO_HOUR  
  
 SQL_C_INTERVAL_DAY_TO_MINUTE  
  
 SQL_C_INTERVAL_DAY_TO_SECOND  
  
 SQL_C_INTERVAL_HOUR_TO_MINUTE  
  
 SQL_C_INTERVAL_HOUR_TO_SECOND  
  
 SQL_C_INTERVAL_MINUTE_TO_SECOND  
  
 Le tableau suivant présente les types de données SQL ODBC dans lesquels les données d’intervalle C peuvent être converties. Pour obtenir une explication des colonnes et des termes du tableau, consultez [conversion de données de C en types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificateur de type SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR [a]<br /><br /> SQL_VARCHAR [a]<br /><br /> SQL_LONGVARCHAR [a]|Longueur d’octet de colonne >= longueur d’octet de caractère<br /><br /> Longueur d’octet de colonne < longueur d’octet de caractère [a]<br /><br /> La valeur des données n’est pas un littéral d’intervalle valide|n/a<br /><br /> 22001<br /><br /> 22015|  
|SQL_WCHAR [a]<br /><br /> SQL_WVARCHAR [a]<br /><br /> SQL_WLONGVARCHAR [a]|Longueur de caractère de colonne >= longueur de caractère des données<br /><br /> Longueur des caractères de la colonne < de la longueur des données [a]<br /><br /> La valeur des données n’est pas un littéral d’intervalle valide|n/a<br /><br /> 22001<br /><br /> 22015|  
|SQL_TINYINT [b]<br /><br /> SQL_SMALLINT [b] SQL_INTEGER [b]<br /><br /> SQL_BIGINT [b] SQL_NUMERIC [b]<br /><br /> SQL_DECIMAL [b]|La conversion d’un intervalle à champ unique n’a pas abouti à la troncation de chiffres entiers<br /><br /> La conversion a entraîné la troncation de chiffres entiers|n/a<br /><br /> 22003|  
|SQL_INTERVAL_DAY<br /><br /> SQL_INTERVAL_HOUR<br /><br /> SQL_INTERVAL_MINUTE<br /><br /> SQL_INTERVAL_SECOND<br /><br /> SQL_INTERVAL_DAY_TO_HOUR<br /><br /> SQL_INTERVAL_DAY_TO_MINUTE<br /><br /> SQL_INTERVAL_DAY_TO_SECOND<br /><br /> SQL_INTERVAL_HOUR_TO_MINUTE<br /><br /> SQL_INTERVAL_HOUR_TO_SECOND<br /><br /> SQL_INTERVAL_MINUTE_TO_SECOND|La valeur des données a été convertie sans troncation des champs<br /><br /> Un ou plusieurs champs de la valeur de données ont été tronqués pendant la conversion|n/a<br /><br /> 22015|  
  
 [a] tous les types de données d’intervalle C peuvent être convertis en type de données caractère.  
  
 [b] si le champ type de la structure interval est tel que l’intervalle est un champ unique (SQL_DAY, SQL_HOUR, SQL_MINUTE ou SQL_SECOND), le type d’intervalle C peut être converti en n’importe quel nombre exact (SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER, SQL_BIGINT, SQL_DECIMAL ou SQL_NUMERIC).  
  
 La conversion par défaut d’un type d’intervalle C est vers le type SQL de l’intervalle de date et d’heure correspondant.  
  
 Le pilote ignore la valeur de longueur/indicateur lors de la conversion de données à partir du type de données Interval C et suppose que la taille de la mémoire tampon de données est égale à la taille du type de données Interval C. La valeur de longueur/indicateur est passée dans l’argument *StrLen_Or_Ind* dans **SQLPutData** et dans la mémoire tampon spécifiée avec l’argument *StrLen_or_IndPtr* dans **SQLBindParameter**. La mémoire tampon de données est spécifiée avec l’argument *DataPtr* dans **SQLPutData** et l’argument *ParameterValuePtr* dans **SQLBindParameter**.  
  
 L’exemple suivant montre comment envoyer des données d’intervalle C stockées dans la structure SQL_INTERVAL_STRUCT dans une colonne de base de données. La structure Interval contient une DAY_TO_SECOND intervalle ; elle est stockée dans une colonne de base de données de type SQL_INTERVAL_DAY_TO_MINUTE.  
  
```  
SQL_INTERVAL_STRUCT is;  
SQLINTEGER cbValue;  
  
// Initialize the interval struct to contain the DAY_TO_SECOND  
// interval "154 days, 22 hours, 44 minutes, and 10 seconds"  
is.intval.day_second.day      = 154;  
is.intval.day_second.hour     = 22;  
is.intval.day_second.minute   = 44;  
is.intval.day_second.second   = 10;  
is.interval_sign              = SQL_FALSE;  
  
// Bind the dynamic parameter  
SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_INTERVAL_DAY_TO_SECOND,  
                  SQL_INTERVAL_DAY_TO_MINUTE, 0, 0, &is,  
                  sizeof(SQL_INTERVAL_STRUCT), &cbValue);  
  
// Execute an insert statement; "interval_column" is a column  
// whose data type is SQL_INTERVAL_DAY_TO_MINUTE.  
SQLExecDirect(hstmt,"INSERT INTO table(interval_column) VALUES (?)",SQL_NTS);  
```
