---
description: Fonctions d'heure, de date et d'intervalle
title: Fonctions d’heure, de date et d’intervalle | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], time functions
- functions [ODBC], date functions
- interval functions [ODBC]
- functions [ODBC], interval functions
- time functions [ODBC]
- date functions [ODBC]
ms.assetid: bdf054a0-7aba-4e99-a34a-799917376fd5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e73d295613f3f11b72317d20b49a6c75be802cbd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202494"
---
# <a name="time-date-and-interval-functions"></a>Fonctions d'heure, de date et d'intervalle
Le tableau suivant répertorie les fonctions d’heure et de date incluses dans l’ensemble de fonctions scalaires ODBC. Une application peut déterminer les fonctions de date et d’heure prises en charge par un pilote en appelant **SQLGetInfo** avec un *type d’informations* SQL_TIMEDATE_FUNCTIONS.  
  
 Les arguments dénotés comme *timestamp_exp* peuvent être le nom d’une colonne, le résultat d’une autre fonction scalaire ou ODBC *-Time-Escape*, *ODBC-date-Escape* ou *ODBC-timestamp-Escape*, où le type de données sous-jacent peut être représenté sous la forme SQL_CHAR, SQL_VARCHAR, SQL_TYPE_TIME, SQL_TYPE_DATE ou SQL_TYPE_TIMESTAMP.  
  
 Les arguments dénotés comme *date_exp* peuvent être le nom d’une colonne, le résultat d’une autre fonction scalaire, ou *ODBC-date-Escape* ou *ODBC-timestamp-Escape*, où le type de données sous-jacent peut être représenté sous la forme SQL_CHAR, SQL_VARCHAR, SQL_TYPE_DATE ou SQL_TYPE_TIMESTAMP.  
  
 Les arguments dénotés comme *time_exp* peuvent être le nom d’une colonne, le résultat d’une autre fonction scalaire, ou *ODBC-Time-Escape* ou *ODBC-timestamp-Escape*, où le type de données sous-jacent peut être représenté sous la forme SQL_CHAR, SQL_VARCHAR, SQL_TYPE_TIME ou SQL_TYPE_TIMESTAMP.  
  
 Les fonctions scalaires CURRENT_DATE, CURRENT_TIME et CURRENT_TIMESTAMP ont été ajoutées dans ODBC 3,0 pour s’aligner sur SQL-92.  
  
