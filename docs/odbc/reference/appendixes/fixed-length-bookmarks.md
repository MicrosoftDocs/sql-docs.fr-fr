---
description: Signets de longueur fixe
title: Fixed-Length les signets | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- backward compatibility [ODBC], bookmarks
- bookmarks [ODBC]
- compatibility [ODBC], bookmarks
- fixed-length bookmarks [ODBC]
ms.assetid: cbd8185e-fb03-408f-b80b-1a2e164534fd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2d5eb7acd97a83be0d150cec8b19a5a3e25a7bf6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194809"
---
# <a name="fixed-length-bookmarks"></a>Signets de longueur fixe
Si un pilote ODBC *3. x* doit fonctionner avec une application ODBC *2. x* qui utilise des signets de longueur fixe, le pilote doit prendre en charge les éléments suivants :  
  
-   SQL_UB_ON comme valeur pour l’option d’instruction SQL_USE_BOOKMARKS. (SQL_UB_ON est déconseillé dans ODBC *3. x*.)  
  
-   Option d’instruction SQL_GET_BOOKMARK.
