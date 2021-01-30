---
description: Longueur des types de données d’intervalle
title: Longueur du type de données de l’intervalle | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], interval data types
- length of data types [ODBC]
- interval data type [ODBC], length
ms.assetid: e9eb38d8-f9db-4401-8c62-aa394054cbbf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3ad7248bc56ab24d999bbc025fada9ca4501ab2d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208543"
---
# <a name="interval-data-type-length"></a>Longueur des types de données d’intervalle
Les règles suivantes sont utilisées pour déterminer la longueur d’un type de données Interval en caractères. La longueur est exprimée en nombre de caractères. Le nombre d’octets dépend du jeu de caractères. La longueur comprend les valeurs suivantes ajoutées ensemble :  
  
-   Deux caractères pour chaque champ dans l’intervalle qui n’est pas le champ de début.  
  
-   Pour le champ de début, le nombre de caractères qui est la précision de début Express ou implicite. Si la précision de début n’est pas spécifiée, la valeur par défaut est 2.  
  
-   Un caractère pour le séparateur entre les champs.  
  
-   L’une plus la précision de la seconde, Express ou implicite. Si la précision en secondes n’est pas spécifiée, la valeur par défaut est 6.  
  
 Les valeurs de longueur de colonne spécifiques pour chaque type de données Interval sont contenues dans la [taille de colonne](../../../odbc/reference/appendixes/column-size.md).
