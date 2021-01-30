---
description: SQLGetStmtOption, mappage
title: Mappage SQLGetStmtOption | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLGetStmtOption function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLGetStmtOption
ms.assetid: fa599517-3f3e-4dad-a65a-b8596ae3f330
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: aa5d59afe92dbbf6a2e0d2d2cc08abb1a529fe5b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202719"
---
# <a name="sqlgetstmtoption-mapping"></a>SQLGetStmtOption, mappage
Quand une application appelle **SQLGetStmtOption** à un pilote ODBC *3. x* qui ne la prend pas en charge, l’appel à  
  
```  
SQLGetStmtOption(hstmt, fOption, pvParam)  
```  
  
 se produit comme suit :  
  
-   Si *fOption* indique une option d’instruction définie par ODBC qui retourne une chaîne, le gestionnaire de pilotes appelle  
  
    ```  
    SQLGetStmtAttr(StatementHandle, Attribute, ValuePtr, BufferLength, NULL)  
    ```  
  
-   Si *fOption* indique une option d’instruction définie par ODBC qui retourne une valeur entière de 32 bits, le gestionnaire de pilotes appelle  
  
    ```  
    SQLGetStmtAttr(StatementHandle, Attribute, ValuePtr, 0, NULL)  
    ```  
  
-   Si *fOption* indique une option d’instruction définie par le pilote, le gestionnaire de pilotes appelle  
  
    ```  
    SQLGetStmtAttr(StatementHandle, Attribute, ValuePtr, BufferLength, NULL)  
    ```  
  
 Dans les trois cas précédents, l’argument *StatementHandle* est défini sur la valeur dans *HSTMT*, l’argument d' *attribut* est défini sur la valeur de *fOption* et l’argument *ValuePtr* est défini sur la même valeur que *pvParam*.  
  
 Pour les options de connexion de chaîne définies par ODBC, le gestionnaire de pilotes définit l’argument *BufferLength* dans l’appel à **SQLGetConnectAttr** sur la longueur maximale prédéfinie (SQL_MAX_OPTION_STRING_LENGTH); pour une option de connexion sans chaîne, *BufferLength* a la valeur 0.  
  
 L’option d’instruction SQL_GET_BOOKMARK est dépréciée dans ODBC *3. x*. Pour qu’un pilote ODBC *3. x* fonctionne avec les applications ODBC *2. x* qui utilisent SQL_GET_BOOKMARK, il doit prendre en charge les SQL_GET_BOOKMARK. Pour qu’un pilote ODBC *3. x* fonctionne avec les applications ODBC *2. x* , il doit prendre en charge la définition de SQL_USE_BOOKMARKS pour SQL_UB_ON et doit exposer des signets de longueur fixe. Si un pilote ODBC *3. x* prend en charge uniquement les signets de longueur variable, pas les signets de longueur fixe, il doit retourner SQLSTATE HYC00 (fonctionnalité facultative non implémentée) si une application ODBC *2. x* tente de définir SQL_USE_BOOKMARKS sur SQL_UB_ON.  
  
 Pour un pilote ODBC *3. x* , le gestionnaire de pilotes ne vérifie plus si l' *option* est comprise entre SQL_STMT_OPT_MIN et SQL_STMT_OPT_MAX, ou est supérieure à SQL_CONNECT_OPT_DRVR_START. Le pilote doit vérifier cela.
