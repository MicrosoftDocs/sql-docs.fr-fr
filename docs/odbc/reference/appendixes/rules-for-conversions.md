---
description: Règles pour les conversions
title: Règles pour les conversions | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- numeric data type [ODBC], literals
- conversions with numeric literals [ODBC]
- numeric literals [ODBC]
- literals [ODBC], numeric
ms.assetid: 89f846a3-001d-496a-9843-ac9c38dc1762
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b015a60e376e47a4c3c931d4739a884db23fc98b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187137"
---
# <a name="rules-for-conversions"></a>Règles pour les conversions
Les règles de cette section s’appliquent aux conversions impliquant des littéraux numériques. Dans le cadre de ces règles, les termes suivants sont définis :  
  
-   *Attribution du magasin :* Lors de l’envoi de données dans une colonne de table dans une base de données. Cela se produit lors des appels à **SQLExecute**, **SQLExecDirect** et **SQLSetPos**. Pendant l’attribution du magasin, la « cible » fait référence à une colonne de base de données et la « source » fait référence aux données dans les mémoires tampons d’application.  
  
-   *Assignation d’extraction :* Lors de la récupération de données de la base de données dans des mémoires tampons d’application. Cela se produit lors des appels à **SQLFetch**, **SQLGetData**, **SQLFetchScroll** et **SQLSetPos**. Lors de l’assignation de la récupération, « Target » fait référence aux mémoires tampons de l’application et « source » fait référence à la colonne de base de données.  
  
-   *CS :* Valeur dans la source du caractère.  
  
-   *NT :* Valeur dans la cible numérique.  
  
-   *Ns :* Valeur dans la source numérique.  
  
-   *CT :* Valeur dans la cible de caractère.  
  
-   Précision d’un littéral numérique exact : nombre de chiffres qu’il contient.  
  
-   Échelle d’un littéral numérique exact : nombre de chiffres à droite de la période exprimée ou implicite.  
  
-   Précision d’un littéral numérique approximatif : la précision de sa mantisse.  
  
## <a name="character-source-to-numeric-target"></a>Source de caractères en cible numérique  
 Voici les règles de conversion d’une source de caractères (CS) en cible numérique (NT) :  
  
1.  Remplacez CS par la valeur obtenue en supprimant les espaces de début ou de fin dans CS. Si CS n’est pas un *littéral numérique* valide, SQLSTATE 22018 (valeur de caractère non valide pour la spécification de cast) est retournée.  
  
2.  Remplacez CS par la valeur obtenue en supprimant les zéros non significatifs avant la virgule décimale, les zéros de fin après la virgule décimale, ou les deux.  
  
3.  Convertissez CS en NT. Si la conversion entraîne une perte de chiffres significatifs, SQLSTATE 22003 (valeur numérique hors limites) est retourné. Si la conversion entraîne la perte de chiffres non significatifs, SQLSTATE 01S07 (troncation fractionnaire) est retourné.  
  
## <a name="numeric-source-to-character-target"></a>Cible numérique à caractère cible  
 Voici les règles de conversion d’une source numérique (NS) en une cible de caractère (CT) :  
  
1.  Donnez à LT la longueur en caractères de CT. Pour l’assignation de récupération, la valeur LT est égale à la longueur de la mémoire tampon en caractères moins le nombre d’octets dans le caractère de fin null pour ce jeu de caractères.  
  
2.  Parfois  
  
    -   Si NS est un type numérique exact, laissez YP égal à la chaîne de caractères la plus petite conforme à la définition du *littéral exact-Numeric* , de sorte que l’échelle de YP est identique à l’échelle de NS, et la valeur interprétée de YP est la valeur absolue de NS.  
  
    -   Si NS est un type numérique approximatif, laissez YP être une chaîne de caractères comme suit :  
  
         Casse :  
  
         Si la valeur NS est égale à 0, YP est égal à 0.  
  
         Laissez YSN être la chaîne de caractères la plus petite conforme à la définition du littéral exact-*Numeric* et dont la valeur interprétée est la valeur absolue de NS. Si la longueur de YSN est inférieure à la valeur (*précision* + 1) du type de données NS, laissez YP égal à YSN.  
  
         Dans le cas contraire, YP est la chaîne de caractères la plus petite qui est conforme à la définition du *littéral approximatif numérique* dont la valeur interprétée est la valeur absolue de NS et dont la *mantisse* est un *chiffre* unique qui n’est pas « 0 », suivi d’un *point* et d’un *entier non signé*.  
  
3.  Casse :  
  
    -   Si la valeur NS est inférieure à 0, le résultat est le suivant :  
  
         '-'  &#124;&#124; YP  
  
         où « &#124;&#124; » est l’opérateur de concaténation de chaînes.  
  
         Sinon, laissez Y égal à YP.  
  
4.  La longueur des caractères de a est égale à EP.  
  
5.  Casse :  
  
    -   Si EP est égal à LT, CT a la valeur Y.  
  
    -   Si EP est inférieur à LT, CT est défini sur Y étendu à droite en fonction du nombre d’espaces approprié.  
  
         Sinon (LY > LT), copiez les premiers caractères LT de Y dans CT.  
  
         Casse :  
  
         S’il s’agit d’une attribution de magasin, retournez l’erreur SQLSTATE 22001 (données de chaîne tronquées à droite).  
  
         S’il s’agit d’une assignation de récupération, retournez l’avertissement SQLSTATE 01004 (données de chaîne tronquées à droite). Lorsque la copie entraîne la perte de chiffres fractionnaires (autres que des zéros de fin), elle est définie par le pilote, que l’un des éléments suivants se produise :  
  
         (1) le pilote tronque la chaîne de Y vers une échelle appropriée (qui peut être également zéro) et écrit le résultat dans CT.  
  
         (2) le pilote arrondit la chaîne de Y à une échelle appropriée (qui peut être également zéro) et écrit le résultat dans CT.  
  
         (3) le pilote n’est ni tronqué ni arrondi, mais copie uniquement les premiers caractères LT de Y dans CT.
