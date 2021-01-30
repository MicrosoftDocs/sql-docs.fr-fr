---
description: 'SQL à C : GUID'
title: 'SQL en C : GUID | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], GUID
- data conversions from SQL to C types [ODBC], guid
- GUID data type [ODBC]
ms.assetid: cf56c684-c261-4b89-994a-db14ab2241d6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9b55b88414dfa78b80c49987af6dab82ba4abeda
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203024"
---
# <a name="sql-to-c-guid"></a>SQL à C : GUID
L’identificateur pour le type de données SQL ODBC GUID est le suivant :  
  
 SQL_GUID  
  
 Le tableau suivant répertorie les types de données ODBC C dans lesquels les données SQL GUID peuvent être converties. Pour obtenir une explication des colonnes et des termes du tableau, consultez [conversion de données SQL en types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|Identificateur de type C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|*BufferLength* > la longueur en octets du caractère|Données|36|n/a|  
||*BufferLength* < 37|Indéfini|Indéfini|22003|  
|SQL_C_WCHAR|*BufferLength* > longueur de caractère|Données|36|n/a|  
||*BufferLength* < 37|Indéfini|Indéfini|22003|  
|SQL_C_BINARY|Longueur en octets des données \< =  *BufferLength*|Données|Longueur des données en octets|n/a|  
||Longueur en octets des données > *BufferLength*|Indéfini|Indéfini|22003|  
|SQL_C_GUID|Aucun [a]|Données|16 [b]|n/a|  
  
 [a] la valeur de *BufferLength* est ignorée pour cette conversion. Le pilote suppose que la taille de **TargetValuePtr* est la taille du type de données C.  
  
 [b] il s’agit de la taille du type de données C correspondant.
