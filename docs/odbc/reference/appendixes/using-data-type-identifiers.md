---
description: Utilisation d’identificateurs de types de données
title: Utilisation des identificateurs de type de données | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], identifiers
- identifiers [ODBC], data types
ms.assetid: 467e0c0c-a818-4737-8a24-3d8e15c7e162
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cd38f5ddb62a28bc3ec2658dca621769e13ab481
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202437"
---
# <a name="using-data-type-identifiers"></a>Utilisation d’identificateurs de types de données
Les applications utilisent des identificateurs de type de données de deux façons : pour décrire leurs mémoires tampons au pilote et pour récupérer les métadonnées sur le jeu de résultats à partir du pilote afin qu’elles puissent déterminer le type de tampons C à utiliser pour stocker les données. Les applications appellent les fonctions suivantes pour effectuer ces tâches :  
  
-   **SQLBindParameter**, **SQLBindCol** et **SQLGetData** -pour décrire le type de données C des mémoires tampons d’application.  
  
-   **SQLBindParameter** -pour décrire le type de données SQL des paramètres dynamiques.  
  
-   **SQLColAttribute** et **SQLDescribeCol** -pour récupérer les types de données SQL des colonnes de jeu de résultats.  
  
-   **SQLDescribeParameter** : pour récupérer les types de données SQL des paramètres.  
  
-   **SQLColumns**, **SQLProcedureColumns** et **SQLSpecialColumns** -pour récupérer les types de données SQL des diverses informations de schéma  
  
-   **SQLGetTypeInfo** : pour récupérer la liste des types de données pris en charge  
  
 Les identificateurs de type de données sont stockés dans le champ SQL_DESC_CONCISE_TYPE d’un descripteur. Les fonctions de descripteur **SQLSetDescField** et **SQLSetDescRec** peuvent être utilisées avec les types appropriés pour effectuer les tâches répertoriées dans la liste précédente. Pour plus d’informations, consultez [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).
