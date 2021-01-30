---
description: SQLSetScrollOptions, mappage
title: Mappage SQLSetScrollOptions | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetScrollOptions function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLSetScrollOptions
ms.assetid: a0fa4510-8891-4a61-a867-b2555bc35f05
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 105443a2b33a1903f1f22affef1c6ca70ee2cfdf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202595"
---
# <a name="sqlsetscrolloptions-mapping"></a>SQLSetScrollOptions, mappage
Quand une application appelle **SQLSetScrollOptions** via un pilote ODBC *3. x* et que le pilote ne prend pas en charge **SQLSetScrollOptions**, l’appel à  
  
```  
SQLSetScrollOptions(StatementHandle, Concurrency, KeysetSize, RowsetSize)  
```  
  
 se produit comme suit :  
  
-   Un appel à  
  
    ```  
    SQLGetInfo(ConnectionHandle, InfoType, InfoValuePtr, BufferLength, StringLengthPtr)  
    ```  
  
     avec l’argument *infotype* défini sur l’une des valeurs du tableau suivant, en fonction de la valeur de l’argument *keyset* dans **SQLSetScrollOptions**.  
  
    |*Configure (argument)*|*Argument InfoType*|  
    |---------------------------|-------------------------|  
    |SQL_SCROLL_FORWARD_ONLY|SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES2|  
    |SQL_SCROLL_STATIC|SQL_STATIC_CURSOR_ATTRIBUTES2|  
    |SQL_SCROLL_KEYSET_DRIVEN|SQL_KEYSET_CURSOR_ATTRIBUTES2|  
    |SQL_SCROLL_DYNAMIC|SQL_DYNAMIC_CURSOR_ATTRIBUTES2|  
    |Valeur supérieure à l’argument *RowsetSize*|SQL_KEYSET_CURSOR_ATTRIBUTES2|  
  
     Si la valeur de l' *argument* bapage n’est pas répertoriée dans le tableau précédent, l’appel à **SQLSetScrollOptions** retourne SQLState S1107 (valeur de ligne hors limites) et aucune des étapes suivantes n’est effectuée.  
  
     Le gestionnaire de pilotes vérifie ensuite si le bit approprié est défini dans la valeur **InfoValuePtr* retournée par l’appel à **SQLGetInfo**, en fonction de la valeur de l’argument d' *accès concurrentiel* dans **SQLSetScrollOptions**.  
  
    |Argument d' *accès concurrentiel*|Paramètre d' *infotype*|  
    |----------------------------|------------------------|  
    |SQL_CONCUR_READ_ONLY|SQL_CA2_READ_ONLY_CONCURRENCY|  
    |SQL_CONCUR_LOCK|SQL_CA2_LOCK_CONCURRENCY|  
    |SQL_CONCUR_ROWVER|SQL_CA2_ROWVER_CONCURRENCY|  
    |SQL_CONCUR_VALUES|SQL_CA2_VALUES_CONCURRENCY|  
  
     Si l’argument d' *accès concurrentiel* n’est pas l’une des valeurs du tableau précédent, l’appel à **SQLSetScrollOptions** retourne SQLState S1108 (option d’accès concurrentiel hors limites) et aucune des étapes suivantes n’est effectuée. Si le bit approprié (comme indiqué dans le tableau précédent) n’est pas défini dans **InfoValuePtr* sur l’une des valeurs correspondant à l’argument d' *accès concurrentiel* , l’appel à **SQLSetScrollOptions** retourne SQLState S1C00 (pilote non conforme) et aucune des étapes suivantes n’est effectuée.  
  
-   Un appel à  
  
    ```  
    SQLSetStmtAttr(StatementHandle, SQL_ATTR_CURSOR_TYPE, ValuePtr, 0)  
    ```  
  
     avec *\* ValuePtr* défini sur l’une des valeurs du tableau suivant, en fonction de la valeur de l’argument *keyset* dans **SQLSetScrollOptions**.  
  
    |*Configure* (argument)|*\*ValuePtr*|  
    |---------------------------|------------------|  
    |SQL_SCROLL_FORWARD_ONLY|SQL_CURSOR_FORWARD_ONLY|  
    |SQL_SCROLL_STATIC|SQL_CURSOR_STATIC|  
    |SQL_SCROLL_KEYSET_DRIVEN|SQL_CURSOR_KEYSET_DRIVEN|  
    |SQL_SCROLL_DYNAMIC|SQL_CURSOR_DYNAMIC|  
    |Valeur supérieure à l’argument *RowsetSize*|SQL_CURSOR_KEYSET_DRIVEN|  
  
-   Un appel à  
  
    ```  
    SQLSetStmtAttr(StatementHandle, SQL_ATTR_CONCURRENCY, ValuePtr, 0)  
    ```  
  
     avec *\* ValuePtr* défini sur l’argument d' *accès concurrentiel* dans **SQLSetScrollOptions**.  
  
-   Si l’argument *deplace* dans l’appel à **SQLSetScrollOptions** est positif, un appel à  
  
    ```  
    SQLSetStmtAttr(StatementHandle, SQL_ATTR_KEYSET_SIZE, ValuePtr, 0)  
    ```  
  
     avec *\* ValuePtr* défini *sur l’argument keyset dans* **SQLSetScrollOptions**.  
  
-   Un appel à  
  
    ```  
    SQLSetStmtAttr(StatementHandle, SQL_ROWSET_SIZE, ValuePtr, 0)  
    ```  
  
     avec *\* ValuePtr* défini sur l’argument *RowsetSize* dans **SQLSetScrollOptions**.  
  
    > [!NOTE]  
    >  Quand le gestionnaire de pilotes mappe **SQLSetScrollOptions** pour une application qui utilise un pilote ODBC *3. x* qui ne prend pas en charge **SQLSetScrollOptions**, le gestionnaire de pilotes définit l’option d’instruction SQL_ROWSET_SIZE, et non l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE, sur l’argument *RowsetSize* dans **SQLSetScrollOption**. Par conséquent, **SQLSetScrollOptions** ne peut pas être utilisé par une application lors de l’extraction de plusieurs lignes par un appel à **SQLFetch** ou **SQLFetchScroll**. Il peut être utilisé uniquement lors de l’extraction de plusieurs lignes par un appel à **SQLExtendedFetch**.
