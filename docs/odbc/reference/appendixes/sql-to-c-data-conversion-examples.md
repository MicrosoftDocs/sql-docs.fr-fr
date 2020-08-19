---
description: Exemples de conversion de données SQL en C
title: Exemples de conversion de données SQL en C | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- data conversions from SQL to C types [ODBC], examples
- converting data from SQL to C types [ODBC], examples
ms.assetid: 0190c76c-7f9b-42f4-be9d-cef7284840fd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a7a3d70a6f74a814262ddad580b5e5a2b0d79927
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88429581"
---
# <a name="sql-to-c-data-conversion-examples"></a>Exemples de conversion de données SQL en C

Les exemples présentés dans le tableau suivant illustrent la façon dont le pilote convertit les données SQL en données C :  
  
|Type SQL<br /><br /> identificateur|Données SQL<br /><br /> value|Type C<br /><br /> identificateur|Buffer<br /><br /> length|**TargetValuePtr*|SQLSTATE|  
|-----------------------------|------------------------|---------------------------|-----------------------|------------------------|--------------|  
|SQL_CHAR|abcdef|SQL_C_CHAR|7|abcdef\0 [a]|n/a|  
|SQL_CHAR|abcdef|SQL_C_CHAR|6|abcde\0 [a]|01004|  
|SQL_DECIMAL|1234,56|SQL_C_CHAR|8|1234,56 \ 0 [a]|n/a|  
|SQL_DECIMAL|1234,56|SQL_C_CHAR|5|1234 \ 0 [a]|01004|  
|SQL_DECIMAL|1234,56|SQL_C_CHAR|4|----|22003|  
|SQL_DECIMAL|1234,56|SQL_C_FLOAT|ignoré|1234,56|n/a|  
|SQL_DECIMAL|1234,56|SQL_C_SSHORT|ignoré|1234|01S07|  
|SQL_DECIMAL|1234,56|SQL_C_STINYINT|ignoré|----|22003|  
|SQL_DOUBLE|1,2345678|SQL_C_DOUBLE|ignoré|1,2345678|n/a|  
|SQL_DOUBLE|1,2345678|SQL_C_FLOAT|ignoré|1,234567|n/a|  
|SQL_DOUBLE|1,2345678|SQL_C_STINYINT|ignoré|1|n/a|  
|SQL_TYPE_DATE|1992-12-31|SQL_C_CHAR|11|1992-12-31 \ 0 [a]|n/a|  
|SQL_TYPE_DATE|1992-12-31|SQL_C_CHAR|10|-----|22003|  
|SQL_TYPE_DATE|1992-12-31|SQL_C_TIMESTAMP|ignoré|1992, 12, 31, 0, 0, 0, 0 [b]|n/a|  
|SQL_TYPE_TIMESTAMP|1992-12-31 23:45:55.12|SQL_C_CHAR|23|1992-12-31 23:45:55.12 \ 0 [a]|n/a|  
|SQL_TYPE_TIMESTAMP|1992-12-31 23:45:55.12|SQL_C_CHAR|22|1992-12-31 23:45:55.1 \ 0 [a]|01004|  
|SQL_TYPE_TIMESTAMP|1992-12-31 23:45:55.12|SQL_C_CHAR|18|----|22003|  
  
 [a] « \ 0 » représente un octet de fin null. Le pilote se termine toujours par un caractère null SQL_C_CHAR données.  
  
 [b] les nombres de cette liste sont les nombres stockés dans les champs de la structure TIMESTAMP_STRUCT.
