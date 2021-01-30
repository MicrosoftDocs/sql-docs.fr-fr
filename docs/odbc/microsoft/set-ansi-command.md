---
description: SET ANSI, commande
title: SET ANSI, commande | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- set ANSI command [ODBC]
ms.assetid: cf9a01b2-14bf-458c-a73c-2a58ddef32d8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bb9dbd2b73b4ff0f7f75442c42de31dbe389211a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208669"
---
# <a name="set-ansi-command"></a>SET ANSI, commande
Détermine comment les comparaisons entre des chaînes de longueurs différentes sont effectuées avec l’opérateur = dans les commandes SQL Visual FoxPro.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET ANSI ON | OFF  
```  
  
## <a name="arguments"></a>Arguments  
 ACTIVÉ  
 (Valeur par défaut pour le pilote ; la valeur par défaut de Visual FoxPro est OFF.) Remplit la chaîne plus petite avec les espaces nécessaires pour la faire correspondre à la longueur de la chaîne la plus longue. Les deux chaînes sont ensuite comparées caractère pour les caractères pour leur longueur entière. Prenons l’exemple de cette comparaison :  
  
```  
'Tommy' = 'Tom'  
```  
  
 Le résultat est false (. F.) si SET ANSI a la valeur on, car lorsqu’il est rempli, « Tom » devient « Tom » et les chaînes « Tom » et « Tommy » ne correspondent pas au caractère pour le caractère.  
  
 L’opérateur « = = » utilise cette méthode pour les comparaisons dans les commandes SQL Visual FoxPro.  
  
 OFF  
 Spécifie que la chaîne la plus petite ne doit pas être complétée par des espaces. Les deux chaînes sont des caractères comparés au caractère jusqu’à ce que la fin de la chaîne plus petite soit atteinte. Prenons l’exemple de cette comparaison :  
  
```  
'Tommy' = 'Tom'  
```  
  
 Le résultat est true (. T.) Lorsque SET ANSI est désactivé, car la comparaison s’arrête après « Tom ».  
  
## <a name="remarks"></a>Notes  
 SET ANSI détermine si le plus petit de deux chaînes est complété par des espaces quand une comparaison de chaînes SQL est effectuée. SET ANSI n’a aucun effet sur l’opérateur = =; Lorsque vous utilisez l’opérateur = =, la chaîne la plus petite est toujours complétée par des espaces pour la comparaison.  
  
## <a name="string-order"></a>Ordre des chaînes  
 Dans les commandes SQL, l’ordre de gauche à droite des deux chaînes dans une comparaison est irrelevantswitching une chaîne d’un côté de l’opérateur = ou = = à l’autre n’affecte pas le résultat de la comparaison.  
  
## <a name="see-also"></a>Voir aussi  
 [SELECT-commande SQL](../../odbc/microsoft/select-sql-command.md)   
 [SET EXACT, commande](../../odbc/microsoft/set-exact-command.md)
