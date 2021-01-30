---
description: SQL_ARD_TYPE
title: SQL_ARD_TYPE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], pseudo-type identifiers
- pseudo-type identifiers [ODBC], SQL_ARD_TYPE
- SQL_ARD_TYPE [ODBC]
ms.assetid: 8d87ca10-f955-4284-8689-e9f4cc31e7ae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 43d8ce804fc66fd8d9a305868cf1a8b22cf8ff7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187075"
---
# <a name="sql_ard_type"></a>SQL_ARD_TYPE
L’identificateur de type SQL_ARD_TYPE est utilisé pour indiquer que les données d’une mémoire tampon seront du type spécifié dans le champ SQL_DESC_CONCISE_TYPE de l’ARD. SQL_ARD_TYPE est entré dans l’argument *TargetType* d’un appel à **SQLGetData** au lieu d’un type de données spécifique et permet à une application de modifier le type de données de la mémoire tampon en modifiant le champ du descripteur. Cette valeur lie le type de données de la mémoire tampon *\* TargetValuePtr* au champ de descripteur. (SQL_ARD_TYPE n’est pas entré dans un appel à **SQLBindCol** ou **SQLBindParameter** , car le type de la mémoire tampon liée est déjà lié aux champs SQL_DESC_TYPE et SQL_DESC_CONCISE_TYPE et peut être modifié à tout moment en modifiant l’un de ces champs.)  
  
 L’identificateur de type SQL_ARD_TYPE peut être utilisé pour spécifier des valeurs autres que celles par défaut pour la précision de début et la précision en secondes des types de données d’intervalle, ainsi que des valeurs de précision et d’échelle pour le type de données SQL_C_NUMERIC. Pour plus d’informations, consultez substitution de la [précision de début et de seconde par défaut pour les types de données d’intervalle](../../../odbc/reference/appendixes/overriding-default-leading-and-seconds-precision-for-interval-data-types.md) et substitution de la [précision et de l’échelle par défaut pour les types de données numériques](../../../odbc/reference/appendixes/overriding-default-precision-and-scale-for-numeric-data-types.md), plus loin dans cette annexe.
