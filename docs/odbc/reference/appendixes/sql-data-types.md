---
description: Types de données SQL
title: Types de données SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQL data types [ODBC]
- SQL data types [ODBC], about SQL data types
- data types [ODBC], SQL data types
ms.assetid: 1b22f985-f5e4-4779-87eb-e43329a442b1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e15771f52af0830f07f135453d08f4355a3fae8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199866"
---
# <a name="sql-data-types"></a>Types de données SQL
Chaque SGBD définit ses propres types SQL. Chaque pilote ODBC expose uniquement les types de données SQL que le SGBD associé définit. Informations sur la façon dont un pilote mappe les types SQL SGBD aux identificateurs de type SQL définis par ODBC et comment un pilote mappe les types SQL SGBD à ses propres identificateurs de type SQL propres au pilote, par le biais d’un appel à **SQLGetTypeInfo**. Un pilote retourne également les types de données SQL lors de la description des types de données des colonnes et des paramètres par le biais d’appels à **SQLColAttribute**, **SQLColumns**, **SQLDescribeCol**, **SQLDescribeParam**, **SQLProcedureColumns** et **SQLSpecialColumns**.  
  
> [!NOTE]  
>  Les types de données SQL sont contenus dans les champs SQL_DESC_ CONCISE_TYPE, SQL_DESC_TYPE et SQL_DESC_DATETIME_INTERVAL_CODE des descripteurs d’implémentation. Les caractéristiques des types de données SQL sont contenues dans les champs SQL_DESC_PRECISION, SQL_DESC_SCALE, SQL_DESC_LENGTH et SQL_DESC_OCTET_LENGTH des descripteurs d’implémentation. Pour plus d’informations, consultez [identificateurs et descripteurs des types de données](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) plus loin dans cette annexe.  
  
 Un pilote et une source de données donnés ne prennent pas nécessairement en charge tous les types de données SQL qui sont définis dans cette annexe. La prise en charge des types de données SQL par un pilote dépend du niveau de SQL-92 avec lequel le pilote est compatible. Pour déterminer le niveau de grammaire SQL-92 pris en charge par le pilote, une application appelle **SQLGetInfo** avec le type d’informations SQL_SQL_CONFORMANCE. En outre, un pilote et une source de données donnés peuvent prendre en charge des types de données SQL supplémentaires spécifiques au pilote. Pour déterminer les types de données pris en charge par un pilote, une application appelle **SQLGetTypeInfo**. Pour plus d’informations sur les types de données SQL spécifiques au pilote, consultez la documentation du pilote. Pour plus d’informations sur les types de données dans une source de données spécifique, consultez la documentation de cette source de données.  
  
> [!IMPORTANT]  
>  Les tableaux de cette annexe ne sont que des recommandations et montrent des noms, des plages et des limites d’utilisation de types de données SQL. Une source de données donnée peut uniquement prendre en charge certains des types de données listés, et les caractéristiques des types de données pris en charge peuvent différer de celles figurant dans la liste.  
  
 Le tableau suivant répertorie les identificateurs de type SQL valides pour tous les types de données SQL. La table répertorie également le nom et la description du type de données correspondant à partir de SQL-92 (le cas échéant).  
  
