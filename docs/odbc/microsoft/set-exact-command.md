---
description: SET EXACT, commande
title: DÉFINIR la commande exacte | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET EXACT command [ODBC]
ms.assetid: 9533d3e0-e7c1-49de-a3a3-0cc4373a91cb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 34a22d320dbd099bd26db1b5396cc55a15a4b288
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203956"
---
# <a name="set-exact-command"></a>SET EXACT, commande
Spécifie les règles de comparaison de deux chaînes de longueurs différentes.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET EXACT ON | OFF  
```  
  
## <a name="arguments"></a>Arguments  
 ACTIVÉ  
 Spécifie que les expressions doivent correspondre au caractère pour que le caractère soit équivalent. Les espaces à droite dans les expressions sont ignorés pour la comparaison. Pour la comparaison, le plus petit des deux expressions est rempli à droite avec des espaces pour correspondre à la longueur de l’expression la plus longue.  
  
 OFF  
 (Par défaut.) Spécifie que, pour être équivalent, les expressions doivent correspondre au caractère pour le caractère jusqu’à ce que la fin de l’expression sur le côté droit soit atteinte.  
  
## <a name="remarks"></a>Notes  
 Le paramètre SET EXACT n’a aucun effet si les deux chaînes ont la même longueur.  
  
## <a name="string-comparisons"></a>Comparaisons de chaînes  
 Visual FoxPro a deux opérateurs relationnels qui testent l’égalité.  
  
 L’opérateur = effectue une comparaison entre deux valeurs du même type. Cet opérateur est adapté à la comparaison de données de type caractère, numérique, date et logique.  
  
 Toutefois, lorsque vous comparez des expressions de caractères à l’opérateur =, les résultats peuvent ne pas être exactement ce que vous attendez. Les expressions de caractères sont des caractères comparés pour les caractères de gauche à droite jusqu’à ce que l’une des expressions ne soit pas égale à l’autre, jusqu’à ce que la fin de l’expression à droite de l’opérateur = soit atteinte (définissez la valeur EXACT OFF) ou jusqu’à ce que les extrémités des deux expressions soient atteintes (définissez EXACT ON).  
  
 L’opérateur « = = » peut être utilisé lorsqu’une comparaison exacte de données caractères est nécessaire. Si deux expressions de caractères sont comparées à l’opérateur = =, les expressions des deux côtés de l’opérateur = = doivent contenir exactement les mêmes caractères, y compris les espaces, pour être considérés comme égaux. Le paramètre SET EXACT est ignoré lorsque les chaînes de caractères sont comparées à l’aide de = =.  
  
 Le tableau suivant montre comment le choix de l’opérateur et le paramètre SET EXACT affectent les comparaisons. (Un trait de soulignement représente un espace vide.)  
  
|Comparaison|= EXACT|= EXACT LE|= = ON ou OFF EXACT|  
|----------------|------------------|-----------------|--------------------------|  
|« ABC » = « ABC »|Faire correspondre|Faire correspondre|Faire correspondre|  
|"AB" = "ABC"|Pas de correspondance|Pas de correspondance|Pas de correspondance|  
|"ABC" = "AB"|Faire correspondre|Pas de correspondance|Pas de correspondance|  
|"ABC" = "ab_"|Pas de correspondance|Pas de correspondance|Pas de correspondance|  
|"AB" = "ab_"|Pas de correspondance|Faire correspondre|Pas de correspondance|  
|"ab_" = "AB"|Faire correspondre|Faire correspondre|Pas de correspondance|  
|"" = "AB"|Pas de correspondance|Pas de correspondance|Pas de correspondance|  
|"AB" = ""|Faire correspondre|Pas de correspondance|Pas de correspondance|  
|"__" = ""|Faire correspondre|Faire correspondre|Pas de correspondance|  
|"" = "___"|Pas de correspondance|Faire correspondre|Pas de correspondance|  
|TRIM ("___") = ""|Faire correspondre|Faire correspondre|Faire correspondre|  
|"" = TRIM ("___")|Faire correspondre|Faire correspondre|Faire correspondre|  
  
## <a name="see-also"></a>Voir aussi  
 [SET ANSI, commande](../../odbc/microsoft/set-ansi-command.md)
