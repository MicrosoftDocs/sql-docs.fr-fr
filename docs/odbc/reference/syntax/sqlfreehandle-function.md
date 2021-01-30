---
description: Fonction SQLFreeHandle
title: SQLFreeHandle, fonction | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLFreeHandle
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLFreeHandle
helpviewer_keywords:
- SQLFreeHandle function [ODBC]
ms.assetid: 17a6fcdc-b05a-4de7-be93-a316f39696a1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 14c649c894250abdcac54010c0d18444d20b3baa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201567"
---
# <a name="sqlfreehandle-function"></a>Fonction SQLFreeHandle
**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLFreeHandle** libère les ressources associées à un environnement, une connexion, une instruction ou un handle de descripteur spécifique.  
  
> [!NOTE]
>  Cette fonction est une fonction générique permettant de libérer des handles. Il remplace les fonctions ODBC 2,0 **sqlfreeconnect,** (pour libérer un handle de connexion) et **sqlfreeenv,** (pour libérer un handle d’environnement). **Sqlfreeconnect,** et **sqlfreeenv,** sont déconseillés dans ODBC 3 *. x*. **SQLFreeHandle** remplace également la fonction ODBC 2,0 **SQLFreeStmt** (avec l' *option* SQL_DROP) pour la libération d’un descripteur d’instruction. Pour plus d’informations, consultez la section « commentaires ». Pour plus d’informations sur le mappage de cette fonction par le gestionnaire de pilotes lorsqu’une application ODBC 3 *. x* utilise un pilote ODBC 2 *. x* , consultez [mappage des fonctions de remplacement pour la compatibilité descendante des applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLFreeHandle(  
     SQLSMALLINT   HandleType,  
     SQLHANDLE     Handle);  
