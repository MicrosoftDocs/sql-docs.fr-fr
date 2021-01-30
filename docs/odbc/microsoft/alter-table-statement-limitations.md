---
description: ALTER TABLE, instruction - limitations
title: Limitations des instructions ALTER TABLE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, ALTER TABLE statement limitations
- ALTER TABLE statement limitations [ODBC]
ms.assetid: f3e88f85-edf4-47cd-a822-292b106ddb34
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d36e5dfb2457a9d3453dbe3bc877c5d267731ea1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205751"
---
# <a name="alter-table-statement-limitations"></a>ALTER TABLE, instruction - limitations
Lorsque le pilote dBASE ou Paradox est utilisé, une fois qu’un index a été créé et qu’un nouvel enregistrement a été ajouté, la structure de la table ne peut pas être modifiée par l’instruction ALTER TABLE, sauf si l’index est supprimé et si le contenu de la table est supprimé.  
  
 Les instructions ALTER TABLE ne sont pas prises en charge pour Microsoft Excel ou les pilotes texte.  
  
> [!NOTE]  
>  Lorsque vous utilisez le pilote Paradox sans implémenter le Moteur de base de données Borland, les instructions ALTER TABLE ne sont pas prises en charge. seules les instructions Read et Append sont autorisées.