|Fonction|Description|  
|--------------|-----------------|  
|**CURRENT_DATE ()** (ODBC 3,0)|Retourne la date du jour.|  
|**Current_time [(** *Time-précision* **)]** (ODBC 3,0)|Retourne l'heure locale actuelle. L’argument de *précision temporelle* détermine la précision en secondes de la valeur retournée.|  
|**CURRENT_TIMESTAMP**<br /> **[(** *horodatage-précision* **)]** (ODBC 3,0)|Retourne la date locale actuelle et l’heure locale sous la forme d’une valeur d’horodatage. L’argument *timestamp-precise* détermine la précision en secondes de l’horodatage retourné.|  
|**Caillé ()** (ODBC 1,0)|Retourne la date du jour.|  
|**CURTIME ()** (ODBC 1,0)|Retourne l'heure locale actuelle.|  
|**DAYNAME (** *date_exp* **)** (ODBC 2,0)|Retourne une chaîne de caractères contenant le nom spécifique à la source de données du jour (par exemple, du dimanche au samedi ou du soleil). à sam pour une source de données qui utilise l’anglais, ou Sonntag à Samstag pour une source de données qui utilise l’allemand) pour la partie jour de *date_exp*.|  
|**DayOfMonth (** *date_exp* **)** (ODBC 1,0)|Retourne le jour du mois basé sur le champ month dans *date_exp* sous la forme d’une valeur entière comprise dans la plage 1-31.|  
|**DayOfWeek (** *date_exp* **)** (ODBC 1,0)|Retourne le jour de la semaine basé sur le champ week dans *date_exp* sous la forme d’une valeur entière comprise dans la plage 1-7, où 1 représente Sunday.|  
|**DayOfYear (** *date_exp* **)** (ODBC 1,0)|Retourne le jour de l’année basé sur le champ Year de *date_exp* sous la forme d’une valeur entière comprise dans la plage 1-366.|  
|**Extraire (** *extract-Field de* *extract-source* **)** (ODBC 3,0)|Retourne la partie *extrait-champ* de la *source d’extraction*. L’argument *extract-source* est une expression datetime ou Interval. L’argument *extract-Field* peut être l’un des mots clés suivants :<br /><br /> ANNÉE MOIS JOUR HEURE MINUTE SECONDE<br /><br /> La précision de la valeur retournée est définie par l’implémentation. L’échelle est égale à 0, sauf si la seconde est spécifiée. dans ce cas, l’échelle n’est pas inférieure à la précision en fractions de seconde du champ *extract-source* .|  
|**Heure (** *time_exp* **)** (ODBC 1,0)|Retourne l’heure basée sur le champ heure de *time_exp* sous la forme d’une valeur entière comprise dans la plage 0-23.|  
|**Minute (** *time_exp* **)** (ODBC 1,0)|Retourne la minute basée sur le champ minute de *time_exp* sous la forme d’une valeur entière comprise dans la plage 0-59.|  
|**Mois (** *date_exp* **)** (ODBC 1,0)|Retourne le mois basé sur le champ month dans *date_exp* sous la forme d’une valeur entière comprise dans la plage 1-12.|  
|**MonthName (** *date_exp* **)** (ODBC 2,0)|Retourne une chaîne de caractères contenant le nom spécifique à la source de données du mois (par exemple, janvier à décembre ou Jan. jusqu’à Dec pour une source de données qui utilise l’anglais, ou Januar à Dezember pour une source de données qui utilise l’allemand) pour la partie mois de *date_exp*.|  
|**Now ()** (ODBC 1,0)|Retourne la date et l’heure actuelles sous la forme d’une valeur d’horodatage.|  
|**Trimestre (** *date_exp* **)** (ODBC 1,0)|Retourne le trimestre dans *date_exp* sous la forme d’une valeur entière comprise dans la plage 1-4, où 1 représente le 1er janvier au 31 mars.|  
|**Deuxième (** *time_exp* **)** (ODBC 1,0)|Retourne la seconde en fonction du deuxième champ de *time_exp* sous la forme d’une valeur entière comprise dans la plage 0-59.|
|**TIMESTAMPADD (** *intervalle*, *integer_exp*, *timestamp_exp* **)** (ODBC 2,0)|Retourne l’horodateur calculé en ajoutant *integer_exp* intervalles de type *Interval* à *timestamp_exp*. Les valeurs valides de *Interval* sont les mots clés suivants :<br /><br /> SQL_TSI_FRAC_SECOND<br /><br /> SQL_TSI_SECOND<br /><br /> SQL_TSI_MINUTE<br /><br /> SQL_TSI_HOUR<br /><br /> SQL_TSI_DAY<br /><br /> SQL_TSI_WEEK<br /><br /> SQL_TSI_MONTH<br /><br /> SQL_TSI_QUARTER<br /><br /> SQL_TSI_YEAR<br /><br /> où les fractions de seconde sont exprimées en milliardièmes de seconde. Par exemple, l’instruction SQL suivante renvoie le nom de chaque employé et sa date anniversaire d’un an :<br /><br /> `SELECT NAME, {fn  TIMESTAMPADD(SQL_TSI_YEAR, 1, HIRE_DATE)} FROM  EMPLOYEES`<br /><br /> Si *timestamp_exp* est une valeur d’heure et que l' *intervalle* spécifie les jours, les semaines, les mois, les trimestres ou les années, la partie de la date de *timestamp_exp* est définie sur la date actuelle avant le calcul de l’horodatage résultant.<br /><br /> Si *timestamp_exp* est une valeur de date et que *Interval* spécifie des fractions de seconde, des secondes, des minutes ou des heures, la partie heure de *timestamp_exp* est définie sur 0 avant le calcul de l’horodateur résultant.<br /><br /> Une application détermine les intervalles pris en charge par une source de données en appelant **SQLGetInfo** avec l’option SQL_TIMEDATE_ADD_INTERVALS.|  
|**TIMESTAMPDIFF (** *intervalle*, *timestamp_exp1*, *timestamp_exp2* **)** (ODBC 2,0)|Retourne le nombre entier d’intervalles de type *intervalle* par lequel *timestamp_exp2* est supérieur à *timestamp_exp1*. Les valeurs valides de *Interval* sont les mots clés suivants :<br /><br /> SQL_TSI_FRAC_SECOND<br /><br /> SQL_TSI_SECOND<br /><br /> SQL_TSI_MINUTE<br /><br /> SQL_TSI_HOUR<br /><br /> SQL_TSI_DAY<br /><br /> SQL_TSI_WEEK<br /><br /> SQL_TSI_MONTH<br /><br /> SQL_TSI_QUARTER<br /><br /> SQL_TSI_YEAR<br /><br /> où les fractions de seconde sont exprimées en milliardièmes de seconde. Par exemple, l’instruction SQL suivante renvoie le nom de chaque employé et le nombre d’années qu’il a occupé :<br /><br /> `SELECT NAME, {fn  TIMESTAMPDIFF(SQL_TSI_YEAR, {fn CURDATE()}, HIRE_DATE)} FROM EMPLOYEES`<br /><br /> Si l’une des expressions timestamp est une valeur d’heure et un *intervalle* spécifie les jours, les semaines, les mois, les trimestres ou les années, la partie Date de cet horodatage est définie sur la date actuelle avant de calculer la différence entre les horodateurs.<br /><br /> Si l’une des expressions timestamp est une valeur de date et que *Interval* spécifie des fractions de seconde, de secondes, de minutes ou d’heures, la partie heure de cet horodatage est définie sur 0 avant de calculer la différence entre les horodateurs.<br /><br /> Une application détermine les intervalles pris en charge par une source de données en appelant **SQLGetInfo** avec l’option SQL_TIMEDATE_DIFF_INTERVALS.|  
|**Semaine (** *date_exp* **)** (ODBC 1,0)|Retourne la semaine de l’année basée sur le champ week dans *date_exp* sous la forme d’une valeur entière comprise dans la plage 1-53.|  
|**Year (** *date_exp* **)** (ODBC 1,0)|Retourne l’année basée sur le champ Year de *date_exp* sous la forme d’une valeur entière. La plage est dépendante de la source de données.|
