---
description: SQLFreeConnect, mappage
title: Mappage Sqlfreeconnect, | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLFreeConnect
- SQLFreeConnect function [ODBC], mapping
ms.assetid: 8a844538-93c0-4709-bab6-35c45e771d80
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e63a4609636d1fbea5ddf83ff629bf50a827cd4a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202835"
---
# <a name="sqlfreeconnect-mapping"></a>SQLFreeConnect, mappage
Quand une application appelle **sqlfreeconnect,** via un pilote ODBC *3. x* , l’appel à  
  
```  
SQLFreeConnect(hdbc)   
```  
  
 est mappé à  
  
```  
SQLFreeHandle(SQL_HANDLE_DBC,Handle)  
```  
  
 avec l’argument *descripteur* défini sur la valeur de *hdbc*.
