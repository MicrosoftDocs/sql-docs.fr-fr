---
description: Limitations des identificateurs
title: Limitations des identificateurs | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC]
- desktop database drivers [ODBC]
ms.assetid: b3466382-71cb-4f82-8318-092a8fcef3df
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 71141e8f695b4ac6e60e6aecd70648f412807abb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190929"
---
# <a name="identifiers-limitations"></a>Limitations des identificateurs
Si un identificateur contient un espace ou un symbole spécial, l’identificateur doit être placé entre guillemets. Un nom valide est une chaîne de plus de 64 caractères, dont le premier caractère ne doit pas être un espace. Les noms valides ne peuvent pas contenir de caractères de contrôle ou les caractères spéciaux suivants : ' &#124; # * ? [ ] . ! $ .  
  
 N’utilisez pas les mots réservés listés dans la grammaire SQL de l’annexe C du *Guide de référence du programmeur ODBC* (ou la forme abrégée de ces mots réservés) en tant qu’identificateurs (autrement dit, les noms de table ou de colonne), à moins que vous n’entouriez le mot placé entre guillemets (').
