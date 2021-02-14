---
description: Fonction SQLGetFunctions
title: SQLGetFunctions fonction) | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetFunctions
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLGetFunctions
helpviewer_keywords:
- SQLGetFunctions function [ODBC]
ms.assetid: 0451d2f9-0f4f-46ba-b252-670956a52183
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: edc7b7875e3ee3f7512ee19a62d68a50a1617436
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081360"
---
# <a name="sqlgetfunctions-function"></a>Fonction SQLGetFunctions
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLGetFunctions** renvoie des informations indiquant si un pilote prend en charge une fonction ODBC spécifique. Cette fonction est implémentée dans le gestionnaire de pilotes ; elle peut également être implémentée dans les pilotes. Si un pilote implémente **SQLGetFunctions**, le gestionnaire de pilotes appelle la fonction dans le pilote. Dans le cas contraire, elle exécute la fonction elle-même.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLGetFunctions(  
     SQLHDBC           ConnectionHandle,  
     SQLUSMALLINT      FunctionId,  
     SQLUSMALLINT *    SupportedPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *ConnectionHandle*  
 [Entrée] Handle de connexion.  
  
 *FunctionId*  
 Entrée Valeur **#define** qui identifie la fonction ODBC digne d’intérêt ; **SQL_API_ODBC3_ALL_FUNCTIONS orSQL_API_ALL_FUNCTIONS**. **SQL_API_ODBC3_ALL_FUNCTIONS** est utilisé par une application ODBC 3 *. x* pour déterminer la prise en charge de ODBC 3 *. x* et des fonctions antérieures. **SQL_API_ALL_FUNCTIONS** est utilisé par une application ODBC 2 *. x* pour déterminer la prise en charge de ODBC 2 *. x* et des fonctions antérieures.  
  
 Pour obtenir la liste des valeurs de **#define** qui identifient les fonctions ODBC, consultez les tableaux dans « Comments ».  
  
 *SupportedPtr*  
 Sortie  Si la fonction *FunctionId* identifie une seule fonction ODBC, *SupportedPtr* pointe vers une valeur SQLUSMALLINT unique qui est SQL_TRUE si la fonction spécifiée est prise en charge par le pilote, et SQL_FALSE si elle n’est pas prise en charge.  
  
 Si la fonction *FunctionId* est SQL_API_ODBC3_ALL_FUNCTIONS, *SupportedPtr* pointe vers un tableau SQLSMALLINT avec un nombre d’éléments égal à SQL_API_ODBC3_ALL_FUNCTIONS_SIZE. Ce tableau est traité par le gestionnaire de pilotes comme une image bitmap 4 000 bits qui peut être utilisée pour déterminer si une fonction ODBC 3 *. x* ou antérieure est prise en charge. La macro SQL_FUNC_EXISTS est appelée pour déterminer la prise en charge de la fonction. (Voir « commentaires ».) Une application ODBC 3 *. x* peut appeler **SQLGetFunctions** avec SQL_API_ODBC3_ALL_FUNCTIONS sur un pilote ODBC 3 *. x* ou ODBC 2 *. x* .  
  
 Si *FunctionId* est SQL_API_ALL_FUNCTIONS, *SupportedPtr* pointe vers un tableau SQLUSMALLINT de 100 éléments. Le tableau est indexé par **#define** valeurs utilisées par *FunctionId* pour identifier chaque fonction ODBC ; certains éléments du tableau sont inutilisés et réservés pour une utilisation ultérieure. Un élément est SQL_TRUE s’il identifie une fonction ODBC 2 *. x* ou antérieure prise en charge par le pilote. Elle est SQL_FALSE si elle identifie une fonction ODBC non prise en charge par le pilote ou n’identifie pas une fonction ODBC.  
  
 Les tableaux retournés dans **SupportedPtr* utilisent une indexation de base zéro.  
  
## <a name="returns"></a>retourne :  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLGetFunctions** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_DBC et un *handle* de *ConnectionHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLGetFunctions** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Error|Description|  
|--------|-----|-----------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) **SQLGetFunctions** a été appelé avant **SQLConnect**, **SQLBrowseConnect** ou **SQLDriverConnect**.<br /><br /> (DM) **SQLBrowseConnect** a été appelé pour *ConnectionHandle* et a retourné SQL_NEED_DATA. Cette fonction a été appelée avant que **SQLBrowseConnect** retourne SQL_SUCCESS_WITH_INFO ou SQL_SUCCESS.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *ConnectionHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY095|Type de fonction hors limites|(DM) une valeur *FunctionId* non valide a été spécifiée.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
  
