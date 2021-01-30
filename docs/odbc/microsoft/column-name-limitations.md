---
description: Limitations des noms de colonne
title: Limitations des noms de colonnes | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- desktop database drivers [ODBC], column names
- ODBC desktop database drivers [ODBC], column names
ms.assetid: 5a339f61-c52f-40ad-8deb-d785f72753d4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 46719ea0a0ff34e103252678e25242f010d9bd37
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190649"
---
# <a name="column-name-limitations"></a>Limitations des noms de colonne
Les noms de colonnes peuvent contenir des caractères valides (par exemple, des espaces). Si les noms de colonne contiennent des caractères à l’exception des lettres, des chiffres et des traits de soulignement, le nom doit être délimité en le mettant entre guillemets (').  
  
 Lorsque le pilote Microsoft Access ou Microsoft Excel est utilisé, les noms de colonnes sont limités à 64 caractères, et les noms plus longs génèrent une erreur. Lorsque le pilote Paradox est utilisé, le nom de colonne maximum est de 25 caractères. Lorsque le pilote de texte est utilisé, le nom de colonne maximum est de 64 caractères et les noms plus longs sont tronqués.  
  
 Lorsque le pilote dBASE est utilisé, les caractères avec une valeur ASCII supérieure à 127 sont convertis en traits de soulignement.  
  
 Lorsque le pilote Microsoft Excel est utilisé, si des noms de colonnes sont présents, ils doivent figurer dans la première ligne. Un nom dans Microsoft Excel utiliserait le caractère «  ! » doit être placé entre guillemets ('). Le caractère «  ! » est converti en caractère « $ », car le caractère «  ! » n’est pas légal dans un nom ODBC, même si le nom est placé entre guillemets. Tous les autres caractères Microsoft Excel valides (à l’exception du caractère de barre verticale (&#124;)) peuvent être utilisés dans un nom de colonne, y compris des espaces. Un identificateur délimité doit être utilisé pour un nom de colonne Microsoft Excel afin d’inclure un espace. Les noms de colonnes non spécifiés seront remplacés par des noms générés par le pilote, par exemple, « col1 » pour la première colonne.  
  
 Le caractère de barre verticale (&#124;) ne peut pas être utilisé dans un nom de colonne, que le nom soit placé entre guillemets ou non.  
  
 Lorsque le pilote de texte est utilisé, le pilote fournit un nom par défaut si aucun nom de colonne n’est spécifié. Par exemple, le pilote appelle la première colonne F1, la deuxième colonne F2, et ainsi de suite.
