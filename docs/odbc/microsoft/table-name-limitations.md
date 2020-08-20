---
description: Limitations des noms de table
title: Limitations des noms de tables | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- ODBC SQL grammar, table name limitations
- table name limitations [ODBC]
- Excel driver [ODBC], table name limitations
ms.assetid: d9843e4b-1d05-4d5a-9dc3-ee9ec59edb97
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ef1395ab9e112e045fb90dc6f06a83b96bc48697
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88471471"
---
# <a name="table-name-limitations"></a>Limitations des noms de table
Les noms de table peuvent contenir des caractères valides (par exemple, des espaces). Si les noms de table contiennent des caractères à l’exception des lettres, des chiffres et des traits de soulignement, le nom doit être délimité en le mettant entre guillemets (').  
  
 Lorsque le pilote Microsoft Excel est utilisé et qu’un nom de table n’est pas qualifié par une référence de base de données, la base de données par défaut est implicite. Si un nom dans Microsoft Excel contient le caractère «  ! », il sera automatiquement converti en caractère « $ » à la place.  
  
 Le nom de table Microsoft Excel référencé \<filename> par est pris en charge pour les fichiers Microsoft excel 3,0 et 4,0. Le nom de table Microsoft Excel référencé \<workbook-name> par est pris en charge pour les fichiers Microsoft excel 5,0, 7,0 ou 97.  
  
 Lorsque le pilote dBASE est utilisé, les caractères avec une valeur ASCII supérieure à 127 sont convertis en traits de soulignement.  
  
 Lorsque le pilote Microsoft Access est utilisé, le nom de la table est limité à 64 caractères.  
  
 Lorsque le pilote dBASE, Microsoft Excel 3,0 ou 4,0, Paradox ou texte est utilisé, les mots clés MS-DOS spéciaux CON, aux, LPT1 et LPT2 ne doivent pas être utilisés comme noms de tables.
