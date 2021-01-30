---
description: 'Annexe D : Types de données'
title: 'Annexe D : types de données | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- C data types [ODBC], defined
- SQL data types [ODBC], defined
- data types [ODBC]
- data types [ODBC], about data types
ms.assetid: 981d49c3-3531-4543-aa75-5bd9e4f67000
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b2ff95858f654bef654b18105b4fda52c77cca0c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212544"
---
# <a name="appendix-d-data-types"></a>Annexe D : Types de données
ODBC définit deux ensembles de types de données : les types de données SQL et les types de données C. Les types de données SQL indiquent le type de données des données stockées dans la source de données. Les types de données C indiquent le type de données des données stockées dans des mémoires tampons d’application.  
  
 Les types de données SQL sont définis par chaque SGBD conformément à la norme SQL-92. Pour chaque type de données SQL spécifié dans la norme SQL-92, ODBC définit un identificateur de type, qui est une **#define** valeur transmise en tant qu’argument dans les fonctions ODBC ou renvoyée dans les métadonnées d’un jeu de résultats. Les seuls types de données SQL-92 non pris en charge par ODBC sont BIT (le type de SQL_BIT ODBC a des caractéristiques différentes), BIT_VARYING, TIME_WITH_TIMEZONE, TIMESTAMP_WITH_TIMEZONE et NATIONAL_CHARACTER. Les pilotes sont chargés de mapper des types de données SQL spécifiques à la source de données à des identificateurs de type de données SQL ODBC et à des identificateurs de type de données SQL spécifiques au pilote. Le type de données SQL est spécifié dans le champ SQL_DESC_CONCISE_TYPE d’un descripteur d’implémentation.  
  
 ODBC définit les types de données C et leurs identificateurs de type ODBC correspondants. Une application spécifie le type de données C de la mémoire tampon qui recevra les données du jeu de résultats en passant l’identificateur de type C approprié dans l’argument *TargetType* dans un appel à **SQLBindCol** ou **SQLGetData**. Elle spécifie le type C de la mémoire tampon contenant un paramètre d’instruction en passant l’identificateur de type C approprié dans l’argument *ValueType* dans un appel à **SQLBindParameter**. Le type de données C est spécifié dans le champ SQL_DESC_CONCISE_TYPE d’un descripteur d’application.  
  
> [!NOTE]  
>  Il n’existe aucun type de données C propre au pilote.  
  
 Chaque type de données SQL correspond à un type de données C ODBC. Avant de renvoyer des données à partir de la source de données, le pilote le convertit en type de données C spécifié. Avant d’envoyer des données à la source de données, le pilote le convertit à partir du type de données C spécifié.  
  
 Cette annexe contient les rubriques suivantes.  
  
-   [Utilisation d’identificateurs de types de données](../../../odbc/reference/appendixes/using-data-type-identifiers.md)  
  
-   [Types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md)  
  
-   [Type de données C](../../../odbc/reference/appendixes/c-data-types.md)  
  
-   [Identificateurs et descripteurs des types de données](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md)  
  
-   [Identificateurs des pseudo-types](../../../odbc/reference/appendixes/pseudo-type-identifiers.md)  
  
-   [Transfert de données dans leur forme binaire](../../../odbc/reference/appendixes/transferring-data-in-its-binary-form.md)  
  
-   [Conseils pour les types de données d’intervalle et numériques](../../../odbc/reference/appendixes/guidelines-for-interval-and-numeric-data-types.md)  
  
-   [Contraintes du calendrier grégorien](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md)  
  
-   [Taille de colonne, nombres décimaux, longueur en octets du transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md)  
  
-   [Conversion de données de SQL en types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md)  
  
-   [Conversion de données de C en types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)  
  
 Pour obtenir une explication des types de données ODBC, consultez [types de données dans ODBC](../../../odbc/reference/develop-app/data-types-in-odbc.md). Pour plus d’informations sur les types de données SQL spécifiques au pilote, consultez la documentation du pilote.
