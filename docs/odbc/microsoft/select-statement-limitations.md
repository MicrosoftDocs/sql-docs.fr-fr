---
description: SELECT, instruction - limitations
title: Limitations des instructions SELECT | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, SELECT statement limitations
- SELECT statement limitations [ODBC]
ms.assetid: c6b05955-f8fd-4706-a1a7-a8dbd74870c2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5bfbdbf4396b51624173e676f4b0c9bea1f4cf87
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208680"
---
# <a name="select-statement-limitations"></a>SELECT, instruction - limitations
Une colonne de fonction d’agrégation ne peut pas être combinée avec une colonne non agrégée dans une instruction SELECT.  
  
 La liste de sélection d’une instruction SELECT qui contient une clause GROUP BY ne peut avoir que des expressions de la clause GROUP BY ou des fonctions set.  
  
 L’utilisation d’un astérisque (pour sélectionner toutes les colonnes) dans une instruction SELECT contenant une clause GROUP BY n’est pas prise en charge. Les noms des colonnes à sélectionner doivent être spécifiés.  
  
 L’utilisation d’une barre verticale dans une instruction SELECT n’est pas prise en charge. Utilisez un paramètre dans l’instruction SELECT si vous devez faire référence à une valeur de données qui contient une barre verticale.  
  
 Lorsque vous utilisez un alias de colonne dans une instruction SELECT, le mot « As » doit précéder l’alias. Par exemple, « sélectionnez col1 en tant que a à partir de b ». Sans « As », l’instruction renverra une erreur.  
  
 Si un nom de colonne incorrect est entré dans une instruction SELECT, une erreur SQLSTATE 07001, « nombre incorrect de paramètres », est retournée à la place d’une erreur SQLSTATE S0022, « colonne introuvable ».  
  
 Lorsque le pilote Microsoft Excel est utilisé, si une chaîne vide est insérée dans une colonne, la chaîne vide est convertie en valeur NULL ; une instruction SELECT recherchée exécutée avec une chaîne vide dans la clause WHERE ne réussit pas sur cette colonne.
