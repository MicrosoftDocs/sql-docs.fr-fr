---
title: Max, fonction (XQuery) | Microsoft Docs
description: En savoir plus sur la fonction XQuery Max () qui retourne un élément dans une séquence dont la valeur est supérieure à celle de tous les autres.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- max function [XQuery]
- fn:max function
ms.assetid: 5ee625c0-044a-4cda-b210-02b64e619d65
author: rothja
ms.author: jroth
ms.openlocfilehash: b52ae499cf09e8fcd024f592ae639d0f9a7c1c5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341959"
---
# <a name="aggregate-functions---max"></a>Fonctions d’agrégation : max
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Retourne à partir d’une séquence de valeurs atomiques, *$arg*, un élément dont la valeur est supérieure à celle de tous les autres.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:max($arg as xdt:anyAtomicType*) as xdt:anyAtomicType?  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Séquence de valeurs atomiques à partir de laquelle la valeur maximale est renvoyée.  
  
## <a name="remarks"></a>Notes  
 Tous les types des valeurs atomisées passées à **Max ()** doivent être des sous-types du même type de base. Les types de base acceptés sont les types qui prennent en charge l’opération **gt** . Ces types incluent les trois types numériques de base intégrés, les types de base date/heure et les types xs:string (chaîne), xs:boolean (booléen) et xdt:untypedAtomic (atomique non typé). Les valeurs de type xdt:untypedAtomic sont converties en xs:double. S’il existe un mélange de ces types, ou si d’autres valeurs d’autres types sont passées, une erreur statique est générée.  
  
 Le résultat de **Max ()** reçoit le type de base des types transmis, tel que XS : double dans le cas de xdt : untypedAtomic. Si l'entrée est vide (valeur empty) de façon statique, « empty » est alors implicite et une erreur statique est émise.  
  
 La fonction **Max ()** retourne la valeur unique dans la séquence qui est supérieure à toute autre valeur dans la séquence d’entrée. Pour les valeurs xs:string, le classement par défaut des points de code Unicode est utilisé. Si une valeur xdt : untypedAtomic ne peut pas être convertie en XS : double, la valeur est ignorée dans la séquence d’entrée *$arg*. Si l'entrée est une séquence vide calculée de manière dynamique, la séquence vide est renvoyée.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] base de données.  
  
### <a name="a-using-the-max-xquery-function-to-find-work-center-locations-in-the-manufacturing-process-that-have-the-most-labor-hours"></a>R. Utilisation de la fonction XQuery max() pour localiser les postes de travail du processus de fabrication enregistrant le plus d'heures de main-d'œuvre  
 La requête fournie dans la [fonction min (XQuery)](../xquery/aggregate-functions-min.md) peut être réécrite pour utiliser la fonction **Max ()** .  
  
## <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   La fonction **Max (**) mappe tous les entiers à XS : Decimal.  
  
-   La fonction **Max ()** sur des valeurs de type xs : Duration n’est pas prise en charge.  
  
-   Les séquences faisant intervenir plusieurs types dérivés de différents types de base ne sont pas prises en charge.  
  
-   L'option syntaxique fournissant un classement n'est pas prise en charge.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
