---
description: Conversion de données de SQL en types de données C
title: Conversion de données des types de données SQL en C | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data conversions from SQL to C types [ODBC]
- data conversions from SQL to C types [ODBC], about converting
- data types [ODBC], C data types
- data types [ODBC], converting data
- data types [ODBC], SQL data types
- SQL data types [ODBC], converting to C types
- converting data from SQL to c types [ODBC]
- converting data from SQL to c types [ODBC], about converting
- C data types [ODBC], converting from SQL types
ms.assetid: 029727f6-d3f0-499a-911c-bcaf9714e43b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5cd8b0790426d9f3461d8ff1600471867ce94b55
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165374"
---
# <a name="converting-data-from-sql-to-c-data-types"></a>Conversion de données de SQL en types de données C
Quand une application appelle **SQLFetch**, **SQLFetchScroll** ou **SQLGetData**, le pilote récupère les données à partir de la source de données. Si nécessaire, il convertit les données du type de données dans lequel le pilote les a récupérées dans le type de données spécifié par l’argument *TargetType* dans **SQLBindCol** ou **SQLGetData.** Enfin, il stocke les données à l’emplacement désigné par l’argument *TargetValuePtr* dans **SQLBindCol** ou **SQLGetData** (et le champ SQL_DESC_DATA_PTR de ARD).  
  
 Le tableau suivant montre les conversions prises en charge des types de données SQL ODBC en types de données ODBC C. Un cercle rempli indique la conversion par défaut pour un type de données SQL (le type de données C dans lequel les données seront converties lorsque la valeur de *TargetType* est SQL_C_DEFAULT). Un cercle creux indique une conversion prise en charge.  
  
 Pour une application ODBC *3. x* fonctionnant avec un pilote ODBC *2. x* , la conversion à partir de types de données spécifiques au pilote peut ne pas être prise en charge.  
  
 Le format des données converties n’est pas affecté par le paramètre pays de l'® Windows.  
  
 Les tableaux des sections suivantes décrivent comment le pilote ou la source de données convertit les données extraites de la source de données ; les pilotes sont requis pour prendre en charge les conversions de tous les types de données ODBC C à partir des types de données SQL ODBC qu’ils prennent en charge. Pour un type de données SQL ODBC donné, la première colonne de la table répertorie les valeurs d’entrée autorisées de l’argument *TargetType* dans **SQLBindCol** et **SQLGetData**. La deuxième colonne répertorie les résultats d’un test, souvent en utilisant l’argument *BufferLength* spécifié dans **SQLBindCol** ou **SQLGetData**, que le pilote effectue pour déterminer s’il peut convertir les données. Pour chaque résultat, les troisième et quatrième colonnes répertorient les valeurs placées dans les mémoires tampons spécifiées par les arguments *TargetValuePtr* et *StrLen_or_IndPtr* spécifiés dans **SQLBindCol** ou **SQLGetData** après que le pilote a tenté de convertir les données. (L’argument *StrLen_or_IndPtr* correspond au champ SQL_DESC_OCTET_LENGTH_PTR de l’ARD.) La dernière colonne répertorie le SQLSTATE retourné pour chaque résultat par **SQLFetch**, **SQLFetchScroll** ou **SQLGetData**.  
  
 Si l’argument *TargetType* dans **SQLBindCol** ou **SQLGetData** contient un identificateur pour un type de données C ODBC non affiché dans la table pour un type de données SQL ODBC donné, **SQLFetch**, **SQLFetchScroll** ou **SQLGetData** retourne SQLSTATE 07006 (violation d’attribut de type de données restreint). Si l’argument *TargetType* contient un identificateur qui spécifie une conversion d’un type de données SQL spécifique au pilote en un type de données C ODBC et que cette conversion n’est pas prise en charge par le pilote, **SQLFetch**, **SQLFETCHSCROLL** ou **SQLGetData** retourne SQLState HYC00 (fonctionnalité facultative non implémentée).  
  
 Bien qu’il ne soit pas affiché dans les tables, le pilote retourne SQL_NULL_DATA dans la mémoire tampon spécifiée par l’argument *StrLen_or_IndPtr* lorsque la valeur de données SQL est null. Pour une explication de l’utilisation de *StrLen_or_IndPtr* lorsque plusieurs appels sont effectués pour récupérer des données, consultez la description de la fonction [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md). Lorsque les données SQL sont converties en données de type C, le nombre de caractères retourné dans \* *StrLen_or_IndPtr* n’inclut pas l’octet de fin null. Si *TargetValuePtr* est un pointeur null, **SQLGETDATA** retourne SQLState HY009 (utilisation non valide du pointeur null); dans **SQLBindCol**, la colonne est dissociée.  
  
 Les termes et conventions suivants sont utilisés dans les tables :  
  
-   La **longueur en octets des données** correspond au nombre d’octets de données C disponibles à retourner dans **TargetValuePtr*, que les données soient tronquées avant d’être retournées à l’application. Pour les données de type chaîne, cela n’inclut pas l’espace pour le caractère de fin null.  
  
-   La **longueur en octets de caractères** est le nombre total d’octets nécessaires pour afficher les données au format caractère. Cette valeur est définie pour chaque type de données C dans la section [taille d’affichage](../../../odbc/reference/appendixes/display-size.md), sauf que la longueur de l’octet de caractère est en octets alors que la taille d’affichage est en caractères.  
  
-   Les mots en *italiques* représentent des arguments de fonction ou des éléments de la grammaire SQL. Pour connaître la syntaxe des éléments de syntaxe, consultez [annexe C : grammaire SQL](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md).  
  
 Cette section contient les rubriques suivantes :  
  
-   [SQL à C : Caractère](../../../odbc/reference/appendixes/sql-to-c-character.md)  
  
-   [SQL à C : Numérique](../../../odbc/reference/appendixes/sql-to-c-numeric.md)  
  
-   [SQL à C : bit](../../../odbc/reference/appendixes/sql-to-c-bit.md)  
  
-   [SQL à C : Binary](../../../odbc/reference/appendixes/sql-to-c-binary.md)  
  
-   [SQL à C : Date](../../../odbc/reference/appendixes/sql-to-c-date.md)  
  
-   [SQL à C : GUID](../../../odbc/reference/appendixes/sql-to-c-guid.md)  
  
-   [SQL à C : Temps](../../../odbc/reference/appendixes/sql-to-c-time.md)  
  
-   [SQL à C : Timestamp](../../../odbc/reference/appendixes/sql-to-c-timestamp.md)  
  
-   [SQL à C : Intervalles d’années-mois](../../../odbc/reference/appendixes/sql-to-c-year-month-intervals.md)  
  
-   [SQL à C : Intervalles de jours-heures](../../../odbc/reference/appendixes/sql-to-c-day-time-intervals.md)  
  
-   [Exemples de conversion de données SQL en C](../../../odbc/reference/appendixes/sql-to-c-data-conversion-examples.md)
