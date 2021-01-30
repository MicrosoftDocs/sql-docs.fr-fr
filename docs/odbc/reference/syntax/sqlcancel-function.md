---
description: SQLCancel, fonction
title: SQLCancel fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLCancel
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLCancel
helpviewer_keywords:
- SQLCancel function [ODBC]
ms.assetid: ac0b5972-627f-4440-8c5a-0e8da728726d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c1d630af340bf676deda8c18f415b85aba15403c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185803"
---
# <a name="sqlcancel-function"></a>SQLCancel, fonction
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLCancel** annule le traitement sur une instruction.  
  
 Pour annuler le traitement d’une connexion ou d’une instruction, utilisez la [fonction SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLCancel(  
     SQLHSTMT     StatementHandle);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLCancel** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLCancel** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par [SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md) dans l’argument *\* MessageText* buffer décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLCancel** .<br /><br /> L’opération d’annulation (DM) a échoué, car une opération asynchrone est en cours sur un handle de connexion associé à *StatementHandle*.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY018|Le serveur a refusé la demande d’annulation|Le serveur a refusé la demande d’annulation.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 **SQLCancel** peut annuler les types de traitement suivants sur une instruction :  
  
-   Une fonction s’exécutant de façon asynchrone sur l’instruction.  
  
-   Fonction sur une instruction qui a besoin de données.  
  
-   Fonction en cours d’exécution sur l’instruction sur un autre thread.  
  
 Dans ODBC 2. *x*, si une application appelle **SQLCancel** quand aucun traitement n’est effectué sur l’instruction, **SQLCancel** a le même effet que **SQLFreeStmt** avec l’option SQL_CLOSE ; ce comportement est défini uniquement pour l’exhaustivité et les applications doivent appeler **SQLFreeStmt** ou **SQLCloseCursor** pour fermer les curseurs.  
  
 Quand **SQLCancel** est appelé pour annuler une fonction qui s’exécute de façon asynchrone dans une instruction ou une fonction sur une instruction qui a besoin de données, les enregistrements de diagnostic publiés par la fonction en cours d’annulation sont effacés et **SQLCancel** publie ses propres enregistrements de diagnostic. quand **SQLCancel** est appelé pour annuler une fonction en cours d’exécution sur une instruction sur un autre thread, toutefois, il n’efface pas les enregistrements de diagnostic de la fonction annulée et ne publie pas ses propres enregistrements de diagnostic.  
  
## <a name="canceling-asynchronous-processing"></a>Annulation du traitement asynchrone  
 Une fois qu’une application a appelé une fonction de manière asynchrone, elle appelle la fonction à plusieurs reprises pour déterminer si elle a terminé le traitement. Si la fonction est toujours en cours de traitement, elle retourne SQL_STILL_EXECUTING. Si la fonction a terminé le traitement, elle retourne un code différent.  
  
 Après tout appel à la fonction qui retourne SQL_STILL_EXECUTING, une application peut appeler **SQLCancel** pour annuler la fonction. Si la demande d’annulation réussit, le pilote retourne SQL_SUCCESS. Ce message n’indique pas que la fonction a effectivement été annulée ; elle indique que la demande d’annulation a été traitée. Lorsque ou si la fonction est réellement annulée dépend du pilote et de la source de données. L’application doit continuer à appeler la fonction d’origine jusqu’à ce que le code de retour ne soit pas SQL_STILL_EXECUTING. Si la fonction a été annulée avec succès, le code de retour est SQL_ERROR et SQLSTATE HY008 (opération annulée). Si la fonction a terminé son traitement normal, le code de retour est SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO si la fonction a réussi ou SQL_ERROR et une SQLSTATE autre que HY008 (opération annulée) si la fonction a échoué.  
  
> [!NOTE]  
>  Dans ODBC 3,5, un appel à **SQLCancel** quand aucun traitement n’est effectué sur l’instruction n’est traité comme **SQLFreeStmt** avec l’option SQL_CLOSE, mais n’a aucun effet. Pour fermer un curseur, une application doit appeler **SQLCloseCursor**, et non **SQLCancel**.  
  
 Pour plus d’informations sur le traitement asynchrone, consultez [exécution asynchrone](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md).  
  
## <a name="canceling-functions-that-need-data"></a>Annulation de fonctions nécessitant des données  
 Une fois **SQLExecute** ou **SQLExecDirect** retourné SQL_NEED_DATA et avant l’envoi des données pour tous les paramètres de données en cours d’exécution, une application peut appeler **SQLCancel** pour annuler l’exécution de l’instruction. Une fois l’instruction annulée, l’application peut rappeler **SQLExecute** ou **SQLExecDirect** . Pour plus d’informations, consultez [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md).  
  
 Une fois que **SQLBulkOperations** ou **SQLSetPos** a retourné SQL_NEED_DATA et avant l’envoi des données pour toutes les colonnes de données en cours d’exécution, une application peut appeler **SQLCancel** pour annuler l’opération. Une fois l’opération annulée, l’application peut appeler de nouveau **SQLBulkOperations** ou **SQLSetPos** ; l’annulation n’affecte pas l’état du curseur ou la position actuelle du curseur. Pour plus d’informations, consultez [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md) ou [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
## <a name="canceling-functions-executing-on-another-thread"></a>Annulation de fonctions s’exécutant sur un autre thread  
 Dans une application multithread, l’application peut annuler une fonction qui s’exécute sur un autre thread. Pour annuler la fonction, l’application appelle **SQLCancel** avec le même descripteur d’instruction que celui utilisé par la fonction cible, mais sur un thread différent. Le mode d’annulation de la fonction dépend du pilote et du système d’exploitation. Comme pour l’annulation d’une fonction qui s’exécute de façon asynchrone, le code de retour de la **SQLCancel** indique uniquement si le pilote a correctement traité la demande. Seul SQL_SUCCESS ou SQL_ERROR peut être retourné ; aucune information de diagnostic n’est retournée. Si la fonction d’origine est annulée, elle retourne SQL_ERROR et SQLSTATE HY008 (opération annulée).  
  
 Si une instruction SQL est exécutée lorsque **SQLCancel** est appelé sur un autre thread pour annuler l’exécution de l’instruction, il est possible que l’exécution réussisse et retourne SQL_SUCCESS alors que l’annulation est également réussie. Dans ce cas, le gestionnaire de pilotes suppose que le curseur ouvert par l’exécution de l’instruction est fermé par l’annulation, de sorte que l’application ne peut pas utiliser le curseur.  
  
 Pour plus d’informations sur les threads, consultez [Multithreading](../../../odbc/reference/develop-app/multithreading.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à un paramètre|[Fonction SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Exécution d’opérations d’insertion ou de mise à jour en bloc|[SQLBulkOperations, fonction](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|Annule une fonction qui s’exécute de façon asynchrone sur un handle de connexion, en plus des fonctionnalités de **SQLCancel**.|[SQLCancelHandle, fonction](../../../odbc/reference/syntax/sqlcancelhandle-function.md)|  
|Exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécution d’une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Libération d’un descripteur d’instruction|[SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Obtention d’un champ d’un enregistrement de diagnostic ou d’un champ de l’en-tête de diagnostic|[Fonction SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)|  
|Obtention de plusieurs champs d’une structure de données de diagnostic|[SQLGetDiagRec, fonction](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)|  
|Retour du paramètre suivant pour l’envoi de données|[SQLParamData, fonction](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
|Envoi de données de paramètre au moment de l’exécution|[Fonction SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|  
|Positionnement du curseur dans un ensemble de lignes, actualisation des données dans l’ensemble de lignes ou mise à jour ou suppression de données dans le jeu de résultats|[SQLSetPos, fonction](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
