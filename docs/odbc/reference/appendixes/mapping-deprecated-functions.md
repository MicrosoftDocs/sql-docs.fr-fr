---
description: Mappage des fonctions dépréciées
title: Mappage des fonctions dépréciées | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], about mapping deprecated functions
- backward compatibility [ODBC], mapping deprecated functions
- deprecated functions [ODBC]
- compatibility [ODBC], mapping deprecated functions
- functions [ODBC], mapping deprecated functions
- mapping deprecated functions [ODBC]
ms.assetid: ee462617-1d79-4c88-afeb-b129cff34cc6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5f52a5f0a5c46c2f4de81f99440d657ae68b925a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206813"
---
# <a name="mapping-deprecated-functions"></a>Mappage des fonctions dépréciées
Cette section décrit comment les fonctions déconseillées sont mappées par le gestionnaire de pilotes ODBC *3. x* pour garantir la compatibilité descendante des pilotes ODBC *3. x* utilisés avec les applications ODBC *2. x* . Le gestionnaire de pilotes effectue ce mappage, quelle que soit la version de l’application. Étant donné que chacune des fonctions ODBC *2. x* de la liste suivante est mappée à la fonction ODBC *3. x* correspondante quand elle est appelée dans un pilote ODBC *3. x* , le pilote ODBC *3. x* n’a pas à implémenter les fonctions ODBC *2. x* .  
  
 Le mappage de la liste est déclenché lorsque le pilote est un pilote ODBC *3. x* et que le pilote ne prend pas en charge la fonction qui est mappée.  
  
 Le tableau suivant répertorie toutes les fonctionnalités dupliquées qui ont été introduites dans ODBC *3. x*.  
  
|ODBC *2. x,* fonction|ODBC *3. x,* fonction|  
|-------------------------|-------------------------|  
|**SQLAllocConnect**|**SQLAllocHandle**|  
|**SQLAllocEnv**|**SQLAllocHandle**|  
|**SQLAllocStmt,**|**SQLAllocHandle**|  
|**SQLBindParam**[1]|**SQLBindParameter**|  
|**SQLColAttributes**|**SQLColAttribute**|  
|**SQLError**|**SQLGetDiagRec**|  
|**Sqlfreeconnect,**|**SQLFreeHandle**|  
|**Sqlfreeenv,**|**SQLFreeHandle**|  
|**SQLFreeStmt** avec une *option* de SQL_DROP|**SQLFreeHandle**|  
|**Sqlgetconnectoption,**|**SQLGetConnectAttr**|  
|**SQLGetStmtOption**|**SQLGetStmtAttr**|  
|**SQLParamOptions,**|**SQLSetStmtAttr**|  
|**SQLSetConnectOption**|**SQLSetConnectAttr**|  
|**SQLSetParam,**[2]|**SQLBindParameter**|  
|**SQLSetScrollOption**|**SQLSetStmtAttr**|  
|**SQLSetStmtOption**|**SQLSetStmtAttr**|  
|**SQLTransact**|**SQLEndTran**|  
  
 [1] même si cette fonction n’existait pas dans ODBC *2. x*, elle se trouve dans les normes de groupe et ISO ouvertes.  
  
 [2] il s’agit d’une fonction ODBC 1,0.  
  
 Cette section contient les rubriques suivantes :  
  
-   [SQLAllocConnect, mappage](../../../odbc/reference/appendixes/sqlallocconnect-mapping.md)  
  
-   [SQLAllocEnv, mappage](../../../odbc/reference/appendixes/sqlallocenv-mapping.md)  
  
-   [SQLAllocStmt, mappage](../../../odbc/reference/appendixes/sqlallocstmt-mapping.md)  
  
-   [SQLBindParam, mappage](../../../odbc/reference/appendixes/sqlbindparam-mapping.md)  
  
-   [SQLColAttributes, mappage](../../../odbc/reference/appendixes/sqlcolattributes-mapping.md)  
  
-   [SQLError, mappage](../../../odbc/reference/appendixes/sqlerror-mapping.md)  
  
-   [SQLFreeConnect, mappage](../../../odbc/reference/appendixes/sqlfreeconnect-mapping.md)  
  
-   [SQLFreeEnv, mappage](../../../odbc/reference/appendixes/sqlfreeenv-mapping.md)  
  
-   [SQLFreeStmt, mappage](../../../odbc/reference/appendixes/sqlfreestmt-mapping.md)  
  
-   [SQLGetConnectOption, mappage](../../../odbc/reference/appendixes/sqlgetconnectoption-mapping.md)  
  
-   [SQLGetStmtOption, mappage](../../../odbc/reference/appendixes/sqlgetstmtoption-mapping.md)  
  
-   [SQLInstallTranslator, mappage](../../../odbc/reference/appendixes/sqlinstalltranslator-mapping.md)  
  
-   [SQLParamOptions, mappage](../../../odbc/reference/appendixes/sqlparamoptions-mapping.md)  
  
-   [SQLSetConnectOption, mappage](../../../odbc/reference/appendixes/sqlsetconnectoption-mapping.md)  
  
-   [SQLSetParam, mappage](../../../odbc/reference/appendixes/sqlsetparam-mapping.md)  
  
-   [SQLSetScrollOptions, mappage](../../../odbc/reference/appendixes/sqlsetscrolloptions-mapping.md)  
  
-   [SQLSetStmtOption, mappage](../../../odbc/reference/appendixes/sqlsetstmtoption-mapping.md)  
  
-   [SQLTransact, mappage](../../../odbc/reference/appendixes/sqltransact-mapping.md)
