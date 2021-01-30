---
description: 'SQL à C : Timestamp'
title: 'SQL en C : horodateur | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- timestamp data type [ODBC]
- converting data from SQL to C types [ODBC], timestamp
- data conversions from SQL to C types [ODBC], timestamp
ms.assetid: 6a0617cf-d8c0-4316-8bb4-e6ddb45d7bf1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1d7881ad4ef9280ab9f2e7ac0d941ae2228b21b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202999"
---
# <a name="sql-to-c-timestamp"></a>SQL à C : Timestamp

L’identificateur du type de données SQL ODBC timestamp est le suivant :

- SQL_TYPE_TIMESTAMP  

Le tableau suivant répertorie les types de données ODBC C dans lesquels les données SQL timestamp peuvent être converties. Pour obtenir une explication des colonnes et des termes du tableau, consultez [conversion de données SQL en types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  

|Identificateur de type C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|*BufferLength* > la longueur en octets du caractère<br /><br /> 20 <= *BufferLength* <= longueur d’octet du caractère<br /><br /> *BufferLength* < 20|Données<br /><br /> Données tronquées [b]<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Longueur des données en octets<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|*BufferLength* > longueur de caractère<br /><br /> 20 <= *BufferLength* <= longueur de caractère<br /><br /> *BufferLength* < 20|Données<br /><br /> Données tronquées [b]<br /><br /> Indéfini|Longueur des données en caractères<br /><br /> Longueur des données en caractères<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_BINARY|Longueur en octets des données <= *BufferLength*<br /><br /> Longueur en octets des données > *BufferLength*|Données<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Indéfini|n/a<br /><br /> 22003|  
|SQL_C_TYPE_DATE|La partie heure de l’horodatage est égale à zéro [a]<br /><br /> La partie heure de l’horodatage est différente de zéro [a]|Données<br /><br /> Données tronquées (c)|6 [f]<br /><br /> 6 [f]|n/a<br /><br /> 01S07|  
|SQL_C_TYPE_TIME|La partie fractions de seconde de l’horodateur est égale à zéro [a]<br /><br /> La partie fractions de seconde de l’horodateur est différente de zéro [a]|Données [d]<br /><br /> Données tronquées [d], [e]|6 [f]<br /><br /> 6 [f]|n/a<br /><br /> 01S07|  
|SQL_C_TYPE_TIMESTAMP|La partie fractions de seconde de l’horodatage n’est pas tronquée [a]<br /><br /> La partie fractions de seconde de l’horodateur est tronquée [a]|Données [e]<br /><br /> Données tronquées [e]|16 [f]<br /><br /> 16 [f]|n/a<br /><br /> 01S07|  

 [a] la valeur de *BufferLength* est ignorée pour cette conversion. Le pilote suppose que la taille de **TargetValuePtr* est la taille du type de données C.  
  
 [b] les fractions de seconde de l’horodatage sont tronquées.  
  
 [c] la partie heure de l’horodatage est tronquée.  
  
 [d] la partie Date de l’horodateur est ignorée.  
  
 [e] la partie fractions de seconde de l’horodatage est tronquée.  
  
 [f] il s’agit de la taille du type de données C correspondant.  

Lorsque les données SQL timestamp sont converties en données de type C, la chaîne résultante est au format «*yyyy* - *mm* - *DD* *hh*:*mm*:*SS*[.*f...*]», où il est possible d’utiliser jusqu’à neuf chiffres pour les fractions de seconde. Ce format n’est pas affecté par le paramètre pays® Windows. (À l’exception de la virgule décimale et des fractions de seconde, le format entier doit être utilisé, quelle que soit la précision du type de données SQL timestamp.)
