---
description: 'SQL à C : Caractère'
title: 'SQL en C : caractère | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], character
- character data type [ODBC]
- data conversions from SQL to C types [ODBC], character
ms.assetid: 7fdb7f38-b64d-48f2-bcb4-1ca96b2bbdb6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 78bbf45362216f8fb2a0706977347219ac6ac28b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203069"
---
# <a name="sql-to-c-character"></a>SQL à C : Caractère

Les identificateurs des types de données SQL ODBC de caractères sont les suivants :

- SQL_CHAR
- SQL_VARCHAR
- SQL_LONGVARCHAR
- SQL_WCHAR
- SQL_WVARCHAR
- SQL_WLONGVARCHAR

Le tableau suivant répertorie les types de données ODBC C dans lesquels les données SQL peuvent être converties. Pour obtenir une explication des colonnes et des termes du tableau, consultez [conversion de données SQL en types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  

|Identificateur de type C|Test|TargetValuePtr|StrLen_or_IndPtr|SQLSTATE|
|:----------------|:---|:-------------|:---------------|:-------|
|SQL_C_CHAR|Longueur en octets des données < *BufferLength*<br /><br /> Longueur en octets des données >= *BufferLength*|Données<br /><br /> Données tronquées|Longueur des données en octets<br /><br /> Longueur des données en octets|n/a<br /><br /> 01004|  
|SQL_C_WCHAR|Longueur de caractère des données < *BufferLength*<br /><br /> Longueur de caractère des >de données = *BufferLength*|Données<br /><br /> Données tronquées|Longueur des données en caractères<br /><br /> Longueur des données en caractères|n/a<br /><br /> 01004|  
|SQL_C_STINYINT SQL_C_UTINYINT SQL_C_TINYINT SQL_C_SBIGINT SQL_C_UBIGINT SQL_C_SSHORT SQL_C_USHORT SQL_C_SHORT SQL_C_SLONG SQL_C_ULONG SQL_C_LONG SQL_C_NUMERIC|Données converties sans troncation [b]<br /><br /> Données converties avec troncation de chiffres fractionnaires [a]<br /><br /> La conversion de données entraînerait la perte de chiffres entiers (par opposition aux chiffres fractionnaires) [a]<br /><br /> Data n’est pas un *littéral numérique*[b]|Données<br /><br /> Données tronquées<br /><br /> Indéfini<br /><br /> Indéfini|Nombre d’octets du type de données C<br /><br /> Nombre d’octets du type de données C<br /><br /> Indéfini<br /><br /> Indéfini|n/a<br /><br /> 01S07<br /><br /> 22003<br /><br /> 22018|  
|SQL_C_FLOAT SQL_C_DOUBLE|Les données se trouvent dans la plage du type de données dans lequel le nombre est converti [a]<br /><br /> Les données se trouvent en dehors de la plage du type de données dans lequel le nombre est converti [a]<br /><br /> Data n’est pas un *littéral numérique*[b]|Données<br /><br /> Indéfini<br /><br /> Indéfini|Taille du type de données C<br /><br /> Indéfini<br /><br /> Indéfini|n/a<br /><br /> 22003<br /><br /> 22018|  
|SQL_C_BIT|Les données sont 0 ou 1<br /><br /> Les données sont supérieures à 0, inférieures à 2 et ne sont pas égales à 1<br /><br /> Les données sont inférieures à 0 ou supérieures ou égales à 2<br /><br /> Les données ne sont pas un *littéral numérique*|Données<br /><br /> Données tronquées<br /><br /> Indéfini<br /><br /> Indéfini|1 [b]<br /><br /> 1 [b]<br /><br /> Indéfini<br /><br /> Indéfini|n/a<br /><br /> 01S07<br /><br /> 22003<br /><br /> 22018|  
|SQL_C_BINARY|Longueur en octets des données <= *BufferLength*<br /><br /> Longueur en octets des données > *BufferLength*|Données<br /><br /> Données tronquées|Longueur des données en octets<br /><br /> Longueur des données|n/a<br /><br /> 01004|  
|SQL_C_TYPE_DATE|La valeur de données est une *valeur de date* valide [a]<br /><br /> La valeur de données est une *valeur timestamp* valide ; la partie heure est égale à zéro [a]<br /><br /> La valeur de données est une *valeur timestamp* valide ; la partie heure est différente de zéro [a], [c]<br /><br /> La valeur des données n’est pas une valeur de *Date* ou d' *horodatage* valide [a]|Données<br /><br /> Données<br /><br /> Données tronquées<br /><br /> Indéfini|6 [b]<br /><br /> 6 [b]<br /><br /> 6 [b]<br /><br /> Indéfini|n/a<br /><br /> n/a<br /><br /> 01S07<br /><br /> 22018|  
|SQL_C_TYPE_TIME|La valeur de données est une valeur d' *heure valide et la valeur de fractions de seconde est 0*[a]<br /><br /> La valeur de données est une valeur *timestamp valide ou une valeur temporelle valide*; la partie fractions de seconde est égale à zéro [a], [d]<br /><br /> La valeur de données est une *valeur timestamp* valide ; la partie fractions de seconde est différente de zéro [a], [d], [e]<br /><br /> La valeur des données n’est pas une valeur d' *heure* ou d' *horodatage* valide [a]|Données<br /><br /> Données<br /><br /> Données tronquées<br /><br /> Indéfini|6 [b]<br /><br /> 6 [b]<br /><br /> 6 [b]<br /><br /> Indéfini|n/a<br /><br /> n/a<br /><br /> 01S07<br /><br /> 22018|  
|SQL_C_TYPE_TIMESTAMP|La valeur de données est une valeur *timestamp valide ou une valeur temporelle valide*; partie fractions de seconde non tronquées [a]<br /><br /> La valeur de données est une valeur *timestamp valide ou une valeur temporelle valide*; portion de fractions de seconde tronquées [a]<br /><br /> La valeur de données est une *valeur de date* valide [a]<br /><br /> La valeur de données est une *valeur de temps* valide [a]<br /><br /> La valeur des données n’est pas une valeur de *Date*, d' *heure* ou d' *horodatage* valide [a]|Données<br /><br /> Données tronquées<br /><br /> Données [f]<br /><br /> Données [g]<br /><br /> Indéfini|16 [b]<br /><br /> 16 [b]<br /><br /> 16 [b]<br /><br /> 16 [b]<br /><br /> Indéfini|n/a<br /><br /> 01S07<br /><br /> n/a<br /><br /> n/a<br /><br /> 22018|  
|Tous les types d’intervalle C|La valeur de données est une *valeur d’intervalle* valide ; aucune troncation<br /><br /> La valeur de données est une *valeur d’intervalle* valide ; troncation d’un ou plusieurs champs de fin<br /><br /> L’intervalle de données est valide ; la précision significative du champ de début est perdue<br /><br /> La valeur de données n’est pas une valeur d’intervalle valide|Données<br /><br /> Données tronquées<br /><br /> Indéfini<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Longueur des données en octets<br /><br /> Indéfini<br /><br /> Indéfini|n/a<br /><br /> 01S07<br /><br /> 22015<br /><br /> 22018|  
|&nbsp;|&nbsp;|&nbsp;|&nbsp;|&nbsp;|

 [a] la valeur de *BufferLength* est ignorée pour cette conversion. Le pilote suppose que la taille de **TargetValuePtr* est la taille du type de données C.  
  
 [b] il s’agit de la taille du type de données C correspondant.  
  
 [c] la partie heure de la *valeur timestamp* est tronquée.  
  
 [d] la partie Date de la *valeur timestamp* est ignorée.  
  
 [e] la partie fractions de seconde de l’horodatage est tronquée.  
  
 [f] les champs d’heure de la structure d’horodatage sont définis sur zéro.  
  
 [g] les champs de date de la structure d’horodatage sont définis sur la date actuelle.  

**Espaces supplémentaires**

Les espaces de début et de fin sont ignorés quand les données de caractères SQL sont converties dans l’un des types suivants :

- date
- numeric
- time
- timestamp
- données de l’intervalle C
