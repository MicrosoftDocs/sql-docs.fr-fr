---
description: Littéraux d’intervalle
title: Littéraux d’intervalle | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], interval data types
- interval literals [ODBC]
- interval data type [ODBC], literals
ms.assetid: f9e6c3c7-4f98-483f-89d8-ebc5680f021b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1e100fcc54f9ca6cc165eece7637b7fae45f65b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184415"
---
# <a name="interval-literals"></a>Littéraux d’intervalle
ODBC exige que tous les pilotes prennent en charge la conversion du type de données SQL_CHAR ou SQL_VARCHAR en tous les types de données de l’intervalle C. Toutefois, si la source de données sous-jacente ne prend pas en charge les types de données d’intervalle, le pilote doit connaître le format correct de la valeur dans le champ SQL_CHAR afin de prendre en charge ces conversions. De même, ODBC exige que tout type ODBC C soit convertible en SQL_CHAR ou SQL_VARCHAR, de sorte qu’un pilote doit savoir quel format un intervalle stocké dans le champ de caractère doit avoir. Cette section décrit la syntaxe des littéraux d’intervalle, que le writer de pilote doit utiliser pour valider les champs de SQL_CHAR lors de la conversion vers ou à partir des types de données de l’intervalle C.  
  
> [!NOTE]  
>  La syntaxe BNF complète pour les littéraux d’intervalle est indiquée dans la [syntaxe de littéral d’intervalle](../../../odbc/reference/appendixes/interval-literal-syntax.md) de section dans l’annexe C : grammaire SQL.  
  
 Pour passer des littéraux d’intervalle dans le cadre d’une instruction SQL, une syntaxe de clause d’échappement est définie pour les littéraux d’intervalle. Pour plus d’informations, consultez [littéraux de date, d’heure et d’horodatage](../../../odbc/reference/develop-app/date-time-and-timestamp-literals.md).  
  
 Un littéral d’intervalle se présente sous la forme suivante :  
  
```  
INTERVAL[<sign>] 'value' <interval qualifier>  
```  
  
 où « INTERVAL » indique que le littéral de caractère est un intervalle. Le signe peut être plus ou moins ; Il est en dehors de la chaîne d’intervalle et est facultatif.  
  
 Le qualificateur Interval peut être un champ DateTime unique ou être composé de deux champs DateTime, sous la forme : \<*leading field*> à \<*trailing field*> .  
  
-   Lorsque l’intervalle est composé d’un seul champ, le champ unique peut être un champ qui n’est pas un second, qui peut être accompagné d’une précision de début facultative entre parenthèses. Le champ DateTime unique peut également être un deuxième champ qui peut être accompagné de la précision de début facultative, de la précision de fraction de seconde facultative entre parenthèses, ou des deux. Si une précision de début et une précision de fraction de seconde sont présentes pour un champ de secondes, elles sont séparées par des virgules. Si le champ secondes a une précision de fraction de seconde, il doit également avoir une précision de début.  
  
-   Lorsque l’intervalle est composé de champs de début et de fin, le champ de début est un champ qui n’est pas un second, qui peut être accompagné de la précision du champ de début de l’intervalle entre parenthèses. Le champ de fin peut être un champ qui n’est pas un deuxième ou un second champ qui peut être accompagné d’un intervalle de précision de fractions de seconde entre parenthèses.  
  
 La chaîne d’intervalle de *valeur* est placée entre guillemets simples. Il peut s’agir d’un littéral d’année-mois ou d’un littéral de jour-heure. Le format de la chaîne dans la *valeur* est déterminé par les règles suivantes :  
  
-   La chaîne contient une valeur décimale pour chaque champ impliqué par le \<*interval* *qualifier*> .  
  
-   Si la précision de l’intervalle comprend les champs année et mois, les valeurs de ces champs sont séparées par un signe moins.  
  
-   Si la précision de l’intervalle comprend les champs jour et heure, les valeurs de ces champs sont séparées par un espace.  
  
-   Si la précision de l’intervalle comprend l’heure du champ et les champs d’ordre inférieur (MINUTE et seconde), les valeurs de ces champs sont séparées par un signe deux-points.  
  
-   Si la précision de l’intervalle comprend un deuxième champ et que la précision en secondes exprimée ou implicite est différente de zéro, le caractère situé juste avant le premier chiffre de la partie fractionnaire de la seconde est un point.  
  
-   Aucun champ ne peut comporter plus de deux chiffres, sauf :  
  
    -   La valeur du champ de début peut être aussi longue que la précision de début de l’intervalle exprimé ou implicite.  
  
    -   La partie fractionnaire du deuxième champ peut être aussi longue que la précision en secondes exprimée ou implicite.  
  
    -   Les champs de fin suivent les contraintes habituelles du calendrier grégorien. (Voir [les contraintes du calendrier grégorien](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md).)  
  
 Le tableau suivant répertorie des exemples de littéraux d’intervalle valides, tels qu’ils sont inclus dans la clause ODBC Escape pour les intervalles. La syntaxe de la clause Escape est la suivante :  
  
