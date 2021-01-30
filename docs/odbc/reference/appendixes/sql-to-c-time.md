---
description: 'SQL à C : Temps'
title: 'SQL en C : heure | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], time
- time data type [ODBC]
- data conversions from SQL to C types [ODBC], time
ms.assetid: 6dc59973-7bb5-40f1-87c8-5bf68b3bf2ee
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: db1882befdb1b295ce247cb62601bbec4ba3db5f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203004"
---
# <a name="sql-to-c-time"></a>SQL à C : Temps
L’identificateur pour le type de données SQL ODBC Time est le suivant :  
  
 SQL_TYPE_TIME  
  
 Le tableau suivant répertorie les types de données ODBC C dans lesquels les données SQL peuvent être converties. Pour obtenir une explication des colonnes et des termes du tableau, consultez [conversion de données SQL en types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|Identificateur de type C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|*BufferLength* > la longueur en octets du caractère<br /><br /> *9*  <=  *BufferLength* <= longueur de l’octet du caractère<br /><br /> *BufferLength* < 9|Données<br /><br /> Données tronquées [a]<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Longueur des données en octets<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|*BufferLength* > longueur de caractère<br /><br /> *9*  <=  *BufferLength* <= longueur de caractère<br /><br /> *BufferLength* < 9|Données<br /><br /> Données tronquées [a]<br /><br /> Indéfini|Longueur des données en caractères<br /><br /> Longueur des données en caractères<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_BINARY|Longueur en octets des données <= *BufferLength*<br /><br /> Longueur en octets des données > *BufferLength*|Données<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Indéfini|n/a<br /><br /> 22003|  
|SQL_C_TYPE_TIME|Aucun [b]|Données|6 [d]|n/a|  
|SQL_C_TYPE_TIMESTAMP|Aucun [b]|Données (c)|16 [d]|n/a|  
  
 [a] les fractions de seconde de l’heure sont tronquées.  
  
 [b] la valeur de *BufferLength* est ignorée pour cette conversion. Le pilote suppose que la taille de **TargetValuePtr* est la taille du type de données C.  
  
 [c] les champs de date de la structure d’horodatage sont définis sur la date du jour et le champ fractions de seconde de la structure d’horodatage est défini sur zéro.  
  
 [d] il s’agit de la taille du type de données C correspondant.  
  
 Quand les données SQL sont converties en données de type C, la chaîne résultante est au format «*hh*:*mm*:*SS*». Ce format n’est pas affecté par le paramètre pays® Windows.
