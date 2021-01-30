---
description: 'SQL à C : Intervalles d’années-mois'
title: 'SQL en C : intervalles de Year-Month | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to c types [ODBC], about converting
- data conversions from SQL to C types [ODBC], year-month intervals
- intervals [ODBC], converting
- year-month intervals [ODBC]
ms.assetid: 1233634b-8214-420f-b872-3b2630105ba4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9d83ca61aa30ce1abbeb38759557f4b75a6e0b9a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202989"
---
# <a name="sql-to-c-year-month-intervals"></a>SQL à C : Intervalles d’années-mois

Les identificateurs pour les types de données SQL ODBC de l’intervalle année-mois sont les suivants :

- SQL_INTERVAL_MONTH
- SQL_INTERVAL_YEAR
- SQL_INTERVAL_YEAR_TO_MONTH

Le tableau suivant répertorie les types de données ODBC C dans lesquels les données SQL de l’année-année peuvent être converties. Pour obtenir une explication des colonnes et des termes du tableau, consultez [conversion de données SQL en types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  

|Identificateur de type C|Test|TargetValuePtr|StrLen_or_IndPtr|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_INTERVAL_MONTH [a]<br /><br /> SQL_C_INTERVAL_YEAR [a]<br /><br /> SQL_C_INTERVAL_YEAR_TO_MONTH [a]|La partie des champs de fin n’est pas tronquée<br /><br /> Partie des champs de fin tronquée<br /><br /> La précision de pointe de la cible n’est pas assez grande pour contenir les données de la source|Données<br /><br /> Données tronquées<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Longueur des données en octets<br /><br /> Indéfini|n/a<br /><br /> 01S07<br /><br /> 22015|  
|SQL_C_STINYINT [b]<br /><br /> SQL_C_UTINYINT [b]<br /><br /> SQL_C_USHORT [b]<br /><br /> SQL_C_SHORT [b]<br /><br /> SQL_C_SLONG [b]<br /><br /> SQL_C_ULONG [b]<br /><br /> SQL_C_NUMERIC [b]<br /><br /> SQL_C_BIGINT [b]|La précision de l’intervalle était un champ unique et les données ont été converties sans troncation<br /><br /> La précision de l’intervalle était un champ unique et tronqué dans son ensemble<br /><br /> La précision de l’intervalle n’est pas un champ unique|Données<br /><br /> Données tronquées<br /><br /> Indéfini|Taille du type de données C<br /><br /> Longueur des données en octets<br /><br /> Taille du type de données C|n/a<br /><br /> 22003<br /><br /> 22015|  
|SQL_C_BINARY|Longueur en octets des données <= *BufferLength*<br /><br /> Longueur en octets des données > *BufferLength*|Données<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Indéfini|n/a<br /><br /> 22003|  
|SQL_C_CHAR|Longueur en octets < *BufferLength*<br /><br /> Nombre de chiffres entiers (par opposition à fractionnaires) < *BufferLength*<br /><br /> Nombre de chiffres entiers (par opposition à fractionnaires) >= *BufferLength*|Données<br /><br /> Données tronquées<br /><br /> Indéfini|Taille du type de données C<br /><br /> Taille du type de données C<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|Longueur de caractère < *BufferLength*<br /><br /> Nombre de chiffres entiers (par opposition à fractionnaires) < *BufferLength*<br /><br /> Nombre de chiffres entiers (par opposition à fractionnaires) >= *BufferLength*|Données<br /><br /> Données tronquées<br /><br /> Indéfini|Taille du type de données C<br /><br /> Taille du type de données C<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
  
 [a] un type SQL d’intervalle d’un mois de l’année peut être converti en type d’intervalle C d’un mois de l’année.  
  
 [b] si la précision de l’intervalle est un champ unique (un de l’année ou du mois), le type SQL de l’intervalle peut être converti en tout nombre exact (SQL_C_STINYINT, SQL_C_UTINYINT, SQL_C_USHORT, SQL_C_SHORT, SQL_C_SLONG, SQL_C_ULONG ou SQL_C_NUMERIC).  

## <a name="default-conversions"></a>Conversions par défaut

La conversion par défaut d’un type SQL interval est vers le type de données de l’intervalle C correspondant. L’application lie ensuite la colonne ou le paramètre (ou définit le champ SQL_DESC_DATA_PTR dans l’enregistrement approprié du ARD) pour pointer vers la structure de SQL_INTERVAL_STRUCT initialisée (ou passe un pointeur vers la structure de INTERVAL_STRUCT SQL_ en tant qu’argument *TargetValuePtr* dans un appel à **SQLGetData**).
