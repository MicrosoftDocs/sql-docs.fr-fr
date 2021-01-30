---
description: Type de données C pour les signets
title: Type de données de signet C | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- C data types [ODBC], bookmark C data type
- pseudo-type identifiers [ODBC], bookmark C data type
- data types [ODBC], pseudo-type identifiers
- bookmarks [ODBC]
- bookmark C data type [ODBC]
ms.assetid: add88e48-ada3-4c0c-a5ac-e78903d3ff41
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0df39afc58e241dd3736bca6f5fa45e31d102668
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212491"
---
# <a name="bookmark-c-data-type"></a>Type de données C pour les signets
Le type de données signet C permet à une application de récupérer un signet. Les types de signets C sont utilisés uniquement pour récupérer des valeurs de signet qui peuvent être de longueur variable. elles ne doivent pas être converties en d’autres types de données. Une application récupère un signet soit à partir de la colonne 0 du jeu de résultats avec **SQLBulkOperations** (avec l’opération SQL_ADD), **SQLFetch**, **SQLFetchScroll** ou **SQLGetData**. Pour plus d’informations, consultez [signets](../../../odbc/reference/develop-app/bookmarks-odbc.md).  
  
 Le tableau suivant répertorie la valeur de *CType* pour le type de données Bookmark c, le type de données c ODBC qui implémente le type de données Bookmark c et la définition de ce type de données à partir de SQL. H.  
  
> [!NOTE]
>  Le type de données SQL_C_BOOKMARK est déconseillé. Les applications ODBC *3. x* ne doivent pas utiliser SQL_C_BOOKMARK. Les pilotes ODBC *3. x* doivent prendre en charge SQL_C_BOOKMARK uniquement s’ils souhaitent travailler avec des applications ODBC *2. x* qui l’utilisent. Le gestionnaire de pilotes mappe SQL_C_VARBOOKMARK à SQL_C_BOOKMARK lorsqu’une application fonctionne avec un pilote ODBC *2. x* .  
  
|Identificateur de type C|Typedef C ODBC|Type C|  
|-----------------------|--------------------|------------|  
|SQL_C_BOOKMARK<br />(Déconseillé)|Signet|unsigned long int|  
|SQL_C_VARBOOKMARK|SQLCHAR|unsigned char *|
