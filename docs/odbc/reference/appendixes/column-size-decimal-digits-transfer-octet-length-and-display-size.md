---
description: Taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage-ODBC
title: Taille de la colonne, chiffres décimaux, longueur de l’octet de transfert, taille d’affichage | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- display size of data types [ODBC]
- data types [ODBC], column size
- transfer octet length of data types [ODBC]
- size of data types [ODBC]
- data types [ODBC], display size
- decimal digits of data types [ODBC]
- data types [ODBC], decimal digits
- SQL data types [ODBC], column characteristics
- column size of data types [ODBC]
- data types [ODBC], transfer octet length
ms.assetid: 723107a1-be08-4ea3-a8c0-b2c45d38d1aa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8a23183fd60d947771cb041af977d518157ac573
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182531"
---
# <a name="column-size-decimal-digits-transfer-octet-length-and-display-size---odbc"></a>Taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage-ODBC
Les types de données sont caractérisés par leur taille de colonne (ou de paramètre), les chiffres décimaux, la longueur et la taille d’affichage. Les fonctions ODBC suivantes retournent ces attributs pour un paramètre dans une instruction SQL ou pour un type de données SQL sur une source de données. Chaque fonction ODBC retourne un ensemble différent de ces attributs, comme suit :  
  
-   **SQLDescribeCol** retourne la taille de la colonne et les chiffres décimaux des colonnes qu’elle décrit.  
  
-   **SQLDescribeParam** retourne la taille du paramètre et les chiffres décimaux des paramètres qu’il décrit. **SQLBindParameter** définit la taille des paramètres et les chiffres décimaux d’un paramètre dans une instruction SQL.  
  
-   Les fonctions de catalogue **SQLColumns**, **SQLProcedureColumns** et **SQLGetTypeInfo** retournent des attributs pour une colonne dans une table, un jeu de résultats ou un paramètre de procédure, ainsi que les attributs de catalogue des types de données dans la source de données. **SQLColumns** retourne la taille de colonne, les chiffres décimaux et la longueur d’une colonne dans les tables spécifiées (par exemple, la table de base, la vue ou une table système). **SQLProcedureColumns** retourne la taille de colonne, les chiffres décimaux et la longueur d’une colonne dans une procédure. **SQLGetTypeInfo** retourne la taille de colonne maximale et les chiffres décimaux minimal et maximal d’un type de données SQL sur une source de données.  
  
 Les valeurs retournées par ces fonctions pour la taille de la colonne ou du paramètre correspondent à la « précision » telle que définie dans ODBC 2. *x*. Toutefois, les valeurs ne correspondent pas nécessairement aux valeurs retournées dans SQL_DESC_PRECISION ou dans un autre champ de descripteur. Il en va de même pour les chiffres décimaux, qui correspondent à « Scale » comme défini dans ODBC 2. *x*. Elle ne correspond pas nécessairement aux valeurs retournées dans SQL_DESC_SCALE ou tout autre champ de descripteur, mais provient de différents champs de descripteur en fonction du type de données. Pour plus d’informations, consultez [taille de colonne](../../../odbc/reference/appendixes/column-size.md) et [chiffres décimaux](../../../odbc/reference/appendixes/decimal-digits.md).  
  
 De même, les valeurs de la longueur octet de transfert ne proviennent pas de SQL_DESC_LENGTH. Ils proviennent de la SQL_DESC_OCTET_LENGTH d’un champ d’un descripteur pour tous les types caractère et binaire. Il n’existe aucun champ de descripteur qui contient ces informations pour d’autres types.  
  
 La valeur taille d’affichage pour tous les types de données correspond à la valeur dans un champ de descripteur unique, SQL_DESC_DISPLAY_SIZE.  
  
 Les champs de descripteur décrivent les caractéristiques d’un jeu de résultats. Les champs de descripteur ne contiennent pas de valeurs valides sur les données avant l’exécution de l’instruction. Les valeurs de taille de colonne, de chiffres décimaux et de taille d’affichage retournées par **SQLColumns**, **SQLProcedureColumns** et **SQLGetTypeInfo**, en revanche, renvoient les caractéristiques des objets de base de données, tels que les colonnes de table et les types de données, qui existent dans le catalogue de la source de données. De même, dans son jeu de résultats, **SQLColAttribute** retourne la taille de la colonne, les chiffres décimaux et la longueur en octets du transfert des colonnes au niveau de la source de données ; ces valeurs ne sont pas nécessairement les mêmes que les valeurs des champs de descripteur SQL_DESC_PRECISION, SQL_DESC_SCALE et SQL_DESC_OCTET_LENGTH.  
  
 Pour plus d’informations sur ces champs de descripteur, consultez [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).  
  
 Rubriques connexes :  
  
-   [Taille de colonne](../../../odbc/reference/appendixes/column-size.md)  
-   [Nombres décimaux](../../../odbc/reference/appendixes/decimal-digits.md)  
-   [Longueur en octets du transfert](../../../odbc/reference/appendixes/transfer-octet-length.md)  
-   [Taille d’affichage](../../../odbc/reference/appendixes/display-size.md)
