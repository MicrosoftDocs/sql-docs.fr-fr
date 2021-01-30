---
description: Taille de colonne
title: Taille de la colonne | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], column size
- size of data types [ODBC]
- SQL data types [ODBC], column characteristics
- column size of data types [ODBC]
ms.assetid: 541b83ab-b16d-4714-bcb2-3c3daa9a963b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bae1a1814ae89b2fe4b91ba9c2936df601c03301
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183665"
---
# <a name="column-size"></a>Taille de colonne
La taille de la colonne (ou du paramètre) des types de données numériques est définie comme étant le nombre maximal de chiffres utilisé par le type de données de la colonne ou du paramètre, ou la précision des données. Pour les types de caractères, il s’agit de la longueur en caractères des données ; pour les types de données binaires, la taille de colonne est définie comme la longueur en octets des données. Pour l’heure, l’horodatage et tous les types de données d’intervalle, il s’agit du nombre de caractères dans la représentation de caractères de ces données. La taille de colonne définie pour chaque type de données SQL concis est indiquée dans le tableau suivant.  
  
|Identificateur de type SQL|Taille de la colonne|  
|-------------------------|-----------------|  
|Tous les types de caractères [a], [b]|Taille de colonne définie ou maximale, en caractères, de la colonne ou du paramètre (comme contenu dans le champ de descripteur SQL_DESC_LENGTH). Par exemple, la taille de colonne d’une colonne de type caractère codé sur un octet définie en tant que CHAR (10) est 10.|  
|SQL_DECIMAL SQL_NUMERIC|Nombre défini de chiffres. Par exemple, la précision d’une colonne définie en tant que valeur numérique (10, 3) est 10.|  
|SQL_BIT (c)|1|  
|SQL_TINYINT (c)|3|  
|SQL_SMALLINT (c)|5|  
|SQL_INTEGER (c)|10|  
|SQL_BIGINT (c)|19 (si signé) ou 20 (si non signé)|  
|SQL_REAL (c)|7|  
|SQL_FLOAT (c)|15|  
|SQL_DOUBLE (c)|15|  
|Tous les types binaires [a], [b]|Longueur définie ou maximale en octets de la colonne ou du paramètre. Par exemple, la longueur d’une colonne définie en tant que BINARY (10) est 10.|  
|SQL_TYPE_DATE (c)|10 (nombre de caractères au format *aaaa-mm-jj* ).|  
|SQL_TYPE_TIME (c)|8 (le nombre de caractères au format *hh-mm-ss* ), ou 9 + *s* (le nombre de caractères au format *hh : mm : SS*[. fff...], où *s* est la précision en secondes).|  
|SQL_TYPE_TIMESTAMP|16 (le nombre de caractères au format *aaaa-mm-jj hh : mm* )<br /><br /> 19 (nombre de caractères au format *aaaa-mm-jj* *hh : mm : SS* )<br /><br /> ou<br /><br /> 20 + *s* (nombre de caractères au format *aaaa-mm-jj hh : mm : SS*[. fff...], où *s* est la précision en secondes).|  
|SQL_INTERVAL_SECOND|Où *p* est la précision de l’intervalle et *s* est la précision en secondes, *p* (si *s*= 0) ou *p* + *s*+ 1 (si *s*>0). [ e|  
|SQL_INTERVAL_DAY_TO_SECOND|Où *p* est la précision de l’intervalle et *s* est la précision en secondes, 9 +*p* (si *s*= 0) ou 10 +*p* + *s* (si *s*>0). [ e|  
|SQL_INTERVAL_HOUR_TO_SECOND|Où *p* est la précision de l’intervalle et *s* est la précision en secondes, 6 +*p* (si *s*= 0) ou 7 +*p* + *s* (si *s*>0). [ e|  
|SQL_INTERVAL_MINUTE_TO_SECOND|Où *p* est la précision de l’intervalle et *s* est la précision en secondes, 3 +*p* (si *s*= 0) ou 4 +*p* + *s* (si *s*>0). [ e|  
|SQL_INTERVAL_YEAR SQL_INTERVAL_MONTH SQL_INTERVAL_DAY SQL_INTERVAL_HOUR SQL_INTERVAL_MINUTE|*p*, où *p* est la précision de début de l’intervalle. e|  
|SQL_INTERVAL_YEAR_TO_MONTH SQL_INTERVAL_DAY_TO_HOUR|3 +*p*, où *p* est la précision de début de l’intervalle. e|  
|SQL_INTERVAL_DAY_TO_MINUTE|6 +*p*, où *p* est la précision de début de l’intervalle. e|  
|SQL_INTERVAL_HOUR_TO_MINUTE|3 +*p*, où *p* est la précision de début de l’intervalle. e|  
|SQL_GUID|36 (nombre de caractères au format *aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee* )|  
  
 [a] pour une application ODBC 1,0 qui appelle **SQLSetParam,** dans un pilote ODBC 2,0, et pour une application ODBC 2,0 qui appelle **SQLBindParameter** dans un pilote odbc 1,0, lorsque \* *StrLen_or_IndPtr* est SQL_DATA_AT_EXEC pour un type SQL_LONGVARCHAR ou SQL_LONGVARBINARY, *Column* doit être défini sur la longueur totale des données à envoyer, et non sur la précision définie dans ce tableau.  
  
 [b] si le pilote ne peut pas déterminer la longueur de la colonne ou du paramètre pour un type de variable, il retourne SQL_NO_TOTAL.  
  
 [c] l’argument *Column* de **SQLBindParameter** est ignoré pour ce type de données.  
  
 [d] pour connaître les règles générales relatives à la longueur des colonnes dans les types de données Interval, consultez longueur des types de [données d’intervalle](../../../odbc/reference/appendixes/interval-data-type-length.md), plus haut dans cette annexe.  
  
 Les valeurs retournées pour la taille de la colonne (ou du paramètre) ne correspondent pas aux valeurs d’un champ de descripteur. Les valeurs peuvent provenir du champ SQL_DESC_PRECISION ou SQL_DESC_LENGTH, selon le type de données, comme indiqué dans le tableau suivant.  
  
|Type SQL|Champ de descripteur correspondant à<br /><br /> taille de la colonne ou du paramètre|  
|--------------|--------------------------------------------------------------------|  
|Tous les types caractère et binaire|LENGTH|  
|Tous les types numériques|PRECISION|  
|Tous les types DateTime et Interval|LENGTH|  
|SQL_BIT|LENGTH|