## <a name="comments"></a>Commentaires  
 **SQLGetFunctions** retourne toujours que **SQLGetFunctions**, **SQLDataSources** et **SQLDrivers** sont pris en charge. Cela est dû au fait que ces fonctions sont implémentées dans le gestionnaire de pilotes. Le gestionnaire de pilotes mappe une fonction ANSI à la fonction Unicode correspondante si la fonction Unicode existe et mappe une fonction Unicode à la fonction ANSI correspondante si la fonction ANSI existe. Pour plus d’informations sur la façon dont les applications utilisent **SQLGetFunctions**, consultez [niveaux de conformité](../../../odbc/reference/develop-app/interface-conformance-levels.md)de l’interface.  
  
 La liste suivante répertorie les valeurs valides pour *FunctionId* pour les fonctions conformes au niveau de conformité ISO 92 conforme aux normes :  
  
|Valeur FunctionId|Valeur FunctionId|  
|----------|----------|  
|SQL_API_SQLALLOCHANDLE|SQL_API_SQLGETDESCFIELD|  
|SQL_API_SQLBINDCOL|SQL_API_SQLGETDESCREC|  
|SQL_API_SQLCANCEL|SQL_API_SQLGETDIAGFIELD|  
|SQL_API_SQLCLOSECURSOR|SQL_API_SQLGETDIAGREC|  
|SQL_API_SQLCOLATTRIBUTE|SQL_API_SQLGETENVATTR|  
|SQL_API_SQLCONNECT|SQL_API_SQLGETFUNCTIONS|  
|SQL_API_SQLCOPYDESC|SQL_API_SQLGETINFO|  
|SQL_API_SQLDATASOURCES|SQL_API_SQLGETSTMTATTR|  
|SQL_API_SQLDESCRIBECOL|SQL_API_SQLGETTYPEINFO|  
|SQL_API_SQLDISCONNECT|SQL_API_SQLNUMRESULTCOLS|  
|SQL_API_SQLDRIVERS|SQL_API_SQLPARAMDATA|  
|SQL_API_SQLENDTRAN|SQL_API_SQLPREPARE|  
|SQL_API_SQLEXECDIRECT|SQL_API_SQLPUTDATA|  
|SQL_API_SQLEXECUTE|SQL_API_SQLROWCOUNT|  
|SQL_API_SQLFETCH|SQL_API_SQLSETCONNECTATTR|  
|SQL_API_SQLFETCHSCROLL|SQL_API_SQLSETCURSORNAME|  
|SQL_API_SQLFREEHANDLE|SQL_API_SQLSETDESCFIELD|  
|SQL_API_SQLFREESTMT|SQL_API_SQLSETDESCREC|  
|SQL_API_SQLGETCONNECTATTR|SQL_API_SQLSETENVATTR|  
|SQL_API_SQLGETCURSORNAME|SQL_API_SQLSETSTMTATTR|  
|SQL_API_SQLGETDATA| |  
  
 La liste suivante répertorie les valeurs valides pour la fonction *FunctionId* pour les fonctions conformes au niveau de conformité de la norme Open Group :  
  
