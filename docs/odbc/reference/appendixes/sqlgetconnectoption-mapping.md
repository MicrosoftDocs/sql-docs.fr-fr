---
description: SQLGetConnectOption, mappage
title: Mappage Sqlgetconnectoption, | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLGetConnectOption
- SQLGetConnectOption function [ODBC], mapping
ms.assetid: e3792fe4-a955-473a-a297-c1b2403660c4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cdc5f2e9249e262d0d3da9a69607861bfe64bc25
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202795"
---
# <a name="sqlgetconnectoption-mapping"></a>SQLGetConnectOption, mappage
Quand une application appelle **sqlgetconnectoption,** via un pilote ODBC *3. x* , l’appel à  
  
```  
SQLGetConnectOption(hdbc, fOption, pvParam)   
```  
  
 est mappé comme suit :  
  
-   Si *fOption* indique une option de connexion définie par ODBC qui retourne une chaîne, le gestionnaire de pilotes appelle  
  
    ```  
    SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength, NULL)  
    ```  
  
-   Si *fOption* indique une option de connexion définie par ODBC qui retourne une valeur entière de 32 bits, le gestionnaire de pilotes appelle  
  
    ```  
    SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, 0, NULL)  
    ```  
  
-   Si *fOption* indique une option d’instruction définie par le pilote, le gestionnaire de pilotes appelle  
  
    ```  
    SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength, NULL)  
    ```  
  
 Dans les trois cas précédents, l’argument *ConnectionHandle* est défini sur la valeur de *hdbc*, l’argument d' *attribut* est défini sur la valeur de *fOption* et l’argument *ValuePtr* est défini sur la même valeur que *pvParam*.  
  
 Pour les options de connexion de chaîne définies par ODBC, le gestionnaire de pilotes définit l’argument *BufferLength* dans l’appel à **SQLGetConnectAttr** sur la longueur maximale prédéfinie (SQL_MAX_OPTION_STRING_LENGTH); pour une option de connexion sans chaîne, *BufferLength* a la valeur 0.  
  
 Pour un pilote ODBC *3. x* , le gestionnaire de pilotes ne vérifie plus si l' *option* est comprise entre SQL_CONN_OPT_MIN et SQL_CONN_OPT_MAX, ou est supérieure à SQL_CONNECT_OPT_DRVR_START. Le pilote doit vérifier la validité des valeurs d’option.
