---
description: DELETE, instruction - limitations
title: Limitations des instructions DELETE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- DELETE statement limitations [ODBC]
- ODBC SQL grammar, DELETE statement limitations
ms.assetid: 084761fe-e65b-4f38-ba4f-69884b2a7700
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b188d782f82e23d031b820bba5f56a30dad414bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181591"
---
# <a name="delete-statement-limitations"></a>DELETE, instruction - limitations
L’instruction DELETE n’est pas prise en charge pour Microsoft Excel ou le pilote texte. Notez que l’instruction INSERT est prise en charge pour le pilote Text.  
  
 Le pilote dBASE ne prend pas en charge l’empaquetage d’une table pour supprimer les valeurs « supprimées ».  
  
 Pour que le pilote Paradox supprime une ligne d’une table, la table doit avoir un index unique (clé primaire Paradox).
