---
description: Informations de référence sur l’API ODBC
title: Informations de référence sur l’API ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apitype: dllExport
ms.assetid: b7a49774-f458-44ce-9a04-a0457501405b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 60dab6c50856d9f4132b9ec3d066d15c99194814
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174654"
---
# <a name="odbc-api-reference"></a>Informations de référence sur l’API ODBC
Les rubriques de cette section décrivent chaque fonction ODBC par ordre alphabétique. Chaque fonction est définie en tant que fonction de langage de programmation C. Les descriptions sont les suivantes :  
  
-   Objectif  
  
-   Version ODBC  
  
-   Niveau de conformité de l’interface de commande standard  
  
-   Syntaxe  
  
-   Arguments  
  
-   Valeurs de retour  
  
-   Diagnostics  
  
-   Commentaires sur l’utilisation et l’implémentation  
  
-   Exemple de code  
  
-   Références aux fonctions connexes  
  
 Le niveau de conformité de l’interface CLI standard peut être l’un des suivants : ISO 92, Open Group, ODBC ou Deprecated. Une fonction marquée comme ISO 92-conforme figure également dans Open Group version 1, car Open Group est un sur-ensemble pur de ISO 92. Une fonction marquée comme Open Group-compatible s’affiche également dans ODBC 3. *x*, car ODBC 3. *x* est un sur-ensemble pur de Open Group version 1. Une fonction marquée comme compatible ODBC n’apparaît ni dans le standard. Une fonction marquée comme déconseillée a été dépréciée dans ODBC 3. *x*.  
  
 La gestion des informations de diagnostic est décrite dans la description de la fonction [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md) . Le texte associé aux valeurs SQLSTATE est inclus pour fournir une description de la condition, mais n’est pas destiné à prescrire un texte spécifique.  
  
> [!NOTE]  
>  Pour obtenir des informations spécifiques au pilote sur les fonctions ODBC, consultez la section relative au pilote.  
  
 Cette section contient les rubriques relatives aux fonctions suivantes :  
  
-   [SQLAllocConnect, fonction](../../../odbc/reference/syntax/sqlallocconnect-function.md)  
  
-   [SQLAllocEnv, fonction](../../../odbc/reference/syntax/sqlallocenv-function.md)  
  
-   [SQLAllocHandle, fonction](../../../odbc/reference/syntax/sqlallochandle-function.md)  
  
-   [SQLAllocStmt, fonction](../../../odbc/reference/syntax/sqlallocstmt-function.md)  
  
-   [Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)  
  
-   [Fonction SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)  
  
-   [Fonction SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)  
  
-   [SQLBulkOperations, fonction](../../../odbc/reference/syntax/sqlbulkoperations-function.md)  
  
-   [SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)  
  
-   [SQLCancelHandle, fonction](../../../odbc/reference/syntax/sqlcancelhandle-function.md)  
  
-   [SQLCloseCursor, fonction](../../../odbc/reference/syntax/sqlclosecursor-function.md)  
  
-   [Fonction SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)  
  
-   [SQLColAttributes, fonction](../../../odbc/reference/syntax/sqlcolattributes-function.md)  
  
-   [Fonction SQLColumnPrivileges](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)  
  
-   [Fonction SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)  
  
-   [SQLCompleteAsync, fonction](../../../odbc/reference/syntax/sqlcompleteasync-function.md)  
  
-   [SQLConnect, fonction](../../../odbc/reference/syntax/sqlconnect-function.md)  
  
-   [SQLCopyDesc, fonction](../../../odbc/reference/syntax/sqlcopydesc-function.md)  
  
-   [SQLDataSources, fonction](../../../odbc/reference/syntax/sqldatasources-function.md)  
  
-   [Fonction SQLDescribeCol](../../../odbc/reference/syntax/sqldescribecol-function.md)  
  
-   [Fonction SQLDescribeParam](../../../odbc/reference/syntax/sqldescribeparam-function.md)  
  
-   [SQLDisconnect, fonction](../../../odbc/reference/syntax/sqldisconnect-function.md)  
  
-   [SQLDriverConnect, fonction](../../../odbc/reference/syntax/sqldriverconnect-function.md)  
  
-   [SQLDrivers, fonction](../../../odbc/reference/syntax/sqldrivers-function.md)  
  
-   [Fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md)  
  
-   [SQLError, fonction](../../../odbc/reference/syntax/sqlerror-function.md)  
  
-   [SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)  
  
-   [SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)  
  
