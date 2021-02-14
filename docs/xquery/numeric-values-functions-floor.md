---
title: Fonction Floor (XQuery) | Microsoft Docs
description: En savoir plus sur la fonction XQuery Floor () qui retourne le plus grand nombre sans partie décimale qui n’est pas supérieur à la valeur de son argument.
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
- floor function [XQuery]
- fn:floor function
ms.assetid: 4ace57dd-b66e-4b60-a2b9-a1b0f1a0831d
author: rothja
ms.author: jroth
ms.openlocfilehash: 206bacf618d38c2dbbe593355d48ed66f7a9e67f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341542"
---
# <a name="numeric-values-functions---floor"></a>Fonctions de valeurs numériques : floor
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Renvoie le nombre le plus élevé sans portion décimale qui ne dépasse pas la valeur de cet argument. Si l'argument est une séquence vide, la fonction renvoie la séquence vide.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:floor ($arg as numeric?) as numeric?  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Nombre à laquelle s'applique la fonction.  
  
## <a name="remarks"></a>Notes  
 Si le type de *$arg* est l’un des trois types numériques de base, **XS : float**, **XS : double** ou **XS : Decimal**, le type de retour est le même que le type de *$arg* . Si le type de *$arg* est un type dérivé de l’un des types numériques, le type de retour est le type numérique de base.  
  
 Si l’entrée des fonctions FN : Floor, fn : Ceiling ou FN : Round est **xdt : untypedAtomic**, les données non typées sont implicitement converties en **XS : double**. Tout autre type génère une erreur statique.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** de l’exemple de base de données AdventureWorks.  
  
 Vous pouvez utiliser l’exemple Working dans la [fonction ceiling (XQuery)](../xquery/numeric-values-functions-ceiling.md) pour la fonction XQuery **Floor ()** . Il vous suffit de remplacer la fonction **Ceiling ()** de la requête par la fonction **Floor ()** .  
  
## <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   La fonction **Floor ()** associe toutes les valeurs entières à XS : Decimal.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonction ceiling &#40;XQuery&#41;](../xquery/numeric-values-functions-ceiling.md)   
 [Fonction Round &#40;XQuery&#41;](../xquery/numeric-values-functions-round.md)   
 [Fonctions XQuery impliquant le type de données xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
