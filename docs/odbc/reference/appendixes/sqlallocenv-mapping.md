---
description: SQLAllocEnv, mappage
title: Mappage SQLAllocEnv | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLAllocEnv function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLAllocEnv
ms.assetid: 4bb51845-ee91-4b97-9dd4-2fab977f2aec
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 134f517b8374aafa223da329fc53c2eeabeefbe4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202969"
---
# <a name="sqlallocenv-mapping"></a>SQLAllocEnv, mappage
Quand une application appelle **SQLAllocEnv** via un pilote ODBC *3. x* , l’appel à **SQLAllocEnv**(*phenv*) est mappé à **SQLAllocHandle** comme suit :  
  
1.  Le gestionnaire de pilotes alloue un handle d’environnement et le retourne à l’application. Le gestionnaire de pilotes appelle **SQLSetEnvAttr** pour définir l’attribut d’environnement SQL_ATTR_ODBC_VERSION sur SQL_OV_ODBC2.  
  
2.  Lorsque l’application établit la première connexion à un pilote, le gestionnaire de pilotes appelle  
  
    ```  
    SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, OutputHandlePtr)  
    ```  
  
     dans le pilote avec *OutputHandlePtr* défini sur *phenv*.