-   [SQLExtendedFetch, fonction](../../../odbc/reference/syntax/sqlextendedfetch-function.md)  
  
-   [SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)  
  
-   [Fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)  
  
-   [Fonction SQLForeignKeys](../../../odbc/reference/syntax/sqlforeignkeys-function.md)  
  
-   [SQLFreeConnect, fonction](../../../odbc/reference/syntax/sqlfreeconnect-function.md)  
  
-   [SQLFreeEnv, fonction](../../../odbc/reference/syntax/sqlfreeenv-function.md)  
  
-   [Fonction SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)  
  
-   [Fonction SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)  
  
-   [Fonction SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)  
  
-   [SQLGetConnectOption, fonction](../../../odbc/reference/syntax/sqlgetconnectoption-function.md)  
  
-   [SQLGetCursorName Function](../../../odbc/reference/syntax/sqlgetcursorname-function.md)  
  
-   [Fonction SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)  
  
-   [Fonction SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)  
  
-   [SQLGetDescRec, fonction](../../../odbc/reference/syntax/sqlgetdescrec-function.md)  
  
-   [Fonction SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)  
  
-   [SQLGetDiagRec, fonction](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)  
  
-   [SQLGetEnvAttr, fonction](../../../odbc/reference/syntax/sqlgetenvattr-function.md)  
  
-   [Fonction SQLGetFunctions](../../../odbc/reference/syntax/sqlgetfunctions-function.md)  
  
-   [Fonction SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)  
  
-   [Fonction SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)  
  
-   [SQLGetStmtOption, fonction](../../../odbc/reference/syntax/sqlgetstmtoption-function.md)  
  
-   [Fonction SQLGetTypeInfo](../../../odbc/reference/syntax/sqlgettypeinfo-function.md)  
  
-   [Fonction SQLMoreResults](../../../odbc/reference/syntax/sqlmoreresults-function.md)  
  
-   [Fonction SQLNativeSql](../../../odbc/reference/syntax/sqlnativesql-function.md)  
  
-   [Fonction SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md)  
  
-   [Fonction SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)  
  
-   [SQLParamData, fonction](../../../odbc/reference/syntax/sqlparamdata-function.md)  
  
-   [SQLParamOptions, fonction](../../../odbc/reference/syntax/sqlparamoptions-function.md)  
  
-   [Fonction SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)  
  
-   [Fonction SQLPrimaryKeys](../../../odbc/reference/syntax/sqlprimarykeys-function.md)  
  
-   [Fonction SQLProcedureColumns](../../../odbc/reference/syntax/sqlprocedurecolumns-function.md)  
  
-   [Fonction SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md)  
  
-   [Fonction SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)  
  
-   [SQLRowCount (fonction)](../../../odbc/reference/syntax/sqlrowcount-function.md)  
  
-   [Fonction SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)  
  
-   [SQLSetConnectOption, fonction](../../../odbc/reference/syntax/sqlsetconnectoption-function.md)  
  
-   [SQLSetCursorName, fonction](../../../odbc/reference/syntax/sqlsetcursorname-function.md)  
  
-   [SQLSetDescField, fonction](../../../odbc/reference/syntax/sqlsetdescfield-function.md)  
  
-   [SQLSetDescRec, fonction](../../../odbc/reference/syntax/sqlsetdescrec-function.md)  
  
-   [Fonction SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md)  
  
-   [SQLSetParam, fonction](../../../odbc/reference/syntax/sqlsetparam-function.md)  
  
-   [SQLSetPos, fonction](../../../odbc/reference/syntax/sqlsetpos-function.md)  
  
-   [SQLSetScrollOptions, fonction](../../../odbc/reference/syntax/sqlsetscrolloptions-function.md)  
  
-   [Fonction SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)  
  
-   [SQLSetStmtOption, fonction](../../../odbc/reference/syntax/sqlsetstmtoption-function.md)  
  
-   [Fonction SQLSpecialColumns](../../../odbc/reference/syntax/sqlspecialcolumns-function.md)  
  
-   [Fonction SQLStatistics](../../../odbc/reference/syntax/sqlstatistics-function.md)  
  
-   [Fonction SQLTablePrivileges](../../../odbc/reference/syntax/sqltableprivileges-function.md)  
  
-   [Fonction SQLTables](../../../odbc/reference/syntax/sqltables-function.md)  
  
-   [SQLTransact, fonction](../../../odbc/reference/syntax/sqltransact-function.md)
