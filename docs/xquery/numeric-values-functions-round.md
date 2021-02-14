---
title: Fonction Round (XQuery) | Microsoft Docs
description: En savoir plus sur la fonction XQuery Round () qui retourne le nombre qui n’a pas de partie fractionnaire qui est le plus proche de l’argument spécifié.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:round function
- round function [XQuery]
ms.assetid: 320b572f-bd5b-4055-95a6-dec5718c0041
author: rothja
ms.author: jroth
ms.openlocfilehash: f245b613c3e5e32fd2d4cc8eb09e969719ab1762
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347915"
---
# <a name="numeric-values-functions---round"></a>Fonctions de valeurs numériques : round
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Retourne le nombre n'ayant pas de partie décimale qui est le plus proche de l'argument. S'il y a plusieurs nombres qui correspondent, le plus proche de l'infini positif est retourné. Par exemple :  
  
 Si l’argument est 2,5, **Round ()** retourne 3.  
  
 Si l’argument est 2,4999, **Round ()** retourne 2.  
  
 Si l’argument est-2,5, **Round ()** retourne-2.  
  
 Si l’argument est une séquence vide, **Round ()** retourne la séquence vide.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:round ( $arg as numeric?) as numeric?  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Nombre à laquelle s'applique la fonction.  
  
## <a name="remarks"></a>Notes  
 Si le type de *$arg* est l’un des trois types numériques de base, **XS : float**, **XS : double** ou **XS : Decimal**, le type de retour est le même que le type de *$arg* . Si le type de *$arg* est un type dérivé de l’un des types numériques, le type de retour est le type numérique de base.  
  
 Si l’entrée des fonctions **FN : Floor**, **FN : Ceiling** ou **FN : Round** est **xdt : untypedAtomic**, les données non typées sont implicitement converties en **XS : double**.  
  
 Tout autre type génère une erreur statique.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la base de données AdventureWorks.  
  
 Vous pouvez utiliser l’exemple Working dans la [fonction ceiling (XQuery)](../xquery/numeric-values-functions-ceiling.md) pour la fonction XQuery **Round ()** . Il vous suffit de remplacer la fonction **Ceiling ()** de la requête par la fonction **Round ()** .  
  
## <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   La fonction **Round ()** mappe les valeurs entières à XS : Decimal.  
  
-   La fonction **Round ()** des valeurs XS : double et XS : float comprise entre-0,5 E0 et-0E0 est mappée à 0E0 au lieu de-0E0.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonction Floor &#40;XQuery&#41;](../xquery/numeric-values-functions-floor.md)   
 [Fonction ceiling &#40;XQuery&#41;](../xquery/numeric-values-functions-ceiling.md)  
  
  
