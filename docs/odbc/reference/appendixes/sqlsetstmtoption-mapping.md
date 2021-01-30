---
description: SQLSetStmtOption, mappage
title: Mappage SQLSetStmtOption | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLSetStmtOption
- SQLSetStmtOption function [ODBC], mapping
ms.assetid: 6a9921aa-8a53-4668-9b13-87164062f1e5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8907de6d6ac80737ba0bb47ca3c6954a482c857f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202574"
---
# <a name="sqlsetstmtoption-mapping"></a>SQLSetStmtOption, mappage
Quand une application appelle **SQLSetStmtOption** via un pilote ODBC *3. x* , l’appel à  
  
```  
SQLSetStmtOption(StatementHandle, fOption, vParam)  
```  
  
 se produit comme suit :  
  
-   Si *fOption* indique un attribut d’instruction défini par ODBC qui est une chaîne, le gestionnaire de pilotes appelle  
  
    ```  
    SQLSetStmtAttr(StatementHandle, fOption, ValuePtr, SQL_NTS)  
    ```  
  
-   Si *fOption* indique un attribut d’instruction défini par ODBC qui retourne une valeur entière de 32 bits, le gestionnaire de pilotes appelle  
  
    ```  
    SQLSetStmtAttr(StatementHandle, fOption, ValuePtr, 0)  
    ```  
  
-   Si *fOption* indique un attribut d’instruction définie par le pilote, le gestionnaire de pilotes appelle  
  
    ```  
    SQLSetStmtAttr(StatementHandle, fOption, ValuePtr, BufferLength)  
    ```  
  
 Dans les trois cas précédents, l’argument **StatementHandle** est défini sur la valeur dans *HSTMT*, l’argument d' *attribut* est défini sur la valeur de *fOption* et l’argument *ValuePtr* est défini sur la valeur de *vParam*.  
  
 Étant donné que le gestionnaire de pilotes ne sait pas si l’attribut d’instruction définie par le pilote a besoin d’une chaîne ou d’une valeur entière de 32 bits, il doit transmettre une valeur valide pour l’argument *StringLength* de **SQLSetStmtAttr**. Si le pilote a défini une sémantique spéciale pour les attributs d’instruction définie par le pilote et doit être appelée à l’aide de **SQLSetStmtOption**, il doit prendre en charge **SQLSetStmtOption**.  
  
 Si une application appelle **SQLSetStmtOption** pour définir une option d’instruction spécifique au pilote dans un pilote ODBC *3. x* et que l’option a été définie dans une version ODBC *2. x* du pilote, une nouvelle constante de manifeste doit être définie pour l’option dans le pilote ODBC *3. x* . Si l’ancienne constante de manifeste est utilisée dans l’appel à **SQLSetStmtOption**, le gestionnaire de pilotes appellera **SQLSetStmtAttr** avec l’argument *StringLength* défini sur 0.  
  
 Quand une application appelle **SQLSetStmtAttr** pour définir SQL_ATTR_USE_BOOKMARKS à SQL_UB_ON dans un pilote ODBC *3. x* , l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS est défini sur SQL_UB_FIXED. SQL_UB_ON est la même constante que SQL_UB_FIXED. Le gestionnaire de pilotes passe SQL_UB_FIXED au pilote. SQL_UB_FIXED est déconseillé dans ODBC *3. x*, mais un pilote ODBC *3. x* doit l’implémenter pour fonctionner avec les applications ODBC *2. x* qui utilisent des signets de longueur fixe.  
  
 Pour un pilote ODBC *3. x* , le gestionnaire de pilotes ne vérifie plus si l' *option* est comprise entre SQL_STMT_OPT_MIN et SQL_STMT_OPT_MAX, ou est supérieure à SQL_CONNECT_OPT_DRVR_START.