> [!NOTE]  
>  *{Intervalle du signe intervalle-intervalle de chaîne-qualificateur}*  
  
|Littéral d’échappement, clause|Intervalle spécifié|  
|---------------------------|------------------------|  
|{INTERVAL' 326 'ANNÉE (4)}|Spécifie un intervalle de 326 ans. La précision de l’intervalle est de 4.|  
|{INTERVAL' 326 'MOIS (3)}|Spécifie un intervalle de 326 mois. La précision de début de l’intervalle est 3.|  
|{INTERVAL' 3261 'DAY (4)}|Spécifie un intervalle de 3261 jours. La précision de l’intervalle est de 4.|  
|{INTERVAL' 163 'HEURE (3)}|Spécifie un intervalle de 163 jours. La précision de début de l’intervalle est 3.|  
|{INTERVAL' 163 'MINUTE (3)}|Spécifie un intervalle de 163 minutes. La précision de début de l’intervalle est 3.|  
|{INTERVAL' 223,16 'SECONDE (3,2)}|Spécifie un intervalle de 223,16 secondes. La précision de début de l’intervalle est 3 et la précision en secondes est 2.|  
|{INTERVAL « 163-11 » YEAR (3) AU MOIS}|Spécifie un intervalle de 163 ans et 11 mois. La précision de début de l’intervalle est 3.|  
|{INTERVAL' 163 12 'DAY (3) À HOUR}|Spécifie un intervalle de 163 jours et 12 heures. La précision de début de l’intervalle est 3.|  
|{INTERVAL' 163 12:39 'DAY (3) À MINUTE}|Spécifie un intervalle de 163 jours, 12 heures et 39 minutes. La précision de début de l’intervalle est 3.|  
|{INTERVAL' 163 12:39:59.163 'DAY (3) TO SECOND (3)}|Spécifie un intervalle de 163 jours, 12 heures, 39 minutes et 59,163 secondes. La précision de début de l’intervalle est 3 et la précision en secondes est 3.|  
|{INTERVALLE « 163:39 » HEURE (3) À MINUTE}|Spécifie un intervalle de 163 heures et de 39 minutes. La précision de début de l’intervalle est 3.|  
|{INTERVAL' 163:39:59.163 'HEURE (3) À SECONDE (4)}|Spécifie un intervalle de 163 heures, 39 minutes et 59,163 secondes. La précision de début de l’intervalle est 3 et la précision en secondes est 4.|  
|{INTERVAL' 163:59.163 'MINUTE (3) À SECOND (5)}|Spécifie un intervalle de 163 minutes et 59,163 secondes. La précision de début de l’intervalle est 3 et la précision en secondes est 5.|  
|{INTERVAL-' 16 23:39:56.23 'JOUR À SECONDE}|Spécifie un intervalle de moins 16 jours, 23 heures, 39 minutes et 56,23 secondes. La précision de début implicite est 2, et la précision des secondes implicites est 6.|  
  
 Le tableau suivant répertorie des exemples de littéraux d’intervalle non valides :  
  
|Littéral d’échappement, clause|Raison pour laquelle non valide|  
|---------------------------|------------------------|  
|{INTERVAL' 163 'HEURE (2)}|La précision de début de l’intervalle est 2, mais la valeur du champ de début est 163.|  
|{INTERVAL' 223,16 'SECOND (2, 2)}<br /><br /> {INTERVAL' 223,16 'SECOND (3, 1)}|Dans le premier exemple, la précision de début est trop petite et, dans le deuxième exemple, la précision en secondes est trop petite.|  
|{INTERVAL' 223,16 'SECONDE}<br /><br /> {INTERVAL' 223 'ANNÉE}|Étant donné que la précision de début n’est pas spécifiée, la valeur par défaut est 2, ce qui est trop petit pour contenir le littéral spécifié.|  
|{INTERVAL' 22,1234567 'SECONDE}|La précision en secondes n’étant pas spécifiée, la valeur par défaut est 6. Le littéral a sept chiffres après la virgule décimale.|  
|{INTERVAL « 163-13 » YEAR (3) AU MOIS}<br /><br /> {INTERVAL' 163 65 'DAY (3) À HOUR}<br /><br /> {INTERVAL' 163 62:39 'DAY (3) À MINUTE}<br /><br /> {INTERVAL' 163 12:125:59.163 'DAY (3) TO SECOND (3)}<br /><br /> {INTERVALLE « 163:144 » HEURE (3) À MINUTE}<br /><br /> {INTERVAL' 163:567:234.163 'HEURE (3) À SECONDE (4)}<br /><br /> {INTERVAL' 163:591.163 'MINUTE (3) À SECOND (5)}|Le champ de fin ne suit pas les règles du calendrier grégorien.|
