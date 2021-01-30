---
description: 'C en SQL : Caractère'
title: 'C en SQL : caractère | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- character data type [ODBC]
- data conversions from C to SQL types [ODBC], character
- converting data from c to SQL types [ODBC], character
ms.assetid: be66188a-ebdb-4c9e-af72-c379886766fa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c387c2063bea8f16c7ec5ec90aa0271830f3e5f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212355"
---
# <a name="c-to-sql-character"></a>C en SQL : Caractère
Les identificateurs pour le type de données de caractères ODBC C sont les suivants :  
  
 SQL_C_CHAR  
  
 SQL_C_WCHAR  
  
 Le tableau suivant présente les types de données SQL ODBC dans lesquels les données de caractères C peuvent être converties. Pour obtenir une explication des colonnes et des termes du tableau, consultez [conversion de données de C en types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
> [!NOTE]  
>  Lorsque les données de caractères C sont converties en données SQL Unicode, la longueur des données Unicode doit être un nombre pair.  
  
|Identificateur de type SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Longueur en octets des données <= longueur de colonne.<br /><br /> Longueur en octets des données > longueur de colonne.|n/a<br /><br /> 22001|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Longueur de caractère des <de données = longueur de colonne.<br /><br /> Longueur de caractère de la longueur de la colonne > de données.|n/a<br /><br /> 22001|  
|SQL_DECIMAL<br /><br /> SQL_NUMERIC<br /><br /> SQL_TINYINT<br /><br /> SQL_SMALLINT<br /><br /> SQL_INTEGER SQL_BIGINT|Données converties sans troncation<br /><br /> Données converties avec troncation de chiffres fractionnaires [e]<br /><br /> La conversion de données entraînerait une perte de la totalité (par opposition aux chiffres fractionnaires) [e]<br /><br /> La valeur des données n’est pas un *littéral numérique*|n/a<br /><br /> 22001<br /><br /> 22001<br /><br /> 22018|  
|SQL_REAL<br /><br /> SQL_FLOAT<br /><br /> SQL_DOUBLE|Les données se trouvent dans la plage du type de données dans lequel le nombre est converti<br /><br /> Les données se trouvent en dehors de la plage du type de données dans lequel le nombre est converti<br /><br /> La valeur des données n’est pas un *littéral numérique*|n/a<br /><br /> 22003<br /><br /> 22018|  
|SQL_BIT|Les données sont 0 ou 1<br /><br /> Les données sont supérieures à 0, inférieures à 2 et ne sont pas égales à 1<br /><br /> Les données sont inférieures à 0 ou supérieures ou égales à 2<br /><br /> Les données ne sont pas un *littéral numérique*|n/a<br /><br /> 22001<br /><br /> 22003<br /><br /> 22018|  
|SQL_BINARY<br /><br /> SQL_VARBINARY<br /><br /> SQL_LONGVARBINARY|(Longueur d’octet des données)/2 <= longueur d’octet de colonne<br /><br /> (Longueur d’octet des données)/2 > longueur d’octet de colonne<br /><br /> La valeur des données n’est pas une valeur hexadécimale|n/a<br /><br /> 22001<br /><br /> 22018|  
|SQL_TYPE_DATE|La valeur des données est un *littéral ODBC-date* valide<br /><br /> La valeur des données est un *littéral ODBC-timestamp* valide ; la partie heure est égale à zéro<br /><br /> La valeur des données est un *littéral ODBC-timestamp* valide ; la partie heure est différente de zéro [a]<br /><br /> La valeur des données n’est pas un littéral *ODBC-date* ou un *littéral ODBC-timestamp*|n/a<br /><br /> n/a<br /><br /> 22008<br /><br /> 22018|  
|SQL_TYPE_TIME|La valeur des données est un *littéral d’heure ODBC* valide<br /><br /> La valeur des données est un *littéral ODBC-timestamp* valide ; la partie fractions de seconde est égale à zéro [b]<br /><br /> La valeur des données est un *littéral ODBC-timestamp* valide ; la partie fractions de seconde est différente de zéro [b]<br /><br /> La valeur des données n’est pas un littéral *ODBC-Time-Literal* ou *ODBC-timestamp-Literal* valide|n/a<br /><br /> n/a<br /><br /> 22008<br /><br /> 22018|  
|SQL_TYPE_TIMESTAMP|La valeur des données est un *littéral ODBC-timestamp* valide ; partie fractions de seconde non tronquée<br /><br /> La valeur des données est un *littéral ODBC-timestamp* valide ; fraction de seconde fractionnaire tronquée<br /><br /> La valeur des données est un *littéral ODBC-date* valide [c]<br /><br /> La valeur des données est un *littéral d’heure ODBC* valide [d]<br /><br /> La valeur des données n’est pas un littéral *ODBC-date-littéral*, *ODBC-Time-* Literal ou *ODBC-timestamp-Literal* valide|n/a<br /><br /> 22008<br /><br /> n/a<br /><br /> n/a<br /><br /> 22018|  
|Tous les types d’intervalle SQL|La valeur de données est une *valeur d’intervalle* valide ; aucune troncation ne se produit<br /><br /> La valeur de données est une *valeur d’intervalle* valide ; la valeur dans l’un des champs est tronquée<br /><br /> La valeur des données n’est pas un littéral d’intervalle valide|n/a<br /><br /> 22015<br /><br /> 22018|  
  
 [a] la partie heure de l’horodatage est tronquée.  
  
 [b] la partie Date de l’horodatage est ignorée.  
  
 [c] la partie heure de l’horodatage est définie à zéro.  
  
 [d] la partie Date de l’horodateur est définie sur la date actuelle.  
  
 [e] le pilote/la source de données attend efficacement jusqu’à ce que la totalité de la chaîne soit reçue (même si les données caractères sont envoyées en plusieurs parties par des appels à **SQLPutData**) avant d’essayer d’effectuer la conversion.  
  
 Lorsque les données de caractères C sont converties en données SQL numériques, de date, d’heure ou d’horodatage, les espaces de début et de fin sont ignorés.  
  
 Lorsque les données de caractères C sont converties en données binaires SQL, les deux octets de données de type caractère sont convertis en un seul octet (8 bits) de données binaires. Chacun des deux octets de données de type caractère représente un nombre au format hexadécimal. Par exemple, « 01 » est converti en binaire 00000001 et « FF » est converti en un binaire 11111111.  
  
 Le pilote convertit toujours les paires de chiffres hexadécimaux en octets individuels et ignore l’octet de fin null. Pour cette raison, si la longueur de la chaîne de caractères est impaire, le dernier octet de la chaîne (à l’exclusion de l’octet de fin null, le cas échéant) n’est pas converti.  
  
> [!NOTE]  
>  Les développeurs d’applications sont déconseillés de lier les données de caractères C à un type de données SQL binaire. Cette conversion est généralement inefficace et lente.
