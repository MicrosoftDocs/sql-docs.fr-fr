---
description: 'Annexe E : Fonctions scalaires'
title: 'Annexe E : fonctions scalaires | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQL-92 functions [ODBC]
- scalar functions [ODBC]
- functions [ODBC], scalar
ms.assetid: 59c7cd5e-32d6-43ab-bac3-7010322d105a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 94ad0914477de0e2dfd25667755757bbc36a8b29
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212539"
---
# <a name="appendix-e-scalar-functions"></a>Annexe E : Fonctions scalaires
ODBC spécifie les types suivants de fonctions scalaires, avec des informations détaillées sur chacun de ces types de fonction fournis dans les sections correspondantes de cette annexe. Les descriptions de fonction incluent une syntaxe associée.  
  
 Cette annexe contient les rubriques suivantes.  
  
-   [Fonctions de chaîne](../../../odbc/reference/appendixes/string-functions.md)  
  
-   [Fonctions numériques](../../../odbc/reference/appendixes/numeric-functions.md)  
  
-   [Fonctions d'heure, de date et d'intervalle](../../../odbc/reference/appendixes/time-date-and-interval-functions.md)  
  
-   [Fonctions système](../../../odbc/reference/appendixes/system-functions.md)  
  
-   [Fonctions de conversion de types de données explicites](../../../odbc/reference/appendixes/explicit-data-type-conversion-function.md)  
  
-   [CAST (SQL-92), fonction](../../../odbc/reference/appendixes/sql-92-cast-function.md)  
  
 ODBC n’impose pas de type de données pour les valeurs de retour des fonctions scalaires, car les fonctions sont souvent spécifiques à la source de données. Dans la mesure du possible, les applications doivent utiliser la fonction de conversion scalaire pour forcer la conversion des types de données.  
  
## <a name="odbc-and-sql-92-scalar-functions"></a>Fonctions scalaires ODBC et SQL-92  
 Les tableaux de cette annexe incluent des fonctions qui ont été ajoutées dans ODBC 3,0 pour s’aligner sur SQL-92. Les fonctions ajoutées pour un type particulier de fonction scalaire, telle que définie dans ODBC, sont indiquées dans chaque section.  
  
 ODBC et SQL-92 classent leurs fonctions scalaires différemment. ODBC classe les fonctions scalaires par type d’argument ; SQL-92 les classe par valeur de retour. Par exemple, la fonction EXTRACT est classée en tant que fonction timedate par ODBC, car l’argument Extract-Field est un mot clé DateTime et l’argument Extract-source est une expression datetime ou Interval. SQL-92, en revanche, classifie l’extraction comme une fonction scalaire numérique, car la valeur de retour est une valeur numérique.  
  
 Une application peut déterminer les fonctions scalaires prises en charge par un pilote en appelant **SQLGetInfo**. Les types d’informations sont inclus à la fois pour ODBC et pour les classifications SQL-92 des fonctions scalaires. Étant donné que ces classifications sont différentes, la prise en charge de certaines fonctions scalaires peut être indiquée dans les types d’informations qui ne correspondent pas à ODBC et SQL-92. Par exemple, la prise en charge de l’extraction dans ODBC est indiquée par le type d’informations SQL_TIMEDATE_FUNCTIONS ; la prise en charge de l’extraction dans SQL-92, en revanche, est indiquée par le type d’informations SQL_SQL92_NUMERIC_VALUE_FUNCTIONS.