|Valeur FunctionId|Valeur FunctionId|  
|-|-|  
|SQL_API_SQLCOLUMNS|SQL_API_SQLSTATISTICS|  
|SQL_API_SQLSPECIALCOLUMNS|SQL_API_SQLTABLES|  
  
 La liste suivante répertorie les valeurs valides pour *FunctionId* pour les fonctions conformes au niveau de conformité des normes ODBC.  
  
|Valeur FunctionId|Valeur FunctionId|  
|-|-|  
|SQL_API_SQLBINDPARAMETER|SQL_API_SQLNATIVESQL|  
|SQL_API_SQLBROWSECONNECT|SQL_API_SQLNUMPARAMS|  
|SQL_API_SQLBULKOPERATIONS [1]|SQL_API_SQLPRIMARYKEYS|  
|SQL_API_SQLCOLUMNPRIVILEGES|SQL_API_SQLPROCEDURECOLUMNS|  
|SQL_API_SQLDESCRIBEPARAM|SQL_API_SQLPROCEDURES|  
|SQL_API_SQLDRIVERCONNECT|SQL_API_SQLSETPOS|  
|SQL_API_SQLFOREIGNKEYS|SQL_API_SQLTABLEPRIVILEGES|  
|SQL_API_SQLMORERESULTS| |  
  
 [1] lorsque vous utilisez un pilote ODBC 2 *. x* , **SQLBulkOperations** est retourné comme pris en charge uniquement si les deux conditions suivantes sont réunies : le pilote ODBC 2 *. x* prend en charge **SQLSetPos**, et le type d’information SQL_POS_OPERATIONS retourne le bit SQL_POS_ADD comme défini.  
  
 La liste suivante répertorie les valeurs valides pour *FunctionId* pour les fonctions introduites dans ODBC 3,8 ou version ultérieure :  
  
|Valeur FunctionId|  
|-|  
|SQL_API_SQLCANCELHANDLE [2]|  
  
 [2]   **SQLCancelHandle** sera renvoyé comme pris en charge uniquement si le pilote prend en charge à la fois **SQLCancel** et **SQLCancelHandle**. Si **SQLCancel** est pris en charge mais que **SQLCancelHandle** n’est pas, l’application peut toujours appeler **SQLCancelHandle** sur un descripteur d’instruction, car elle est mappée à **SQLCancel**.  
  
## <a name="sql_func_exists-macro"></a>Macro SQL_FUNC_EXISTS  
 La macro SQL_FUNC_EXISTS (*SupportedPtr*, *FunctionID*) est utilisée pour déterminer la prise en charge de ODBC 3 *. x* ou des fonctions antérieures après l’appel de **SQLGetFunctions** avec un argument *FunctionID* de SQL_API_ODBC3_ALL_FUNCTIONS. L’application appelle SQL_FUNC_EXISTS avec l’argument *SupportedPtr* défini sur le *SupportedPtr* passé dans *SQLGetFunctions*, et avec l’argument *FunctionID* défini sur la **#define** pour la fonction. SQL_FUNC_EXISTS retourne SQL_TRUE si la fonction est prise en charge, et SQL_FALSE dans le cas contraire.  
  
> [!NOTE]
>  Lorsque vous utilisez un pilote ODBC 2 *. x* , le gestionnaire de pilotes ODBC 3 *. x* retourne SQL_TRUE pour **SQLAllocHandle** et **SQLFreeHandle** , **car SQLAllocHandle** est mappé à **SQLAllocEnv**, **SQLAllocConnect** ou **SQLAllocStmt,**, et parce que **SQLFreeHandle** est mappé à **sqlfreeenv,**, **sqlfreeconnect,** ou **SQLFreeStmt**. **SQLAllocHandle** ou **SQLFreeHandle** avec un argument *comme HandleType* de SQL_HANDLE_DESC n’est pas pris en charge, bien que SQL_TRUE soit retourné pour les fonctions, car il n’existe aucune fonction ODBC 2 *. x* à mapper dans ce cas.  
  
