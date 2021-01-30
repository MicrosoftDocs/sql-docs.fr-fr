---
description: UPDATE, instruction - limitations
title: Limitations des instructions UPDATE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- UPDATE statement limitations [ODBC]
- ODBC SQL grammar, UPDATE statement limitations
ms.assetid: 14700aac-e135-4dc0-9138-4b01224461d5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 721af98991f4d63c459ba5df44bc425c245d2dc7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190629"
---
# <a name="update-statement-limitations"></a>UPDATE, instruction - limitations
Pour que le pilote Paradox met à jour une table, la table doit avoir un index unique (clé primaire Paradox). Lorsque vous utilisez le pilote Paradox sans implémenter le Moteur de base de données Borland, il n’est pas possible de mettre à jour une table Paradox.  
  
 Non pris en charge par le pilote de texte.  
  
 Lorsque le pilote Microsoft Excel est utilisé, il est possible de mettre à jour les valeurs, mais une ligne ne peut pas être supprimée d’une table basée sur une feuille de calcul Microsoft Excel. Par conséquent, l’instruction UPDATE n’est pas considérée comme officiellement prise en charge par le pilote Microsoft Excel. Seule l’instruction INSERT est considérée comme prise en charge.
