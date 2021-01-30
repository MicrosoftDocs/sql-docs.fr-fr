---
description: 'SQL à C : Binary'
title: 'SQL en C : binaire | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to c types [ODBC], binary
- binary data type [ODBC]
- data conversions from SQL to C types [ODBC], binary
- binary data transfers [ODBC]
ms.assetid: 8c519072-ae4c-4d32-9d4e-775e3d3d6389
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 30fbabde654f69af7a0cb68e2606c0f2d93697c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160815"
---
# <a name="sql-to-c-binary"></a>SQL à C : Binary
Les identificateurs des types de données SQL ODBC binaires sont les suivants :  
  
 SQL_BINARY  
  
 SQL_VARBINARY  
  
 SQL_LONGVARBINARY  
  
 Le tableau suivant répertorie les types de données ODBC C dans lesquels les données SQL binaires peuvent être converties. Pour obtenir une explication des colonnes et des termes du tableau, consultez [conversion de données SQL en types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|Identificateur de type C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|(Longueur en octets des données) \* 2 < *BufferLength*<br /><br /> (Longueur en octets des données) \* 2 >= *BufferLength*|Données<br /><br /> Données tronquées|Longueur des données en octets<br /><br /> Longueur des données en octets|n/a<br /><br /> 01004|  
|SQL_C_WCHAR|(Longueur de caractère des données) \* 2 < *BufferLength*<br /><br /> (Longueur de caractère des données) \* 2 >= *BufferLength*|Données<br /><br /> Données tronquées|Longueur des données en caractères<br /><br /> Longueur des données en caractères|n/a<br /><br /> 01004|  
|SQL_C_BINARY|Longueur en octets des données <= *BufferLength*<br /><br /> Longueur en octets des données > *BufferLength*|Données<br /><br /> Données tronquées|Longueur des données en octets<br /><br /> Longueur des données en octets|n/a<br /><br /> 01004|  
  
 Lorsque les données SQL binaires sont converties en données de type C, chaque octet (8 bits) des données sources est représenté sous la forme de deux caractères ASCII. Ces caractères sont la représentation de caractères ASCII du nombre sous sa forme hexadécimale. Par exemple, un binaire 00000001 est converti en « 01 » et un binaire 11111111 est converti en « FF ».  
  
 Le pilote convertit toujours les octets individuels en paires de chiffres hexadécimaux et met fin à la chaîne de caractères avec un octet NULL. Pour cette raison, si *BufferLength* est pair et est inférieur à la longueur des données converties, le dernier octet de la mémoire tampon **TargetValuePtr* n’est pas utilisé. (Les données converties requièrent un nombre pair d’octets, le prochain octet est un octet NULL et le dernier octet ne peut pas être utilisé.)  
  
> [!NOTE]  
>  Les développeurs d’applications ne sont pas déconseillés de lier les données SQL binaires à un type de données C caractère. Cette conversion est généralement inefficace et lente.
