---
description: Types de données C par défaut
title: Types de données C par défaut | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], pseudo-type identifiers
- pseudo-type identifiers [ODBC], about pseudo-type identifiers
- pseudo-type identifiers [ODBC]
ms.assetid: 229140ae-af8f-4ec8-9ccf-1e92360e0bac
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fd7f50e58fd84cec5938aacc598d12e6d733fcdc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194901"
---
# <a name="default-c-data-types"></a>Types de données C par défaut
Si une application spécifie SQL_C_DEFAULT dans **SQLBindCol**, **SQLGetData** ou **SQLBindParameter**, le pilote part du principe que le type de données C de la sortie ou de la mémoire tampon d’entrée correspond au type de données SQL de la colonne ou du paramètre auquel la mémoire tampon est liée.  
  
> [!IMPORTANT]  
>  Les applications interopérables ne doivent pas utiliser SQL_C_DEFAULT. Au lieu de cela, ils doivent toujours spécifier le type C de la mémoire tampon qu’ils utilisent. Cela est dû au fait que les pilotes ne peuvent pas toujours déterminer correctement le type C par défaut, pour les raisons suivantes :  
  
-   Si le SGBD promeut un type de données SQL d’une colonne ou d’un paramètre, le pilote ne peut pas déterminer le type de données SQL d’origine d’une colonne ou d’un paramètre. Par conséquent, il ne peut pas déterminer le type de données C par défaut correspondant.  
  
-   Si le pilote ne peut pas déterminer si une colonne ou un paramètre particulier est signé, comme c’est souvent le cas quand il est géré par le SGBD, le pilote ne peut pas déterminer si le type de données C par défaut correspondant doit être signé ou non signé.  
  
     Étant donné que SQL_C_DEFAULT est fourni uniquement comme une pratique de programmation, l’application ne perd aucune fonctionnalité lorsqu’elle spécifie le type de données C réel.  
  
 Une table qui indique le type de données C par défaut pour chaque type de données SQL est incluse dans la [conversion des données des types de données SQL en c](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md), plus loin dans cette annexe.
