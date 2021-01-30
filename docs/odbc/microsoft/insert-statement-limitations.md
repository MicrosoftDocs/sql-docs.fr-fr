---
description: INSERT, instruction - limitations
title: Limitations des instructions INSERT | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, INSERT statement limitations
- INSERT statement limitations [ODBC]
- truncation of data [ODBC]
ms.assetid: dea05698-527a-41ab-8729-bbed85556185
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c8decef6ed2a17a2aca4b107d46b856b6ba828a4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211188"
---
# <a name="insert-statement-limitations"></a>INSERT, instruction - limitations
Les données insérées sont tronquées à droite sans avertissement si elles sont trop longues pour tenir dans la colonne.  
  
 Toute tentative d’insertion d’une valeur qui se trouve en dehors de la plage du type de données d’une colonne entraîne l’insertion d’une valeur NULL dans la colonne.  
  
 Lorsqu’un dBASE, Microsoft Excel, Paradox ou TextDriver est utilisé, l’insertion d’une chaîne de longueur nulle dans une colonne insère en fait une valeur NULL à la place.  
  
 Lorsque le pilote Microsoft Excel est utilisé, si une chaîne vide est insérée dans une colonne, la chaîne vide est convertie en valeur NULL ; une instruction SELECT recherchée exécutée avec une chaîne vide dans la clause WHERE ne réussit pas sur cette colonne.  
  
 Une table ne peut pas être mise à jour par le pilote Paradox dans les deux conditions suivantes :  
  
-   Lorsqu’un index unique n’est pas défini sur la table. Cela n’est pas vrai pour une table vide, qui peut être mise à jour avec une seule ligne, même si un index unique n’est pas défini sur la table. Si une seule ligne est insérée dans une table vide qui n’a pas d’index unique, une application ne peut pas créer d’index unique ou insérer des données supplémentaires après l’insertion de la ligne unique.  
  
-   Si le Moteur de base de données Borland n’est pas implémenté, seules les instructions Read et Append sont autorisées sur la table Paradox.  
  
 Lorsque le pilote de texte est utilisé, les valeurs NULL sont représentées par une chaîne remplie par des blancs dans des fichiers de longueur fixe, mais ne sont pas représentées par des espaces dans les fichiers délimités. Par exemple, dans la ligne suivante contenant trois champs, le deuxième champ est une valeur NULL :  
  
```  
"Smith:,, 123  
```  
  
 Lorsque le pilote de texte est utilisé, toutes les valeurs de colonne peuvent être complétées par des espaces de début. La longueur de n’importe quelle ligne doit être inférieure ou égale à 65 543 octets.
