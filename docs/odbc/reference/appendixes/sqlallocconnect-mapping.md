---
description: SQLAllocConnect, mappage
title: Mappage SQLAllocConnect | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLAllocConnect function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLAllocConnect
ms.assetid: ac89dd1f-c565-47cc-8fa3-6fa5f80b5d63
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3fd1659e4faaee713a2b9d942deb53fe00ea367c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202975"
---
# <a name="sqlallocconnect-mapping"></a>SQLAllocConnect, mappage
Quand une application appelle **SQLAllocConnect** via ODBC 3. *x* , l’appel à **SQLAllocConnect**(*henv*, *phdbc*) est mappé à **SQLAllocHandle** comme suit :  
  
1.  Le gestionnaire de pilotes alloue une connexion et le renvoie à l’application.  
  
2.  Lorsque l’application établit une connexion, le gestionnaire de pilotes appelle  
  
    ```  
    SQLAllocHandle(SQL_HANDLE_DBC, InputHandle, OutputHandlePtr)  
    ```  
  
     dans le pilote avec *InputHandle* défini sur *henv*, et *OutputHandlePtr* défini sur *phdbc*.
