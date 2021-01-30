---
description: Fonction SQLFreeStmt
title: SQLFreeStmt fonction) | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLFreeStmt
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLFreeStmt
helpviewer_keywords:
- SQLFreeStmt function [ODBC]
ms.assetid: 03408162-8b63-4470-90c4-e6c7d8d33892
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b0dcab0413f353b4c600964dd38818df37f86bfe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201556"
---
# <a name="sqlfreestmt-function"></a>Fonction SQLFreeStmt
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLFreeStmt** arrête le traitement associé à une instruction spécifique, ferme tous les curseurs ouverts associés à l’instruction, ignore les résultats en attente, ou libère éventuellement toutes les ressources associées au descripteur d’instruction.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLFreeStmt(  
     SQLHSTMT       StatementHandle,  
     SQLUSMALLINT   Option);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction  
  
 *Option*  
 Entrée L’une des options suivantes :  
  
 SQL_ CLOSE : ferme le curseur associé à *StatementHandle* (s’il a été défini) et ignore tous les résultats en attente. L’application peut rouvrir ce curseur ultérieurement en exécutant à nouveau une instruction **Select** avec des valeurs de paramètre identiques ou différentes. Si aucun curseur n’est ouvert, cette option n’a aucun effet sur l’application. **SQLCloseCursor** peut également être appelé pour fermer un curseur. Pour plus d’informations, consultez [fermeture du curseur](../../../odbc/reference/develop-app/closing-the-cursor.md).  
  
 SQL_DROP : cette option est déconseillée. Un appel à **SQLFreeStmt** avec une *option* de SQL_DROP est mappé dans le gestionnaire de pilotes à [SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md).  
  
 SQL_UNBIND : définit le champ SQL_DESC_COUNT de ARD sur 0, en libérant toutes les mémoires tampons de colonne liées par **SQLBindCol** pour le *StatementHandle* donné. Cela n’annule pas la liaison de la colonne de signet ; pour ce faire, le champ SQL_DESC_DATA_PTR de la colonne ARD de la colonne Bookmark a la valeur NULL. Notez que si cette opération est effectuée sur un descripteur explicitement alloué qui est partagé par plusieurs instructions, l’opération affecte les liaisons de toutes les instructions qui partagent le descripteur. Pour plus d’informations, consultez [vue d’ensemble de la récupération des résultats (de base)](../../../odbc/reference/develop-app/retrieving-results-basic.md).  
  
 SQL_RESET_PARAMS : définit le champ SQL_DESC_COUNT de APD sur 0, en libérant toutes les mémoires tampons de paramètres définies par **SQLBindParameter** pour le *StatementHandle* donné. Si cette opération est effectuée sur un descripteur explicitement alloué qui est partagé par plusieurs instructions, cette opération affecte les liaisons de toutes les instructions qui partagent le descripteur. Pour plus d’informations, consultez [liaison de paramètres](../../../odbc/reference/develop-app/binding-parameters-odbc.md).  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLFreeStmt** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLFreeStmt** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de **SQLFreeStmt** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avec l' *option* définie sur SQL_RESET_PARAMS avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY092|Type d’option hors limites|(DM) la valeur spécifiée pour l' *option* d’argument n’était pas :<br /><br /> SQL_CLOSE SQL_DROP SQL_UNBIND SQL_RESET_PARAMS|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 L’appel de **SQLFreeStmt** avec l’option SQL_CLOSE équivaut à appeler **SQLCloseCursor**, sauf que **SQLFreeStmt** avec SQL_CLOSE n’affecte pas l’application si aucun curseur n’est ouvert sur l’instruction. Si aucun curseur n’est ouvert, un appel à **SQLCloseCursor** retourne SQLState 24000 (état de curseur non valide).  
  
 Une application ne doit pas utiliser un descripteur d’instruction après qu’elle a été libérée ; le gestionnaire de pilotes ne vérifie pas la validité d’un descripteur dans un appel de fonction.  
  
## <a name="example"></a>Exemple  
 Il s’agit d’une bonne pratique de programmation pour libérer des handles. Toutefois, par souci de simplicité, l’exemple suivant n’inclut pas de code qui libère les descripteurs alloués. Pour obtenir un exemple illustrant comment libérer des handles, consultez la [fonction SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md).  
  
```cpp  
// SQLFreeStmt.cpp  
// compile with: user32.lib odbc32.lib  
#include <windows.h>  
#include <sqlext.h>  
  
int main() {  
   // declare and initialize the environment, connection, statement handles  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   SQLRETURN retCode;  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen;  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, -1);  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
   retCode = SQLDriverConnect(hdbc, desktopHandle, (SQLCHAR *)"Driver={SQL Server}", SQL_NTS, connStrbuffer, 1024 + 1, &connStrBufferLen, SQL_DRIVER_PROMPT);  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
   retCode = SQLFreeStmt(hstmt, SQL_UNBIND);  
   retCode = SQLFreeStmt(hstmt, SQL_RESET_PARAMS);  
}  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Allocation d’un descripteur|[SQLAllocHandle, fonction](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Fermeture d’un curseur|[SQLCloseCursor, fonction](../../../odbc/reference/syntax/sqlclosecursor-function.md)|  
|Libération d’un descripteur|[Fonction SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|Définition d’un nom de curseur|[SQLSetCursorName, fonction](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