```  
  
## <a name="arguments"></a>Arguments  
 *HandleType*  
 Entrée Type de descripteur à libérer par **SQLFreeHandle**. Il doit s’agir de l’une des valeurs suivantes :   
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 SQL_HANDLE_DBC_INFO_TOKEN descripteur est utilisé uniquement par le gestionnaire de pilotes et le pilote. Les applications ne doivent pas utiliser ce type de handle. Pour plus d’informations sur la SQL_HANDLE_DBC_INFO_TOKEN, consultez [développement de la sensibilisation aux Connection-Pool dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).  
  
 Si *comme HandleType* n’est pas l’une de ces valeurs, **SQLFreeHandle** retourne SQL_INVALID_HANDLE.  
  
 *Handle*  
 Entrée Handle à libérer.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
 Si **SQLFreeHandle** retourne SQL_ERROR, le handle est toujours valide.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLFreeHandle** retourne SQL_ERROR, une valeur SQLSTATE associée peut être obtenue à partir de la structure de données de diagnostic pour le handle que **SQLFreeHandle** a tenté de libérer, mais n’a pas pu le faire. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLFreeHandle** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) l’argument *comme HandleType* a été SQL_HANDLE_ENV, et au moins une connexion était dans un État alloué ou connecté. **SQLDisconnect** et **SQLFreeHandle** avec un *comme HandleType* de SQL_HANDLE_DBC doivent être appelés pour chaque connexion avant d’appeler **SQLFreeHandle** avec un *comme HandleType* de SQL_HANDLE_ENV.<br /><br /> (DM) l’argument *comme HandleType* a été SQL_HANDLE_DBC, et la fonction a été appelée avant l’appel de **SQLDisconnect** pour la connexion.<br /><br /> (DM) l’argument *comme HandleType* a été SQL_HANDLE_DBC. Une fonction d’exécution asynchrone a été appelée avec un *handle* et la fonction était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) l’argument *comme HandleType* a été SQL_HANDLE_STMT. **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé avec le descripteur d’instruction et retournés SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.<br /><br /> (DM) l’argument *comme HandleType* a été SQL_HANDLE_STMT. Une fonction d’exécution asynchrone a été appelée sur le descripteur d’instruction ou sur le handle de connexion associé et la fonction était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) l’argument *comme HandleType* a été SQL_HANDLE_DESC. Une fonction d’exécution asynchrone a été appelée sur le handle de connexion associé ; et la fonction était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) tous les descripteurs subsidiaires et autres ressources n’ont pas été libérés avant l’appel de **SQLFreeHandle** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour l’un des descripteurs d’instruction associés au *Handle* et *comme HandleType* a été défini sur SQL_HANDLE_STMT ou SQL_HANDLE_DESC a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.|  
|HY013|Erreur de gestion de la mémoire|L’argument *comme HandleType* a été SQL_HANDLE_STMT ou SQL_HANDLE_DESC, et l’appel de fonction n’a pas pu être traité parce que les objets mémoire sous-jacents n’ont pas pu être accédés, probablement en raison de conditions de mémoire insuffisante.|  
|HY017|Utilisation non valide d’un handle de descripteur alloué automatiquement.|(DM) l' *argument descripteur* a été défini sur le handle d’un descripteur alloué automatiquement.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) l’argument *comme HandleType* a été SQL_HANDLE_DESC, et le pilote était un pilote ODBC 2 *. x* .<br /><br /> (DM) l’argument *comme HandleType* a été SQL_HANDLE_STMT, et le pilote n’était pas un pilote ODBC valide.|  
  
## <a name="comments"></a>Commentaires  
 **SQLFreeHandle** est utilisé pour libérer des handles pour les environnements, les connexions, les instructions et les descripteurs, comme décrit dans les sections suivantes. Pour obtenir des informations générales sur les descripteurs, consultez [Handles](../../../odbc/reference/develop-app/handles.md).  
  
 Une application ne doit pas utiliser de handle après qu’elle a été libérée ; le gestionnaire de pilotes ne vérifie pas la validité d’un descripteur dans un appel de fonction.  
  
## <a name="freeing-an-environment-handle"></a>Libération d’un descripteur d’environnement  
 Avant d’appeler **SQLFreeHandle** avec un *comme HandleType* de SQL_HANDLE_ENV, une application doit appeler **sqlfreehandle** avec un *comme HandleType* de SQL_HANDLE_DBC pour toutes les connexions allouées dans l’environnement. Dans le cas contraire, l’appel à **SQLFreeHandle** retourne SQL_ERROR et l’environnement et toute connexion active restent valides. Pour plus d’informations, consultez [Handles d’environnement](../../../odbc/reference/develop-app/environment-handles.md) et [allocation du handle d’environnement](../../../odbc/reference/develop-app/allocating-the-environment-handle.md).  
  
 Si l’environnement est un environnement partagé, l’application qui appelle **SQLFreeHandle** avec un *comme HandleType* de SQL_HANDLE_ENV n’a plus accès à l’environnement après l’appel, mais les ressources de l’environnement ne sont pas nécessairement libérées. L’appel à **SQLFreeHandle** décrémente le décompte de références de l’environnement. Le nombre de références est géré par le gestionnaire de pilotes. S’il n’atteint pas zéro, l’environnement partagé n’est pas libéré, car il est toujours utilisé par un autre composant. Si le nombre de références atteint zéro, les ressources de l’environnement partagé sont libérées.  
  
## <a name="freeing-a-connection-handle"></a>Libération d’un descripteur de connexion  
 Avant d’appeler **SQLFreeHandle** avec un *comme HandleType* de SQL_HANDLE_DBC, une application doit appeler **SQLDisconnect** pour la connexion s’il existe une connexion sur ce handle *.* Dans le cas contraire, l’appel à **SQLFreeHandle** retourne SQL_ERROR et la connexion reste valide.  
  
 Pour plus d’informations, consultez [Handles de connexion](../../../odbc/reference/develop-app/connection-handles.md) et [déconnexion à partir d’une source de données ou d’un pilote](../../../odbc/reference/develop-app/disconnecting-from-a-data-source-or-driver.md).  
  
## <a name="freeing-a-statement-handle"></a>Libération d'un descripteur d'instruction  
 Un appel à **SQLFreeHandle** avec un *comme HandleType* de SQL_HANDLE_STMT libère toutes les ressources allouées par un appel à **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_STMT. Quand une application appelle **SQLFreeHandle** pour libérer une instruction qui a des résultats en attente, les résultats en attente sont supprimés. Lorsqu’une application libère un handle d’instruction, le pilote libère les quatre descripteurs alloués automatiquement associés à ce handle. Pour plus d’informations, consultez [Handles d’instruction](../../../odbc/reference/develop-app/statement-handles.md) et [libération d’un descripteur d’instruction](../../../odbc/reference/develop-app/freeing-a-statement-handle-odbc.md).  
  
 Notez que **SQLDisconnect** supprime automatiquement toutes les instructions et descripteurs ouverts sur la connexion.  
  
## <a name="freeing-a-descriptor-handle"></a>Libération d’un handle de descripteur  
 Un appel à **SQLFreeHandle** avec un *comme HandleType* de SQL_HANDLE_DESC libère le handle de descripteur dans *handle*. L’appel à **SQLFreeHandle** ne libère pas la mémoire allouée par l’application qui peut être référencée par un champ de pointeur (y compris SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR et SQL_DESC_OCTET_LENGTH_PTR) d’un enregistrement de descripteur de *handle*. La mémoire allouée par le pilote pour les champs qui ne sont pas des champs de pointeur est libérée quand le handle est libéré. Lorsqu’un handle de descripteur alloué par l’utilisateur est libéré, toutes les instructions que le handle libéré a été associé à reprennent leurs handles de descripteurs alloués automatiquement respectifs.  
  
> [!NOTE]
>  Les pilotes ODBC 2 *. x* ne prennent pas en charge la libération des handles de descripteur, tout comme ils ne prennent pas en charge l’allocation des handles de descripteur  
  
 Notez que **SQLDisconnect** supprime automatiquement toutes les instructions et descripteurs ouverts sur la connexion. Lorsqu’une application libère un handle d’instruction, le pilote libère tous les descripteurs générés automatiquement associés à ce handle.  
  
 Pour plus d’informations sur les descripteurs, consultez [descripteurs](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="code-example"></a>Exemple de code  
 Pour obtenir des exemples de code supplémentaires, consultez [SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md) et [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
### <a name="code"></a>Code  
  
```cpp  
// SQLFreeHandle.cpp  
// compile with: user32.lib odbc32.lib  
#include <windows.h>  
#include <sqlext.h>  
#include <stdio.h>  
  
int main() {  
   SQLRETURN retCode;  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen;  
  
   // Initialize the environment, connection, statement handles.  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   // Allocate the environment.  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set environment attributes.  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, -1);  
  
   // Allocate the connection.  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
  
   // Set the login timeout.  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
  
   // Let the user select the data source and connect to the database.  
   retCode = SQLDriverConnect(hdbc, desktopHandle, (SQLCHAR *)"Driver={SQL Server}", SQL_NTS, connStrbuffer, 1025, &connStrBufferLen, SQL_DRIVER_PROMPT);  
  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   // Free handles, and disconnect.     
   if (hstmt) {   
      SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
      hstmt = NULL;   
   }  
   if (hdbc) {   
      SQLDisconnect(hdbc);  
      SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
      hdbc = NULL;   
   }  
   if (henv) {   
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
      henv = NULL;   
   }  
}  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Allocation d’un descripteur|[SQLAllocHandle, fonction](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Annulation du traitement des instructions|[SQLCance fonction)](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Définition d’un nom de curseur|[SQLSetCursorName, fonction](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Exemple de programme ODBC](../../../odbc/reference/sample-odbc-program.md)