## <a name="code-example"></a>Exemple de code  
 Les trois exemples suivants montrent comment une application utilise **SQLGetFunctions** pour déterminer si un pilote prend en charge **SQLTables**, **SQLColumns** et **SQLStatistics**. Si le pilote ne prend pas en charge ces fonctions, l’application se déconnecte du pilote. Le premier exemple appelle **SQLGetFunctions** une fois pour chaque fonction.  
  
```cpp  
SQLUSMALLINT TablesExists, ColumnsExists, StatisticsExists;  
RETCODE retcodeTables, retcodeColumns, retcodeStatistics  
  
retcodeTables = SQLGetFunctions(hdbc, SQL_API_SQLTABLES, &TablesExists);  
retcodeColumns = SQLGetFunctions(hdbc, SQL_API_SQLCOLUMNS, &ColumnsExists);  
retcodeStatistics = SQLGetFunctions(hdbc, SQL_API_SQLSTATISTICS, &StatisticsExists);  
  
// SQLGetFunctions is completed successfully and SQLTables, SQLColumns, and SQLStatistics are supported by the driver.  
if (retcodeTables == SQL_SUCCESS && TablesExists == SQL_TRUE &&   
retcodeColumns == SQL_SUCCESS && ColumnsExists == SQL_TRUE &&   
retcodeStatistics == SQL_SUCCESS && StatisticsExists == SQL_TRUE)   
{  
  
   // Continue with application  
  
}  
  
SQLDisconnect(hdbc);  
```  
  
 Dans le deuxième exemple, une application ODBC 3. x appelle **SQLGetFunctions** et lui transmet un tableau dans lequel **SQLGetFunctions** retourne des informations sur toutes les fonctions ODBC 3. x et antérieures.  
  
```cpp  
RETCODE retcodeTables, retcodeColumns, retcodeStatistics  
SQLUSMALLINT fExists[SQL_API_ODBC3_ALL_FUNCTIONS_SIZE];  
  
retcode = SQLGetFunctions(hdbc, SQL_API_ODBC3_ALL_FUNCTIONS, fExists);  
  
// SQLGetFunctions is completed successfully and SQLTables, SQLColumns, and SQLStatistics are supported by the driver.  
if (reccode == SQL_SUCCESS &&   
SQL_FUNC_EXISTS(fExists, SQL_API_SQLTABLES) == SQL_TRUE &&  
   SQL_FUNC_EXISTS(fExists, SQL_API_SQLCOLUMNS) == SQL_TRUE &&  
   SQL_FUNC_EXISTS(fExists, SQL_API_SQLSTATISTICS) == SQL_TRUE)   
{  
  
   // Continue with application  
  
}  
  
SQLDisconnect(hdbc);  
```  
  
 Le troisième exemple est une application ODBC 2. x qui appelle **SQLGetFunctions** et lui transmet un tableau d’éléments 100 dans lequel **SQLGetFunctions** retourne des informations sur toutes les fonctions ODBC 2. x et antérieures.  
  
```cpp  
#define FUNCTIONS 100  
  
RETCODE retcodeTables, retcodeColumns, retcodeStatistics  
SQLUSMALLINT fExists[FUNCTIONS];  
  
retcode = SQLGetFunctions(hdbc, SQL_API_ALL_FUNCTIONS, fExists);  
  
/* SQLGetFunctions is completed successfully and SQLTables, SQLColumns, and SQLStatistics are supported by the driver. */  
if (retcode == SQL_SUCCESS &&   
fExists[SQL_API_SQLTABLES] == SQL_TRUE &&  
   fExists[SQL_API_SQLCOLUMNS] == SQL_TRUE &&  
   fExists[SQL_API_SQLSTATISTICS] == SQL_TRUE)   
{  
  
   /* Continue with application */  
  
}  
  
SQLDisconnect(hdbc);  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Renvoi du paramètre d’un attribut de connexion|[Fonction SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Retour d’informations sur un pilote ou une source de données|[Fonction SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
|Renvoi du paramètre d’un attribut d’instruction|[Fonction SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
