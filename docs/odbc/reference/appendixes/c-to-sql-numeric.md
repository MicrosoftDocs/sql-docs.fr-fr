---
description: 'C en SQL : Numérique'
title: 'C en SQL : Numeric | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- numeric data type [ODBC], converting
- data conversions from C to SQL types [ODBC], numeric
- converting data from c to SQL types [ODBC], numeric
ms.assetid: af4095ff-06c3-4b04-83bf-19f9ee098dc2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b825725ada434a72e14f9631504ee343951d6a4a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207819"
---
# <a name="c-to-sql-numeric"></a>C en SQL : Numérique
Les identificateurs des types de données numériques ODBC C sont les suivants :  
  
 SQL_C_STINYINT  
  
 SQL_C_SLONG  
  
 SQL_C_UTINYINT  
  
 SQL_C_ULONG  
  
 SQL_C_TINYINT  
  
 SQL_C_LONG  
  
 SQL_C_SSHORT  
  
 SQL_C_FLOAT  
  
 SQL_C_USHORT  
  
 SQL_C_DOUBLE  
  
 SQL_C_SHORT  
  
 SQL_C_NUMERIC  
  
 SQL_C_SBIGINT  
  
 SQL_C_UBIGINT  
  
 Le tableau suivant répertorie les types de données SQL ODBC dans lesquels les données C numériques peuvent être converties. Pour obtenir une explication des colonnes et des termes du tableau, consultez [conversion de données de C en types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificateur de type SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Nombre de chiffres <= longueur d’octet de colonne<br /><br /> Nombre de chiffres > longueur d’octet de colonne|n/a<br /><br /> 22001|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Nombre de caractères <= longueur des caractères de la colonne<br /><br /> Nombre de caractères > la longueur des caractères de la colonne|n/a<br /><br /> 22001|  
|SQL_DECIMAL [b]<br /><br /> SQL_NUMERIC [b]<br /><br /> SQL_TINYINT [b]<br /><br /> SQL_SMALLINT [b]<br /><br /> SQL_INTEGER [b]<br /><br /> SQL_BIGINT [b]|Données converties sans troncation ou avec des chiffres fractionnaires tronqués<br /><br /> Données converties avec troncation de chiffres entiers|n/a<br /><br /> 22003|  
|SQL_REAL<br /><br /> SQL_FLOAT<br /><br /> SQL_DOUBLE|Les données se trouvent dans la plage du type de données dans lequel le nombre est converti<br /><br /> Les données se trouvent en dehors de la plage du type de données dans lequel le nombre est converti|n/a<br /><br /> 22003|  
|SQL_BIT|Les données sont 0 ou 1<br /><br /> Les données sont supérieures à 0, inférieures à 2 et ne sont pas égales à 1<br /><br /> Les données sont inférieures à 0 ou supérieures ou égales à 2|n/a<br /><br /> 22001<br /><br /> 22003|  
|SQL_INTERVAL_YEAR [a]<br /><br /> SQL_INTERVAL_MONTH [a]<br /><br /> SQL_INTERVAL_DAY [a]<br /><br /> SQL_INTERVAL_HOUR [a]<br /><br /> SQL_INTERVAL_MINUTE [a]<br /><br /> SQL_INTERVAL_SECOND [a]|Données non tronquées.<br /><br /> Données tronquées.|n/a<br /><br /> 22015|  
  
 [a] ces conversions sont prises en charge uniquement pour les types de données numériques exacts (SQL_C_STINYINT, SQL_C_UTINYINT, SQL_C_SSHORT, SQL_C_USHORT, SQL_C_SLONG, SQL_C_ULONG ou SQL_C_NUMERIC). Ils ne sont pas pris en charge pour les types de données numériques approximatifs (SQL_C_FLOAT ou SQL_C_DOUBLE). Les types de données numériques exacts ne peuvent pas être convertis en type SQL Interval dont la précision de l’intervalle n’est pas un champ unique.  
  
 [b] pour le cas « n/a », un pilote peut éventuellement retourner SQL_SUCCESS_WITH_INFO et 01S07 en cas de troncation fractionnaire.  
  
 Le pilote ignore la valeur de longueur/indicateur lors de la conversion de données à partir des types de données C numériques et suppose que la taille de la mémoire tampon de données correspond à la taille du type de données C numérique. La valeur de longueur/indicateur est passée dans l’argument *StrLen_Or_Ind* dans **SQLPutData** et dans la mémoire tampon spécifiée avec l’argument *StrLen_or_IndPtr* dans **SQLBindParameter**. La mémoire tampon de données est spécifiée avec l’argument *DataPtr* dans **SQLPutData** et l’argument *ParameterValuePtr* dans **SQLBindParameter**.