|Identificateur de type SQL [1]|Données SQL standard<br /><br /> type [2]|Description de type standard|  
|------------------------------|------------------------------------|------------------------------|  
|SQL_CHAR|CHAR (*n*)|Chaîne de caractères de longueur de chaîne fixe *n*.|  
|SQL_VARCHAR|VARCHAR (*n*)|Chaîne de caractères de longueur variable avec une longueur de chaîne maximale *n*.|  
|SQL_LONGVARCHAR|LONG VARCHAR|Données de caractères de longueur variable. La longueur maximale dépend de la source de données. 0,9|  
|SQL_WCHAR|WCHAR (*n*)|Chaîne de caractères Unicode de longueur de chaîne fixe *n*|  
|SQL_WVARCHAR|VARWCHAR (*n*)|Chaîne de caractères de longueur variable Unicode avec une longueur de chaîne maximale *n*|  
|SQL_WLONGVARCHAR|LONGWVARCHAR|Données de caractères de longueur variable Unicode. La longueur maximale dépend de la source de données|  
|SQL_DECIMAL|DÉCIMAL (*p*,*s*)|Valeur numérique, exacte et signée avec une précision d’au moins *p* et Scale *s.* (La précision maximale est définie par le pilote.) (1 <= *p* <= 15 ; *s*  <=  *p*). 4|  
|SQL_NUMERIC|NUMÉRIQUE (*p*,*s*)|Valeur numérique, exacte et signée avec une précision *p* et une échelle *s* (1 <= *p* <= 15 ; *s*  <=  *p*). 4|  
|SQL_SMALLINT|SMALLINT|Valeur numérique exacte avec une précision de 5 et une échelle de 0 (signée :-32 768 <= *n* <= 32 767, non signée : 0 <= *n* <= 65535) [3].|  
|SQL_INTEGER|INTEGER|Valeur numérique exacte avec une précision de 10 et une échelle de 0 (signée :-2 [31] <= *n* <= 2 [31]-1, non signée : 0 <= *n* <= 2 [32]-1) [3].|  
|SQL_REAL|real|Valeur numérique approximative signée avec une précision binaire de 24 (zéro ou valeur absolue 10 [-38] à 10 [38]).|  
|SQL_FLOAT|FLOAT (*p*)|Valeur numérique approximative signée avec une précision binaire d’au moins *p*. (La précision maximale est définie par le pilote.) 5,5|  
|SQL_DOUBLE|DOUBLE PRECISION|Valeur numérique approximative signée avec une précision binaire de 53 (zéro ou valeur absolue 10 [-308] à 10 [308]).|  
|SQL_BIT|BIT|Données binaires sur un bit. version8|  
|SQL_TINYINT|TINYINT|Valeur numérique exacte avec une précision de 3 et une échelle de 0 (signée :-128 <= *n* <= 127, non signée : 0 <= *n* <= 255) [3].|  
|SQL_BIGINT|bigint|Valeur numérique exacte avec la précision 19 (si signée) ou 20 (si non signé) et l’échelle 0 (signée :-2 [63] <= *n* <= 2 [63]-1, non signée : 0 <= *n* <= 2 [64]-1) [3], [9].|  
|SQL_BINARY|BINARY (*n*)|Données binaires de longueur fixe *n*. 0,9|  
|SQL_VARBINARY|VARBINARY (*n*)|Données binaires de longueur variable de longueur maximale *n*. La valeur maximale est définie par l’utilisateur. 0,9|  
|SQL_LONGVARBINARY|VARBINARY LONG|Données binaires de longueur variable. La longueur maximale dépend de la source de données. 0,9|  
|SQL_TYPE_DATE [6]|DATE|Champs année, mois et jour, conformes aux règles du calendrier grégorien. (Voir [les contraintes du calendrier grégorien](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md), plus loin dans cette annexe.)|  
|SQL_TYPE_TIME [6]|HEURE (*p*)|Les champs Hour, minute et second, avec des valeurs valides pour les heures de 00 à 23, les valeurs valides pour les minutes de 00 à 59, et les valeurs valides pour les secondes de 00 à 61. La précision *p* indique la précision en secondes.|  
|SQL_TYPE_TIMESTAMP [6]|HORODATEUR (*p*)|Les champs année, mois, jour, heure, minute et seconde, avec des valeurs valides définies pour les types de données de DATE et d’heure.|  
|SQL_TYPE_UTCDATETIME|UTCDATETIME|Champs année, mois, jour, heure, minute, seconde, utchour et utcminute. Les champs utchour et utcminute ont une précision de 1/10 microsecondes.|  
|SQL_TYPE_UTCTIME|UTCTIME|Champs Hour, minute, second, utchour et utcminute. Les champs utchour et utcminute ont une précision de 1/10 microsecondes..|  
|SQL_INTERVAL_MONTH [7]|INTERVALLE mois (*p*)|Nombre de mois entre deux dates ; *p* est la précision de début de l’intervalle.|  
|SQL_INTERVAL_YEAR [7]|INTERVALLE année (*p*)|Nombre d’années entre deux dates ; *p* est la précision de début de l’intervalle.|  
|SQL_INTERVAL_YEAR_TO_MONTH [7]|INTERVALLE année (*p*) à mois|Nombre d’années et de mois entre deux dates ; *p* est la précision de début de l’intervalle.|  
|SQL_INTERVAL_DAY [7]|INTERVALLE jour (*p*)|Nombre de jours entre deux dates ; *p* est la précision de début de l’intervalle.|  
|SQL_INTERVAL_HOUR [7]|HEURE de l’intervalle (*p*)|Nombre d’heures entre deux dates/heures ; *p* est la précision de début de l’intervalle.|  
|SQL_INTERVAL_MINUTE [7]|INTERVALLE MINUTE (*p*)|Nombre de minutes entre deux dates/heures ; *p* est la précision de début de l’intervalle.|  
|SQL_INTERVAL_SECOND [7]|INTERVALLE en secondes (*p*,*q*)|Nombre de secondes entre deux dates/heures ; *p* correspond à la précision de l’intervalle et *q* à la précision de l’intervalle en secondes.|  
|SQL_INTERVAL_DAY_TO_HOUR [7]|INTERVALLE jour (*p*) à heure|Nombre de jours/heures entre deux dates/heures ; *p* est la précision de début de l’intervalle.|  
|SQL_INTERVAL_DAY_TO_MINUTE [7]|INTERVALLE jour (*p*) à minute|Nombre de jours/heures/minutes entre deux dates/heures ; *p* est la précision de début de l’intervalle.|  
|SQL_INTERVAL_DAY_TO_SECOND [7]|INTERVALLE jour (*p*) à seconde (*q*)|Nombre de jours/heures/minutes/secondes entre deux dates/heures ; *p* correspond à la précision de l’intervalle et *q* à la précision de l’intervalle en secondes.|  
|SQL_INTERVAL_HOUR_TO_MINUTE [7]|INTERVALLE en heures (*p*) à minute|Nombre d’heures/minutes entre deux dates/heures ; *p* est la précision de début de l’intervalle.|  
|SQL_INTERVAL_HOUR_TO_SECOND [7]|INTERVALLE en heures (*p*) à seconde (*q*)|Nombre d’heures/minutes/secondes entre deux dates/heures ; *p* correspond à la précision de l’intervalle et *q* à la précision de l’intervalle en secondes.|  
|SQL_INTERVAL_MINUTE_TO_SECOND [7]|INTERVALLE MINUTE (*p*) à seconde (*q*)|Nombre de minutes/secondes entre deux dates/heures ; *p* correspond à la précision de l’intervalle et *q* à la précision de l’intervalle en secondes.|  
|SQL_GUID|GUID|GUID de longueur fixe.|  
  
 [1] il s’agit de la valeur retournée dans la colonne DATA_TYPE par un appel à **SQLGetTypeInfo**.  
  
 [2] il s’agit de la valeur retournée dans la colonne NAME et CREATe params par un appel à **SQLGetTypeInfo**. La colonne NAME retourne la désignation, par exemple CHAR, tandis que la colonne CREATe params retourne une liste de paramètres de création séparés par des virgules, tels que la précision, l’échelle et la longueur.  
  
 [3] une application utilise **SQLGetTypeInfo** ou **SQLColAttribute** pour déterminer si un type de données particulier ou une colonne particulière d’un jeu de résultats n’est pas signé.  
  
 [4] SQL_DECIMAL et SQL_NUMERIC types de données diffèrent uniquement par leur précision. La précision d’une DÉCIMALe (*p*,*s*) est une précision décimale définie par l’implémentation qui n’est pas inférieure à *p*, tandis que la précision d’un numérique (*p*,*s*) est exactement égale à *p*.  
  
 [5] en fonction de l’implémentation, la précision de SQL_FLOAT peut être 24 ou 53 : si la valeur est égale à 24, le type de données SQL_FLOAT est le même que SQL_REAL ; s’il s’agit de 53, le type de données SQL_FLOAT est le même que SQL_DOUBLE.  
  
 [6] dans ODBC *3. x*, les types de données de date, d’heure et d’horodatage SQL sont SQL_TYPE_DATE, SQL_TYPE_TIME et SQL_TYPE_TIMESTAMP, respectivement. dans ODBC *2. x*, les types de données sont SQL_DATE, SQL_TIME et SQL_TIMESTAMP.  
  
 [7] pour plus d’informations sur les types de données SQL Interval, consultez la section [types de données d’intervalle](../../../odbc/reference/appendixes/interval-data-types.md) , plus loin dans cette annexe.  
  
 [8] le type de données SQL_BIT a des caractéristiques différentes de celles du type de bits dans SQL-92.  
  
 [9] ce type de données n’a pas de type de données correspondant dans SQL-92.  
  
 Cette section fournit l’exemple suivant.  
  
-   [Exemple de jeu de résultats de SQLGetTypeInfo](../../../odbc/reference/appendixes/example-sqlgettypeinfo-result-set.md)
