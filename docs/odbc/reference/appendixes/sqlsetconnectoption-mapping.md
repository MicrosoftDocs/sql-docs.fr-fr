---
description: SQLSetConnectOption, mappage
title: SQLSetConnectOption mappage | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetConnectOption function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLSetConnectOption
ms.assetid: a1b325cf-0c42-41c1-b141-b5a4fee7e708
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 919220fed87878bf0bcf23359f0714f702f9a6a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202653"
---
# <a name="sqlsetconnectoption-mapping"></a>SQLSetConnectOption, mappage
Quand ODBC 2. l’application *x* appelle **SQLSetConnectOption** via un pilote ODBC 3 *. x* , l’appel à  
  
```  
SQLSetConnectOption(hdbc, fOption, vParam)  
```  
  
 se produit comme suit :  
  
-   Si *fOption* indique un attribut de connexion défini par ODBC qui requiert une chaîne, le gestionnaire de pilotes appelle  
  
    ```  
    SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, SQL_NTS)  
    ```  
  
-   Si *fOption* indique un attribut de connexion défini par ODBC qui retourne une valeur entière de 32 bits, le gestionnaire de pilotes appelle  
  
    ```  
    SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, 0)  
    ```  
  
-   Si *fOption* indique un attribut de connexion défini par le pilote, le gestionnaire de pilotes appelle  
  
    ```  
    SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength)  
    ```  
  
 Dans les trois cas précédents, l' *argument ConnectionHandle* est défini sur la valeur de *hdbc*, l' *argument attribute* est défini sur la valeur de *fOption* et l’argument *ValuePtr* est défini sur la même valeur que *vParam*.  
  
 Étant donné que le gestionnaire de pilotes ne sait pas si l’attribut de connexion défini par le pilote a besoin d’une chaîne ou d’une valeur entière de 32 bits, il doit transmettre une valeur valide pour l’argument *BufferLength* de **SQLSetConnectAttr**. Si le pilote a défini une sémantique spéciale pour les attributs de connexion définis par le pilote et doit être appelée à l’aide de **SQLSetConnectOption**, il doit prendre en charge **SQLSetConnectOption**.  
  
 Si ODBC 2. *x* appelle **SQLSetConnectOption** pour définir une option d’instruction spécifique au pilote dans un pilote ODBC 3 *. x* , et l’option a été définie dans ODBC 2. *x* version du pilote, une nouvelle constante de manifeste doit être définie pour l’option dans le pilote ODBC 3 *. x* . Si l’ancienne constante de manifeste est utilisée dans l’appel à **SQLSetConnectOption**, le gestionnaire de pilotes appellera **SQLSetConnectAttr** avec l’argument **StringLength** défini sur 0.  
  
 Pour un pilote ODBC 3 *. x* , le gestionnaire de pilotes ne vérifie plus si la valeur de *fOption* est comprise entre SQL_CONN_OPT_MIN et SQL_CONN_OPT_MAX, ou est supérieur à SQL_CONNECT_OPT_DRVR_START.  
  
## <a name="setting-statement-options-on-the-connection-level"></a>Définition des options d’instruction au niveau de la connexion  
 Dans ODBC 2. *x*, une application peut appeler **SQLSetConnectOption** pour définir une option d’instruction. Lorsque cela est fait, le pilote établit l’option d’instruction en tant que valeur par défaut pour toutes les instructions allouées ultérieurement pour cette connexion. Elle est définie par le pilote si le pilote définit l’option d’instruction pour les instructions existantes associées à la connexion spécifiée.  
  
 Cette capacité a été dépréciée dans ODBC 3 *. x*. Les pilotes ODBC 3 *. x* doivent uniquement prendre en charge le paramétrage ODBC 2. *x* au niveau de la connexion, s’ils souhaitent utiliser ODBC 2. *x* applications qui le font. Les applications ODBC 3 *. x* ne doivent jamais définir d’attributs d’instruction au niveau de la connexion. Les attributs d’instruction ODBC 3 *. x* ne peuvent pas être définis au niveau de la connexion, à l’exception des attributs SQL_ATTR_METADATA_ID et SQL_ATTR_ASYNC_ENABLE, qui sont des attributs de connexion et des attributs d’instruction, et peuvent être définis au niveau de la connexion ou au niveau de l’instruction.
