---
title: Fonction Avg (XQuery) | Microsoft Docs
description: En savoir plus sur la fonction XQuery AVG () qui retourne la moyenne d’une séquence de nombres spécifiée.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:avg function
- avg function [XQuery]
ms.assetid: 0cc60267-3c56-4a88-8ad7-bb07f0255d56
author: rothja
ms.author: jroth
ms.openlocfilehash: ee09ab89e1acb0fc8ddad23d002e2cd0e136bc59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340496"
---
# <a name="aggregate-functions---avg"></a>Fonctions d’agrégation : avg
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Retourne la moyenne d'une série de nombres.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:avg($arg as xdt:anyAtomicType*) as xdt:anyAtomicType?  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Série de valeurs atomiques dont la moyenne est calculée.  
  
## <a name="remarks"></a>Notes  
 Tous les types des valeurs atomisées passées à **AVG ()** doivent être un sous-type de exactement l’un des trois types de base numériques intégrés ou xdt : untypedAtomic. Ils ne peuvent pas être un mélange. Les valeurs de type xdt:untypedAtomic sont traitées comme valeurs xs:double. Le résultat de la fonction **AVG ()** reçoit le type de base des types transmis, tels que XS : double dans le cas de xdt : untypedAtomic.  
  
 Si l'entrée est vide (valeur empty) de façon statique, « empty » est alors implicite et une erreur statique est émise.  
  
 La fonction **AVG ()** retourne la moyenne des nombres calculés. Par exemple :  
  
 **Sum (** *$arg* **) div count (** *$arg* **)**  
  
 Si *$arg* est une séquence vide, la séquence vide est retournée.  
  
 Si une valeur xdt : untypedAtomic ne peut pas être convertie en XS : double, la valeur est ignorée dans la séquence d’entrée *$arg*.  
  
 Dans tous les autres cas, la fonction retourne une erreur statique.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la base de données AdventureWorks.  
  
### <a name="a-using-the-avg-xquery-function-to-find-work-center-locations-in-the-manufacturing-process-in-which-labor-hours-are-greater-than-the-average-for-all-work-center-locations"></a>R. Utilisation de la fonction XQuery avg() pour rechercher les ateliers dans le processus de fabrication, dans lesquels le nombre d'heures de travail est supérieur à la moyenne de tous les ateliers.  
 Vous pouvez réécrire la requête fournie dans la [fonction min (XQuery)](../xquery/aggregate-functions-min.md) pour utiliser la fonction **AVG ()** .  
  
## <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   La fonction **AVG ()** mappe tous les entiers à XS : Decimal.  
  
-   La fonction **AVG ()** sur les valeurs de type xs : Duration n’est pas prise en charge.  
  
-   Les séquences faisant intervenir plusieurs types dérivés de différents types de base ne sont pas prises en charge.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
